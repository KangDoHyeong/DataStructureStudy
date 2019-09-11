## 열혈 자료구조 스터디

# Chaprter 1

- 자료구조와 알고리즘의 차이
    - 자료구조 : 데이터의 표현 및 저장방법 
    - 알고리즘 : 표현 및 저장된 데이터를 대상으로 하는 '문제의 해결 방법'
    
  → 결론 : 서로 의존적이다.


### 알고리즘의 성능 분석 방법
- 시간복잡도/공간복잡도
    - 시간복잡도 : 알고리즘 실행속도에 관한 것
    - 공간복잡도 : 메모리 사용량에 관한 것

- 알고리즘 수행속도 평가방법
    1) 연산의 횟수를 세기
    2) 처리해야할 데이터의 수 n에 대한 연산횟수의 함수 T(n)구성하기
        
        > 이때, T(n)을 굳이 구성하는 이유는 <br>T(n)을 통해 **데이터 수의 증가에 따른 연산횟수의 변화정도**를 파악하는 것이 가능하기 때문
       
    > Q. 어떤 알고리즘이 좋은 알고리즘인가? <br>A. 상황에 맞게 판단해야한다. <br> 안정적인 성능을 보장하는 알고리즘은 구현난이도가 높은 반면, 그렇지 않은 알고리즘은 구현난이도가 낮으므로, <br>데이터가 수가 많지 않고 성능에 덜 민감한 경우 구현난이도가 낮은 알고리즘을 선택한다.
  

- 시간 복잡도 분석의 핵심요소 
    1) 알고리즘마다 핵심이 되는 연산 파악하기
        - 그 연산을 줄이는 것이 시간 복잡도를 줄이는 길
    2) 시간복잡도를 따지는 기준
        - 최선의 경우(best case)
        - 최악의 경우(worst case)
        - 평균적인 경우(average case) : 제일 좋으나, 어떤게 평균적인 기준인지 명확하지 않음
        
        → 결론 : 최악의 경우로 따진다.


### 순차 탐색(Linear Search) 알고리즘과 시간 복잡도 분석의 핵심요소

```C
#include <stdio.h>

int LSearch(int ar[], int len, int target) {
	// 순차 탐색 알고리즘 적용된 함수

	int i;
	for (i = 0; i < len; i++) {
		if (ar[i] == target) {
			return i;              // 찾은 대상의 인덱스 값 반환
		} 
	}
	return -1;                    // 찾지 못했음을 의미하는 값 반환 
}

int main() {
	int arr[] = { 3, 5, 2, 4, 9 };
	int idx;
	idx = LSearch(arr, sizeof(arr) / sizeof(int), 4);
	if (idx == -1) {
		printf("탐색 실패 \n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}

	idx = LSearch(arr, sizeof(arr) / sizeof(int), 7);
	if (idx == -1) {
		printf("탐색 실패\n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}
	return 0;
}
```
#### 순차 탐색 알고리즘의 시간복잡도 계산하기
- 최악의 경우(worst case) : T(n) = n        -> _O_(n)
- 평균적인 경우(worst case) : T(n) = (3/4)n  
   <br> (가정 1: 탐색 대상이 배열에 존재하지 않을 확률은 50% <br> 가정 2: 배열의 첫 요소부터 마지막 요소까지 탐색 대상이 존재할 확률은 동일)



### 이진 탐색(binary search) 알고리즘의 소개
- 순차탐색보다 좋은 성능이나 조건이 있음
  <br> : **데이터가 정렬되어 있어야한다**
  
  
  
```C
#include <stdio.h>

int BSearch(int ar[], int len, int target) {

	int first = 0;                // 탐색 대상의 시작 인덱스 값
	int last = len - 1;           // 탐색 대상의 마지막 인덱스 값
	int mid;

	while (first <= last) {
		mid = (first + last) / 2;     // 탐색 대상의 중앙을 찾는다.
		if (target == ar[mid]) {        // 중앙에 저장된 값이 타겟이라면
			return mid;             // 탐색 완료
		}
		else {                   // 타겟이 아니라면 탐색 대상을 반으로 줄임
			if (target < ar[mid]) {
				last = mid - 1;
			}
			else {
				first = mid + 1;
			}
		}
	}
	return -1;                    // 찾지 못했음을 의미하는 값 반환 
}

int main() {
	int arr[] = { 1, 3, 5, 7, 9 };
	int idx;
	idx = BSearch(arr, sizeof(arr) / sizeof(int), 7);
	if (idx == -1) {
		printf("탐색 실패 \n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}

	idx = BSearch(arr, sizeof(arr) / sizeof(int), 4);
	if (idx == -1) {
		printf("탐색 실패\n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}
	return 0;
}
```
####  이진탐색 알고리즘의 단계
1. 시작 인덱스와 마지막 인덱스를 통해 탐색 대상의 중앙 찾기
2. 중앙값이 타겟인 경우 : 탐색 완료
3. 중앙에 저장된 값이 타겟이 아닌 경우: <br> 중앙값이 타겟보다 크면, 마지막 인덱스를 중앙값보다 하나 작은 값으로 이동 
   <br> 중앙값이 타겟보다 작으면,  인덱스를 중앙값보다 하나 큰 값으로 이동
4. 다시 1번과정으로 돌아가 1, 2, 3 과정 진행 -> 시작 인덱스가 마지막 인덱스보다 크거나 같아지면 종료!


#### 이진 탐색 알고리즘의 시간복잡도 계산하기
- 최악의 경우(worst case) : T(n) = log_{2}(n) + 1 -> _O_(logn)


### 대표적인 빅-오

| 기호 | 이름 | 설명 |
|---|:---:|:---:|
| _O_(1) | 상수형 빅-오 | 데이터 수에 상관없이 연산횟수가 고정인 유형의 알고리즘 |
| _O_(logn) | 로그형 빅-오 | 데이터 수의 증가율에 비해서 연산횟수의 증가율이 훨씬 낮은 알고리즘 |
| _O_(n) | 선형 빅-오 | 데이터의 수와 연산횟수가 비례하는 알고리즘|
| _O_(nlongn) | 선형로그형 빅-오 | 데이터의 수가 2배로 늘 때, 연산횟수는 두 배를 조금 넘게 증가하는 알고리즘 |
| _O_(n^{2}) |  | 데이터 수의 제곱에 해당하는 연산횟수를 요구하는 알고리즘 |
| _O_(n^{3}) |  |  데이터 수의 세 제곱에 해당하는 연산횟수를 요구하는 알고리즘 |
| _O_(2^{n}) | 지수형 빅-오 |  사용하기에 무리가 는 비현실적인 알고리즘 |
> _O_(1) < _O_(logn) < _O_(n) < _O_(nlongn) < _O_(n^{2}) < _O_(n^{3}) < _O_(2^{n})


### 순차 탐색 알고리즘과 이진 탐색 알고리즘의 비교

#### _O_(logn) 과 _O_(n) 알고리즘의 비교연산횟수를 수치적으로 비교해보기
- 비교를 위한 원칙
	- 최악의 경우를 대상으로 비교하는 것이 목적 → 탐색의 실패 유도하기
	- 탐색의 실패가 결정되기까지 몇 번의 비교연산이 진행되는지 세기
	- 데이터의 수는 500, 5000, 50000일 때를 기준으로 각각 실험 진행하기
- _O_(n)의 경우 데이터의 수가 곧 비교연산횟수이므로 계산을 따로 진행하지 않음

##### _O_(n) 알고리즘의 비교연산 횟수

```C
#include <stdio.h>

int BSearch(int ar[], int len, int target) {

	int first = 0;
	int last = len - 1;
	int mid;
	int opCount = 0;


	while (first <= last) {
		mid = (first + last) / 2;
		if (target == ar[mid]) {
			return mid;
		}
		else {                   
			if (target < ar[mid]) {
				last = mid - 1;
			}
			else {
				first = mid + 1;
			}
		}
		opCount += 1;
	}
	printf("비교연산횟수: %d \n", opCount);    // 탐색 실패 시 연산횟수 출력
	return -1;                    
}

int main() {
	int arr1[500] = { 0, };    // 모든 요소 0으로 초기화
	int arr2[5000] = { 0, };    // 모든 요소 0으로 초기화
	int arr3[50000] = { 0, };    // 모든 요소 0으로 초기화
	int idx;

	idx = BSearch(arr1, sizeof(arr1) / sizeof(int), 1);
	if (idx == -1) {
		printf("탐색 실패 \n\n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}

	idx = BSearch(arr2, sizeof(arr2) / sizeof(int), 2);
	if (idx == -1) {
		printf("탐색 실패 \n\n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}

	idx = BSearch(arr3, sizeof(arr3) / sizeof(int), 3);
	if (idx == -1) {
		printf("탐색 실패 \n\n");
	}
	else {
		printf("타겟 저장 인덱스 : %d\n", idx);
	}
	return 0;
}

```
##### 결론
| n | 순차 탐색 연산횟수 | 이진탐색 연산횟수 |
|:---:|:---:|:---:|
| 500 | 500 | 9 |
| 5,000 | 5,000 | 13 |
| 50,000 | 50,000 | 16 |


### 빅-오에 대한 수학적 접근 
> 두 개의 함수 f(n)과 g(n) 이 주어졌을 때, <br> 모든 n>=K에 대하여 f(n) <= Cg(n) 을 만족하는 두 개의 상수 C와 K가 존재하면, f(n)의 빅-오는 O(g(n))이다.
