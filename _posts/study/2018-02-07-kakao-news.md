---
layout: post
title: "카카오 문제(뉴스 클러스터링)"
date: 2018-02-07 16:34
category: posts
comments: false
image: "../../images/posts/algokakao/jaccard1.png"
tag:
- algorithm
---
# 카카오 문제(뉴스 클러스터링)
### 알콜리즘
### 카카오 신입 공채 1차 코딩 테스트 문제 (난이도: 중)

여러 언론사에서 쏟아지는 뉴스, 특히 속보성 뉴스를 보면 비슷비슷한 제목의 기사가 많아 정작 필요한 기사를 찾기가 어렵다. Daum 뉴스의 개발 업무를 맡게 된 신입사원 튜브는 사용자들이 편리하게 다양한 뉴스를 찾아볼 수 있도록 문제점을 개선하는 업무를 맡게 되었다.

개발의 방향을 잡기 위해 튜브는 우선 최근 화제가 되고 있는 "카카오 신입 개발자 공채" 관련 기사를 검색해보았다.

- 카카오 첫 공채..'블라인드' 방식 채용
- 카카오, 합병 후 첫 공채.. 블라인드 전형으로 개발자 채용
- 카카오, 블라인드 전형으로 신입 개발자 공채
- 카카오 공채, 신입 개발자 코딩 능력만 본다
- 카카오, 신입 공채.. "코딩 실력만 본다"
- 카카오 "코딩 능력만으로 2018 신입 개발자 뽑는다"

기사의 제목을 기준으로 "블라인드 전형"에 주목하는 기사와 "코딩 테스트"에 주목하는 기사로 나뉘는 걸 발견했다. 튜브는 이들을 각각 묶어서 보여주면 카카오 공채 관련 기사를 찾아보는 사용자에게 유용할 듯싶었다.

유사한 기사를 묶는 기준을 정하기 위해서 논문과 자료를 조사하던 튜브는 "자카드 유사도"라는 방법을 찾아냈다.

자카드 유사도는 집합 간의 유사도를 검사하는 여러 방법 중의 하나로 알려져 있다. 두 집합 `A`, `B` 사이의 자카드 유사도 `J(A, B)`는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의된다.

예를 들어 집합 `A = {1, 2, 3}`, 집합 `B = {2, 3, 4}`라고 할 때, 교집합 `A ∩ B = {2, 3}`, 합집합 `A ∪ B = {1, 2, 3, 4}`이 되므로, 집합 A, B 사이의 자카드 유사도 `J(A, B) = 2/4 = 0.5`가 된다. 집합 A와 집합 B가 모두 공집합일 경우에는 나눗셈이 정의되지 않으니 따로 `J(A, B) = 1`로 정의한다.

자카드 유사도는 원소의 중복을 허용하는 다중집합에 대해서 확장할 수 있다. 다중집합 A는 원소 "1"을 3개 가지고 있고, 다중집합 B는 원소 "1"을 5개 가지고 있다고 하자. 이 다중집합의 교집합 `A ∩ B`는 원소 "1"을 min(3, 5)인 3개, 합집합 `A ∪ B`는 원소 "1"을 max(3, 5)인 5개 가지게 된다. 다중집합 `A = {1, 1, 2, 2, 3}`, 다중집합 `B = {1, 2, 2, 4, 5}`라고 하면, 교집합 `A ∩ B = {1, 2, 2}`, 합집합 `A ∪ B = {1, 1, 2, 2, 3, 4, 5}`가 되므로, 자카드 유사도 `J(A, B) = 3/7`, 약 `0.42`가 된다.

이를 이용하여 문자열 사이의 유사도를 계산하는데 이용할 수 있다. 문자열 "FRANCE"와 "FRENCH"가 주어졌을 때, 이를 두 글자씩 끊어서 다중집합을 만들 수 있다. 각각 {FR, RA, AN, NC, CE}, {FR, RE, EN, NC, CH}가 되며, 교집합은 {FR, NC}, 합집합은 {FR, RA, AN, NC, CE, RE, EN, CH}가 되므로, 두 문자열 사이의 자카드 유사도 `J("FRANCE", "FRENCH") = 2/8 = 0.25`가 된다.

### 입력 형식

- 입력으로는 `str1`과 `str2`의 두 문자열이 들어온다. 각 문자열의 길이는 2 이상, 1,000 이하이다.
- 입력으로 들어온 문자열은 두 글자씩 끊어서 다중집합의 원소로 만든다. 이때 영문자로 된 글자 쌍만 유효하고, 기타 공백이나 숫자, 특수 문자가 들어있는 경우는 그 글자 쌍을 버린다. 예를 들어 "ab+"가 입력으로 들어오면, "ab"만 다중집합의 원소로 삼고, "b+"는 버린다.
- 다중집합 원소 사이를 비교할 때, 대문자와 소문자의 차이는 무시한다. "AB"와 "Ab", "ab"는 같은 원소로 취급한다.

### 출력 형식

입력으로 들어온 두 문자열의 자카드 유사도를 출력한다. 유사도 값은 0에서 1 사이의 실수이므로, 이를 다루기 쉽도록 65536을 곱한 후에 소수점 아래를 버리고 정수부만 출력한다.

### 예제 입출력

| str1 | str2 | answer |
| ---- | ---- | ------ |
| FRANCE | french | 16384 |
| handshake | shake hands | 65536 |
| aa1+aa2 | AAAA12 | 43690 |
| E=M*C^2 | e=m*c^2 | 65536 |   


<br>
### 문제 해설

이 문제는 자카드 유사도를 설명해주고 자카드 유사도를 직접 계산하는 프로그램을 작성하는 문제입니다. 자카드 유사도는 실무에서도 유사한 문서를 판별할 때 주로 쓰이는데요, 몰랐더라도 문제에서 자세히 설명해주기 때문에 이해하는데 어려움은 없었을 거 같습니다. 공식은 매우 간단한데요, 교집합을 합집합으로 나눈 수입니다. 다만, 이 값은 0에서 1 사이의 실수가 되는데, 여기서는 이를 다루기 쉽도록 65536을 곱한 후 소수점 아래를 버리고 정수부만 취하도록 합니다.

문제 설명은 원소의 중복을 허용하는 다중집합<sup>multiset</sup>으로 되어 있는데, 자주 접하는 자료구조가 아니고, 일부 언어에서는 기본으로 제공하는 자료구조가 아니라 어려워하는 분들이 있었습니다. 하지만 다중집합 자료구조를 쓰지 않더라도, 각 원소를 정렬된 배열에 넣은 후 병합 정렬<sup>Merge sort</sup>에서 배웠던 코드를 응용, 어렵지 않게 합집합과 교집합 함수를 직접 구현할 수도 있습니다.

다중집합의 교집합, 합집합만 잘 구해낸다면 이 문제는 어렵지 않게 풀 수 있으며, 다만 집합 A와 B가 모두 공집합일 경우에는 나눗셈이 정의되지 않으므로<sup>division by zero</sup> 따로 `J(A,B) = 1`로 정의합니다. 즉, 65536을 곱하면 이 경우 `1 * 65536 = 65536`이 정답이 됩니다. 예제 입출력에도 합집합이 공집합인 경우가 포함되어 있으므로 이 경우만 주의한다면 쉽게 풀 수 있는 문제입니다.

_**이 문제의 정답률은 41.84%입니다.**_

**[문제 출처](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)**

------------------------------------------------------

## 해결 방법  
**코드의 전체적인 기능**  
단어를 입력받아 한글자 단위로 쪼개어 배열에 저장한다. 이렇게 쪼개어진 총 n자의 단어를 2자씩 끊기위해 n-1번의 비교를 수행한다. 이 비교를 수행하면서 n번째 단어와 n+1번째의 단어를 하나로 묶고 특수문자, 숫자, 공백을 제거하였을때 단어의 길이가 2개가 되지 않는다면 유효한 글자 단위가 아니기 때문에 버리고 유효하다면 배열리스트에 추가를 한다. 자카드 유사도를 계산한다!  

  
_218-02-13  교집합 공집합 추가_      
 - 교집합: 단순히 이중 `for`문으로 두 집합을 비교하여 같은 원소가 있다면 한 집합에서 제거를 한다. 남은 것이 교집합 원소가 된다.
 - 합집합: 위에서 제거 후 남은 집합과 다른 집합 전체가 합집합이다.
  
![과정사진](../../images/posts/algokakao/jaccard1.png)

##### 소스코드
{% highlight java %}
package week1;

import java.util.ArrayList;

public class JaccardSimilarity {

	public ArrayList<String> preprocess(String data) {
		String temp = "";
		ArrayList<String> stringList = new ArrayList<String>();

		data = data.toUpperCase();// 대문자 변환
		String[] stringSplit = data.split("");// 쪼개기
		for (int i = 0; i < stringSplit.length - 1; i++) {// 유효한 것만 합치기
			temp = stringSplit[i].concat(stringSplit[i + 1]);
			if (validation(temp) == true) {
				stringList.add(temp);
			}
		}
		return stringList;
	}

	public boolean validation(String data) { // 유효성검사
		data = data.replaceAll("[^\uAC00-\uD7A3xfe0-9a-zA-Z\\s]", "");// 특수문자 제거
		data = data.replaceAll("[0-9]", "");// 숫자제거
		data = data.replaceAll(" ", "");// 공백제거
		if (data.length() != 2) { // 두 글자 쌍
			return false;
		}
		return true;
	}

	public void compare(String data1, String data2) {
		if (!((data1.length() >= 2 && data1.length() <= 1000) && (data2.length() >= 2 && data2.length() <= 1000))) {
			System.out.println("입력 형식에 맞지 않습니다.");
		} else {
			float intersection = 0, union = 0, result = 0;
			ArrayList<String> list1 = new ArrayList<String>();
			ArrayList<String> list2 = new ArrayList<String>();
			list1 = preprocess(data1);
			list2 = preprocess(data2);
			if (list1.size() == 0 && list2.size() == 0) { // 공집합인 경우
				result = 1 * 65536;
				System.out.println("유사도 =" + (int) result);

			} else {
				for (int i = 0; i < list1.size(); i++) {
					String value = list1.get(i);// 성능위해 cast 남발 줄이기...
					for (int j = 0; j < list2.size(); j++) {
						if (value.equals(list2.get(j))) {
							list2.remove(j);
							intersection++;// 교집합
							break;
						}
					}
				}
				union = list1.size() + list2.size();// 합집합
				result = (intersection / union) * 65536;
				System.out.println("유사도 =" + (int) result);
			}
		}
	}

	public static void main(String[] args) {
		JaccardSimilarity jaccard = new JaccardSimilarity();
		jaccard.compare("FRANCE", "french");
		jaccard.compare("handshake", "shake hands");
		jaccard.compare("aa1+aa2", "AAAA12");
		jaccard.compare("e=m*c^2", "E=M*C^2");
	}

}

{% endhighlight %}  
## 결과
![결과사진](../../images/posts/algokakao/jaccard.png)
