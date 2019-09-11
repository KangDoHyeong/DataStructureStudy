## 열혈 자료구조 스터디

# Chaprter 2

## 2-1. 함수의 재귀적 호출의 이해
### 재귀함수의 기본적인 이해

- **재귀함수** : 함수 내에서 자기 자신을 다시 호출하는 함수

#### 재귀함수의 형태
```C
void Recursive(void)
{
	printf("Recursive call! \n");
	Recursive();         // 나! 자신을 재 호출한다.
}
```

재귀함수가 호출되면, 그 함수의 복사본이 만들어져 복사본이 실행된다고 이해하면 좋다!
> "Recursive 함수를 실행하는 중간에 다시 Recursive 함수가 호출되면, <br>Recursive 함수의 복사본을 하나 더 만들어서 복사본을 실행하게 됩니다."

#### 재귀함수 탈출 조건까지 추가한 재귀함수 예제
#### Recursive
```C
#include <stdio.h>

void Recursive(int num)
{
	if (num <= 0) {      // 재귀의 탈출 조건
		return;         // 재귀의 탈출!
	}
	printf("Recursive call! %d \n", num);
	Recursive(num-1);        
}

int main() 
{	
	Recursive(3);
	return 0;
}
```
위 예제에 따르면 Recursive(3)함수에서 Recursive(2)함수, Recursive(1)함수, Recursive(0)함수가 호출되고, <br>Recursive(0)함수에서 반환조건이 성립하여 Recursive(1), Recursive(2), Recursive(3)함수가 차례로 반환된다.

### 재귀함수의 디자인 사례

#### 함수 설계
n!=n * (n-1) * (n-2) * ... * 2 * 1이므로, <br>
n>=1일 때, n!=n * (n-1)!로 표현할 수 있다. <br>
n=0일 때 n!=1이므로, 이를 이용하여 코드를 구현하면 아래와 같은 함수식이 나온다.

#### RecursiveFactorial
```C
#include <stdio.h>

int Factorial(int n)
{
	if (n == 0)
	{
		return 1;
	}
	else
	{
		return n * Factorial(n - 1);
	}
}

int main() 
{	
	printf("1! = %d \n", Factorial(1));
	printf("2! = %d \n", Factorial(2));
	printf("3! = %d \n", Factorial(3));
	printf("4! = %d \n", Factorial(4));
	printf("9! = %d \n", Factorial(9));
	return 0;
}
```


## 2-2. 재귀의 활용
### 피보나치 수열 : Fibonacci Sequence

#### 함수 설계

피보나치 수열은 n=1일 때 f(n) = 0, n=2일 때 f(n) = 1, n>=3일 때, f(n) = f(n-1) + f(n-2)이므로 코드로 구현하면 다음과 같다.

#### FibonacciFunc
```C
#include <stdio.h>

int Fibo(int n)
{
	if (n == 1)
	{
		return 0;
	}
	else if (n == 2) 
	{
		return 1;
	}
	else
	{
		return Fibo(n - 1) + Fibo(n - 2);
	}
}

int main() 
{	
	int i;
	for (i = 1; i < 15; i++) {
		printf("%d ", Fibo(i));
	}
	return 0;
}
```

다음은 Fibo 함수의 호출 순서를 정리하기 위한 코드이다.

```C
#include <stdio.h>

int Fibo(int n)
{
	printf("func call param %d \n", n);

	if (n == 1)
	{
		return 0;
	}
	else if (n == 2) 
	{
		return 1;
	}
	else
	{
		return Fibo(n - 1) + Fibo(n - 2);
	}
}

int main() 
{	
	Fibo(7);
	return 0;
}
```

결과를 보게 되면 많은 수의 함수호출(이 코드의 경우 25회의 함수호출)이 동반되었다.
<br> 재귀함수의 호출순서를 정리하는 것은 큰 의미가 없으므로!! 쉽지않거나 매우 귀찮으므로!!
<br> **호출순서를 정리하기 보다는 재귀 함수 자체를 이해하는 것이 필요해 보인다.**

### 이진 탐색 알고리즘의 재귀적 구현
이진 탐색도 다음과 같은 과정이 반복되므로 재귀함수를 활용하여 표현할 수 있다.
1) 탐색 범위의 중앙에 타겟 값이 있는지 확인
2) 타겟 값이 없다면 탐색 범위를 반으로 줄여서 다시 탐색 시작

이를 코드로 나타내면 다음과 같다.
(참고로 이 코드는 재귀함수를 익숙해지기 위한 목적으로 쓰여진 코드이다.)

#### RecursiveBinarySearch
```C
#include <stdio.h>

int BSearchRecur(int ar[], int first, int last, int target)
{
	int mid;
	if (first > last) 
	{
		return -1;
	}
	mid = (first + last) / 2;

	if (ar[mid] == target) 
	{
		return mid;
	}
	else if (target < ar[mid])
	{
		return BSearchRecur(ar, first, mid - 1, target);
	}
	else 
	{
		return BSearchRecur(ar, mid + 1, last, target);
	}
}

int main() 
{	
	int arr[] = { 1, 3, 5, 7, 9 };
	int idx;

	idx = BSearchRecur(arr, 0, sizeof(arr) / sizeof(int) - 1, 7);
	if (idx == -1) {
		printf("탐색 실패 \n");
	}
	else 
	{
		printf("타겟 저장 인덱스: %d \n", idx);
	}

	idx = BSearchRecur(arr, 0, sizeof(arr) / sizeof(int) - 1, 4);
	if (idx == -1) {
		printf("탐색 실패 \n");
	}
	else
	{
		printf("타겟 저장 인덱스: %d \n", idx);
	}

	return 0;
}
```

## 2-3. 하노이 타워: The Tower of Hanoi
### 하노이 타워 문제의 이해
- 재귀적으로 접근하지 않으면 해결이 쉽지 않은 문제
> "원반은 한 번에 하나씩만 옮길 수 있습니다. 그리고 옮기는 과정에서 작은 원반의 위에 큰 원반이 올려져서는 안됩니다."
<br> ![The Tower of Hanoi](https://en.wikipedia.org/wiki/File:Iterative_algorithm_solving_a_6_disks_Tower_of_Hanoi.gif)

하노이 타워의 문제를 일반화시키면 다음과 같다.
1) 작은 원반 n-1개를 A에서 B로 이동
2) 큰 원반 1개를 A에서 C로 이동
3) 작은 원반 n-1개를 B에서 C로 이동

### 하노이 타워 문제의 해결

최종목표는 num개의 원반을 by를 이용해서 from에서 to로 이동시키는 것이다.

#### HanoiTowerSolu
```C
#include <stdio.h>

void HanoiTowerMove(int num, char from, char by, char to)
{
	if (num == 1)        // 이동할 원반의 수가 1개라면
	{
		printf("원반 1을 %c에서 %c로 이동 \n", from, to);
	}
	else {
		HanoiTowerMove(num - 1, from, to, by);
		printf("원반 %d을(를) %c에서 %c로 이동 \n", num, from, to);
		HanoiTowerMove(num - 1, by, from, to);
	}
}

int main() 
{	
	// 막대 A의 원반 3개를 막대 B를 경유하여 막대 C로 옮기기
	HanoiTowerMove(3, 'A', 'B', 'C');

	return 0;
}
```
