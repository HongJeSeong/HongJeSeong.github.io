---
layout: post
title: "기존 태양광 모니터링 시스템과 ARTIK Cloud 연동"
date: 2018-02-1 18:20
category: projects
comments: false
tag:
- Artik
- Linux
- Java
- js
---

# 기존 태양광 모니터링 시스템과 ARTIK Cloud 연동 구현 
## ARTIK 710 부분만 업로드
###### 기존의 태양광 모니터링 시스템은 단순히 발전소의 인버터와의 통신, 발전량등의 상태만을 확인한다. 앞으로는 에너지를 효율적으로 사용하기 위해 단순 모니터링이 아닌 에너지 소비량 확인과 에너지를 과소비 하는 전자제품을 제어하는 기능이 필요하다. 이러한 기능을 위해, 기존의 태양광 시스템에 IoT 플랫폼인 삼성 ARTIK 메카니즘을 적용한다. 기존 시스템에 1) 기존 태양광 인버터에서 일사량과 발전량 데이터와 2) “소비 전력 측정 데이터”등등을 받아서, ARTIK 710 모듈을 통해 클라우드에 전송하는 프로그램이다. 즉 소비전력과 태양광 데이터를 수집하여, 이를 전처리 과정 후에 실시간으로 ARTIK 클라우드로 보내는 기능을 구현하였다.  


### 전체 구성도
![전체 구성도](https://raw.githubusercontent.com/HongJeSeong/HongSolarARTIK/master/artikWhole.PNG)

- IoT에 적합한 통신 방법중 실시간 데이터 전송을 위하여 WebSocket을 사용
- Artik 053 으로 전력 측정
- 안드로이드 OS로 모바일 사용

**코드수정 필요**
###### monitor.js
{% highlight js %}
'use strict';
require('date-utils');
var ConfigWSParameters = require("./config.json");
var WebSocket = require("ws");
var ConfigWSConnection = {

    "scheme": "wss",
    "domain": "api.artik.cloud",
    "version": "v1.1",
    "path": "live"

};

function getConnectionParameters(config) {

    return "authorization=bearer+" + config.userToken + "&" +
                "sdids=" + config.deviceID;
}

function getConnectionString(config, parameters) {

    var connectionString =
            config.scheme + "://" +
            config.domain + "/" +
            config.version + "/" +
            config.path + "?" +
            getConnectionParameters(parameters);

    console.log("Connecting to: ", connectionString);

    return connectionString;

}
/*DB*/
var sqlite3 = require('sqlite3').verbose();
var db = new sqlite3.Database('053db.db');

/*create connection*/
var ws = new WebSocket(
    getConnectionString(ConfigWSConnection, ConfigWSParameters));

/*listen*/
ws.on("message", function (data) {
    console.log("Received message with data: %s\n", data);
var ping=data.indexOf("ping");
var error=data.indexOf("error");
var rssi=data.indexOf("rssi");

if(ping==-1)
if(error==-1)
if(rssi!=-1)
  db.serialize(function() {
  db.run("CREATE TABLE if not exists data (rssi double, time TEXT)");

  var resr=data.substring(rssi+6,rssi+11);
  var out=parseFloat(resr);
  var stmt = db.prepare("INSERT INTO data VALUES (?,?)");
  var d = new Date();
  var n = d.toFormat('YYYY-MM-DD HH24:MI:SS');
      stmt.run(-out,n);
      stmt.finalize();

  db.each("SELECT rssi, time FROM data", function(err, row) {
      console.log(row.rssi + ": " + row.time+"[receive data]");
  });
});

});

ws.on("open", function () {
    console.log("Websocket connection is open ...");
});

ws.on("close", function () {
    console.log("Websocket connection is closed ...");
    db.close();
});


{% endhighlight %}

###### send.js
{% highlight js %}
require('date-utils');
var ConfigWSRegistrationInfo = require('./config.json');

var sqlite3 = require('sqlite3').verbose();
var useDb = new sqlite3.Database('/workspace/websocket/receiver/053db.db');
var inverterDb = new sqlite3.Database('/hongsolar/hsEnergyData.db');

var WebSocket = require('ws');
const websocketUrl = "wss://api.artik.cloud/v1.1/websocket?ack=true";


console.log("Connecting to url: ", websocketUrl);
var ws = new WebSocket(websocketUrl);
var init=0;
        var accumulated_gen;
        var accumulated_use;
        var co_dec;
        var hanjeon;
        var month_gen;
        var month_use;
        var now_gen;
        var now_use;
        var today_gen;
        var today_use;
        var time_use;
    var todayTime = "";
    var monthTime = "";



ws.on('open', function() {
    console.log('WebSocket connection is open ...');
    sendRegistrationMessage();
});


ws.on('message', function(data, flags) {
    console.log('Received message: %s\n', data);

    var message = JSON.parse(data);

    if (message.type === 'action') {
        //Á¦¾î¸¦ À§ÇÑ ÄÚµå
    }

    if (message.type === 'ping') {

  var d = new Date();
  var n = d.toFormat('YYYY-MM-DD HH24:MI:SS');
  var time1 = n.substring(0,14);
  var time2 = n.substring(0,15);
  var like1 = "%"+time1+"%";
  var like2 = "%"+time2+"%";
console.log(n);
 useDb.each('SELECT * FROM data WHERE time LIKE ?',[like2],
 function(err, row) {
   now_use=row.rssi;
   var realUse = now_use / 360;
   time_use=row.time;

   if(monthTime != "" && monthTime == time_use.substring(5,6)) {
      month_use += today_use;
   } else if(monthTime != "" && monthTime != time_use.substring(5,6)) {
      month_use = 0;
   }

   if(todayTime != "" && todayTime == time_use.substring(8,9)) {
      today_use += realUse;
      accumulated_use += realUse;
   } else if(todayTime != "" && todayTime != time_use.substring(8,9)) {
      today_use = 0;
   }

    console.log(row);
  });
inverterDb.each('SELECT * FROM virtual_inverter WHERE time LIKE ?',[like1]
,
 function(err, row) {
        accumulated_gen=row.ACCUMULATED_GEN;
        co_dec=row.CO_DEC;
        hanjeon=row.HANJEON;
        month_gen=row.MONTH_GEN;
        now_gen=row.NOW_GEN;
        today_gen=row.TODAY_GEN;
    console.log(row);
    console.log(row.HANJEON);
  });
if(init==1){
  updateDeviceField({"accumulated_gen":accumulated_gen,"accumulated_use":accumulated_use,
"co_dec":co_dec,"hanjeon":hanjeon,
"month_gen":month_gen,"month_use":month_use,"now_gen":now_gen,"now_use":now_use,
"today_gen":today_gen,"today_use":today_use});
 }
 init=1;
}
});


ws.on('close', function() {
    console.log('Websocket connection is closed ...');
        useDb.close();
        inverterDb.close();
});



function sendRegistrationMessage() {
    var payload = {
        'type': 'register',
        'sdid': ConfigWSRegistrationInfo.deviceID,
        'authorization': 'bearer ' + ConfigWSRegistrationInfo.deviceToken,
        'cid': getTimeMillis(),
    };

    console.log(
        'Sending register message payload: %s',
        JSON.stringify(payload));

    sendMessage(payload);

}

function updateDeviceField(data) {
    var payload = {
        'sdid': ConfigWSRegistrationInfo.deviceID,
        'data': data,
        'cid': getTimeMillis(),
    };

    sendMessage(payload, 'Send message and update field:');
}

function sendMessage(payload, prefix) {
    console.log(prefix, JSON.stringify(payload));

    ws.send(JSON.stringify(payload), {
        binary: false,
        mask: true,
    });
}


function getTimeMillis() {
    return parseInt(Date.now().toString());
}
{% endhighlight %}

[github source](https://github.com/HongJeSeong/HongSolarARTIK)
