---
layout: post
title: "카카오 문제(프렌즈4블록)"
date: 2018-02-09 13:38
category: posts
comments: false
image: "http://t1.kakaocdn.net/welcome2018/pang1.png"
tag:
- algorithm
---
## 6. 프렌즈4블록(난이도: 상)

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".  
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

<img src="http://t1.kakaocdn.net/welcome2018/pang1.png" />

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

<img src="http://t1.kakaocdn.net/welcome2018/pang2.png" />

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

<img src="http://t1.kakaocdn.net/welcome2018/pang3.png" />

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

<img src="http://t1.kakaocdn.net/welcome2018/pang4.png" />

위 초기 배치를 문자로 표시하면 아래와 같다.

```
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

### 입력 형식

- 입력으로 판의 높이 `m`, 폭 `n`과 판의 배치 정보 `board`가 들어온다.
- 2 ≦ `n`, `m` ≦ 30
- `board`는 길이 `n`인 문자열 `m`개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### 출력 형식

입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

### 입출력 예제

| m | n | board | answer |
| -- | -- | --- | ------ |
| 4 | 5 | ["CCBDE", "AAADE", "AAABF", "CCBBF"] | 14 |
| 6 | 6 | ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"] | 15 |

### 예제에 대한 설명

- 입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.
- 입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.

### 문제 해설

게임 요구 사항을 구현해보는 문제입니다. 같은 모양의 카카오프렌즈 블록이 2x2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임인데요. 인접한 모든 블록이 사라지는 실제 게임들과 달리 계산을 쉽게 하기 위해 2x2로 제한하고, 사라진 블록 자리에는 새로운 블록이 채워지지 않습니다. 그럼에도 불구하고 인접한 블록을 모두 스캔해야 하는 문제라 짧지 않은 코드가 필요했을 것 같네요. 이번 시험에서 가장 긴 코드가 필요한 문제였습니다. 자바의 경우 무려 80라인이나 필요했네요. 블록 매트릭스를 생성하여 스캔하고 제거해 나가는 작업을 반복하면서 더 이상 제거되지 않을 때 사라진 블록 자리의 수를 계산하면 됩니다.

__이 문제의 정답률은 48.01%입니다.__

--------------------------------------------
## 해결 
코드의 양은 많지만.. 개인적으로 카카오 5번 자카드 유사도 문제보다는 생각하는게 단순하여 풀기는 쉬웠던 것 같다.  
- 받은 값을 배열에 세팅한다.**init()**
- 배열의 순서대로 인접한 2 x 2 행렬의 값을 비교탐색(소문자인 경우도 같이)한다. 2 x 2 행렬이 모두 같은 값이라면, 소문자로 변환한다. **search()**
- 탐색이 끝나면 소문자로 변경된 값을 `-` 로 변경한다. **punch()**
- 다시 배열의 열을 탐색하며 `-` 가 아닌 값을 행렬 위로 몰아 넣는다. **relocate()**
###### 처음 생각했던 행렬 원소 탐색 순서
![search사진]("../../images/posts/algokakao/search.png")
__행렬을 탐색할 때 어떻게 하면 빨리 탐색할까 생각하며 코딩했지만, 코드를 다 짜고 생각하니 별 의미 없는 방법이었음 __

{% highlight java%}
public class Friends4 {
	int m, n;
	String[] board;
	String[][] field;
	int result = 0;

	public Friends4(int m, int n, String[] board) {
		this.m = m;
		this.n = n;
		this.board = board;
		field = new String[m][n];

		init();
		print();
		while (search()) {
			punch();
			relocate();
			print();
		}
		System.out.println(result);
	}

	public void init() {
		for (int i = 0; i < m; i++) {// 배열 초기 배치
			String[] data = board[i].split("");
			for (int j = 0; j < n; j++) {
				field[i][j] = data[j];
			}
		}
	}

	public boolean search() {
		boolean flag = false;
		for (int i = 0; i < m - 1; i++) {
			for (int j = 0; j < n - 1; j++) {
				String value_1 = field[i][j];
				if (!value_1.equals("-")) {
					if (value_1.equals(field[i + 1][j + 1]) || value_1.equals((field[i + 1][j + 1]).toLowerCase())) {
						if (value_1.equals(field[i][j + 1]) || value_1.equals((field[i][j + 1]).toLowerCase())) {
							if (value_1.equals(field[i + 1][j]) || value_1.equals((field[i + 1][j]).toLowerCase())) {
								mark(i, j);
								flag = true;
							}
						}
					}

				}
			}
		}
		return flag;
	}

	public void relocate() {
		String temp = "";
		for (int i = 0; i < n; i++) {
			for (int j = m - 2; j >= 0; j--) {
				if (!field[j][i].equals("-") && field[j + 1][i].equals("-")) {
					temp = field[j][i];
					field[j][i] = field[j + 1][i];
					field[j + 1][i] = temp;
					j = m - 2;
					continue;
				}
			}
		}
	}

	public void punch() {
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (Character.isLowerCase(field[i][j].charAt(0))) {// 소문자라면 구멍뚫기
					result++;
					field[i][j] = "-";
				}
			}
		}
	}

	public void mark(int x, int y) {
		field[x][y] = field[x][y].toLowerCase();
		field[x][y + 1] = field[x][y + 1].toLowerCase();
		field[x + 1][y] = field[x + 1][y].toLowerCase();
		field[x + 1][y + 1] = field[x][y + 1].toLowerCase();
	}

	public void print() {
		System.out.println("-----------------------------------------");
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				System.out.print(field[i][j] + " ");
			}
			System.out.println("");
		}
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String[] a = { "CCBDE", "AAADE", "AAABF", "CCBBF" };
		new Friends4(4, 5, a);
		String[] b = { "TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ" };
		new Friends4(6, 6, b);
	}

}

{% endhighlight %}

## 결과
![결과]("../../images/posts/algokakao/6res.png")
