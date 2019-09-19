# chapter 03
## 추상 자료형 : Abstract Data Type

### 추상 자료형(ADT)의 이해
- 추상 자료형이란? <br>
   '구체적인 기능의 완성과정을 언급하지 않고, 순수하게 기능이 무엇인지를 나열한 것'
<br><br>
- 지갑의 추상 자료형
    - 카드의 삽입
    - 카드의 추출(카드를 빼냄)
    - 동전의 삽임
    - 동전의 추출(동전을 빼냄)
    - 지폐의 삽임
    - 지폐의 추출(지폐를 빼냄)

### 지갑을 의미하는 구조체 Wallet의 정의
- 자료형 Wallet의 정의
```C
typedef struct _wallet // 동전 및 지폐 일부만을 대상으로 표현한 지갑
{ 
    int coin100Num;    // 100원짜리 동전의 수
    int bill5000Num;   // 5,000원짜리 지의 수
} Wallet;
```

>" 완전한 자료형의 정의로 인식되기 위해서는 해당 자료형과 관련이 있는 연산이 함께 정의되어야 한다!"

- 자료형 Wallet 정의의 일부
    - 돈을 꺼내는 연산 int TakeOutMoney(Wallet * pw, int coinNum, int billNum);
    - 돈을 넣는 연산 int PutMoney(Wallet * pw, int coinNum, int billNum);
<br><br>
- Wallet의 ADT<br>
Operations:
<br><br>
    - int TakeOutMoney(Wallet * pw, int coinNum, int billNum);
	     - 첫 번째 인자로 전달된 주소의 지갑에서 돈을 꺼낸다.
	     - 두 번째 인자로 꺼낼 동전의 수, 세 번째 인자로 꺼낼 지폐의 수를 전달한다.
	     - 꺼내고자 하는 돈의 총액이 반환된다. 그리고 그만큼 돈은 차감된다.
    <br><br>
    - int PutMoney(Wallet * pw, int coinNum, int billNum);
	     - 첫 번째 인자로 전달된 주소의 지갑에 돈을 넣는다.
	     - 두 번째 인자로 넣을 동전의 수, 세 번째 인자로 넣을 지폐의 수를 전달한다.
	     - 넣은 만큼 동전과 지폐의 수가 증가한다.
<br><br>
- Wallet 기반의 main

```C
int main(void)
{
    Wallet myWallet; // 지갑 하나 장만 했음!
    ....
    PutMoney(&myWallet, 5,10); // 돈을 넣는다.
    ....
    ret = TakeOutMoney(&myWallet, 2, 5); // 돈을 꺼낸다.
    ....
}
```

## 연결 리스트(Linked List) 1
> "지금 여러분이 공부하고 있는 이 Chapert 03의 제목은 '연결 리스트(Linked List)이다. 하지만 이 제목은 바뀌어야 한다. 그냥 '리스트(List)로 말이다. 그럼에도 불구하고 제목을 '연결 리스트'로 그냥 두었는데, 이는 '리스트는 연결 리스트이다'라는 오해를 애초부터 부정하고 들어가기 위한 것이다. 필자 스스로가 자신이 지은 제목이 잘못되었다고 하는 것만큼 기억에 오래 남는 것도 드물 테니 말이다.<br>
> _( 열혈 자료구조 )_
### 리스트의 이해
- 리스트의 구분
    - 순차 리스트 : 배열을 기반으로 구현된 리스트
    - 연결 리스트 : 메모리의 동적 할당을 기반으로 구현된 리스트
 
>"이는 구현 방법을 기준으로 한 구분이다. 따라서 이 두 리스트의 ADT가 동일하다고 해서 문제가 되지는 않는다"

- 리스트의 특징
    - 저장 형태 : 데이터를 나란히(하나의 열로) 저장한다.
    - 저장 특성 : 중볻이 되는 데이터의 저장을 허용한다.

>"이것이 리스트의 ADT를 정의하는데 있어서 고려해야 할 유일한 내용이다."

### 리스트 자료구조의 ADT
- 리스트의 초기화
     - void ListInit(List * plist);
         - 초기화할 리스트의 주소 값을 인자로 전달한다.
         - 리스트 생성 후 제일 먼저 호출되어야 하는 함수이다.
<br><br>
- 데이터 저장
    - void LInsert(List * plist, LData data);
        - 리스트에 데이터를 저장한다. 매개변수 data에 전달된 값을 저장한다.
<br><br>        
- 저장된 데이터의 탐색 및 초기화
    - int LFirst(List * plist, LData * pdata);
        - 첫 번째 데이터가 pdata가 가리키는 메모리에 저장된다.
        - 데이터의 참조를 위한 초기화가 진행된다.
        - 참조 성공 시 TRUE(1), 실패 시 FALSE(0) 반환
<br><br>
- 다음 데이터의 잠조(반환)을 목적으로 호출
    - int LNext(List * plist, LData * pdata);
        - 참조된 데이터의 다음 데이터가 pdata가 가리키는 메모리에 저장된다.
        - 순차적인 참조를 위해서 반복 호출이 가능하다.
        - 참조를 새로 시작하려면 먼저 LFirst 함수를 호출해야 한다.
        - 참조 성공시 TRUE(1), 실패 시 FALSE(0) 반환
<br><br>
- 바로 이전에 참조(반환)이 이루어진 데이터의 삭제
    - LData LRemove(List * plist);
        - LFirst 또는 LNext 함수의 마지막 반환 데이터를 삭제한다.
        - 삭제된 데이터는 반환된다.
        - 마지막 반환 데이터를 삭제하므로 연이은 반복 호출을 허용하지 않는다.
<br><br>
- 현재 저장되어 있는 데이터의 수를 반환
    - int LCount(List * plist);
        - 리스트에 저장되어 있는 데이터의 수를 반환한다.
<br><br>   
### 리스트의 ADT를 기반으로 정의된 main 함수
- ListMain.c

``` C
#include <stdio.h>
#include "ArrayList.h"

int main(void)
{
    // ArrayList의 생성 및 초기화
    List list;  // 리스트의 생성
    int data;
    ListInit(&list)  // 리스트의 초기화
        
    // 5개의 데이터 저장
    LInsert(&list, 11); LInsert(&list, 11); // 리스트에 11을 저장
    LInsert(&list, 22); LInsert(&list, 22); // 리스트에 22를 저장
    LInsert(&list, 33); // 리스트에 33을 저장
    
    // 저장된 데이터의 전체 출력
    printf("현재 데어티의 수: %d \n", LCount(&list));
    
    if(LFirst(&list, &data)) // 첫 번째 데이터 조회
    {
        printf("%d", data);
        
        while(LNext(&list, &data)) // 두 번째 이후의 데이터 조회
            printf("%d", data);
    }
    printf("\n\n");
    
    // 숫자 22를 탐색하여 모두 삭제
    if(LFirst(&list, &data))
    {
        if(data == 22)
            LRemove(&list); // 위의 LFirst 함수를 통해 참조한 데이터 삭제!
        
        while(LNext(&list, &data))
        {
            if(data == 22)
                LRemove(&list); // 위의 LNext 함수를 통해 참조한 데이터 삭제!
        }
    }
    
    // 삭제 후 남은 데이터 전체 출력
    printf("현재 데이터의 수: %d \n", LCount(&list));
    
    if(LFirst(&list, &data))
    {
        printf("%d", data);
        
        while(LNext(&list, &data))
            printf("%d",data);
    }
    printf("\n\n");
    return 0;
}
```
> "아직 소개하지 않은 파일들은 해당 도서의 자료실에서 다운받아서 실행결과를 확인하시기 바랍니다."<br>
[열혈 자료구조 자료실](http://www.orentec.co.kr/jaryosil/DA_ST_1/add_form1.php)

### 배열 기반 리스트의 헤더파일 정의
- ArrayList.h

``` h
#ifndef __ARRAY_LIST_H__
#define __ARRAY_LIST_H___

#define TRUE  1   // '참'을 표현하기 위한 매크로 정의
#define FALSE 0   // '거짓'을 표현하기 위한 매크로 정의

#define LIST_LEN 100
typedef int LData; // 저장할 대상의 자료형의 변경을 위한 typedef 선언

typedef struct __ArrayList // 배열기반 리스트를 정의한 구조체
{
    LData arr[LIST_LEN];   // 리스트의 저장소인 배열
    int numOfData;         // 저장된 데이터의 수
    int curPosition        // 데이터 참조위치를 기록
} ArrayList;               // 배열 기반 리스트를 의미하는 구조체

typedef ArrayList List;    // 리스트의 변경을 용이하게 하기 위한 typedef 선언

void ListInit(List * plist);              // 초기화
void LInsert(List * plist, LData data);   // 데이터 저장

int LFirst(List * plist, LData * pdata);  // 첫 데이터 참조
int LNext(List * plist, LData * pdata);   // 두 번째 이후 데이터 참조

LData LRemove(List * plist);              // 참조한 데이터 삭제
int LCount(List * plist);                 // 저장된 데이터의 수 반환

#endif
```
> 위의 함수들은 리스트 ADT를 기반으로 선언된 함수들이다.<br>
> 따라서 배열 기반 리스트로 선언된 함수들의 내용을 제한할 필요가 없다.

### 배열 기반의 리스트 구현 
- ArrayList.c

``` C
#include <stdio.h>
#include "ArrayList.h"

void ListInit(List * plist)
{
	(plist->numOfData) = 0;
	(plist->curPosition) = -1; // 아무런 위치도 참조하지 않았음을 의미
}

void LInsert(List * plist, LData data)
{
	if(plist->numOfData > LIST_LEN)  // 더 이상 저장할 공간이 없다면
	{
		puts("저장이 불가능합니다.");
		return;
	}

	plist->arr[plist->numOfData] = data; // 데이터 저장
	(plist->numOfData)++;                // 저장된 데이터의 수 증가
}

int LFirst(List * plist, LData * pdata)
{
	if(plist->numOfData == 0)  // 저장된 데이터가 하나도 없다면
		return FALSE;

	(plist->curPosition) = 0;  // 참조 위치 초기화! 첫 번째 데이터의 참조를 의미!
	*pdata = plist->arr[0];    // pdata가 가리키는 공간에 데이터 저장
	return TRUE;
}

int LNext(List * plist, LData * pdata)
{
	if(plist->curPosition >= (plist->numOfData)-1) // 더 이상 참조할 데이터가 없다면
		return FALSE;

	(plist->curPosition)++;
	*pdata = plist->arr[plist->curPosition];
	return TRUE;
}

LData LRemove(List * plist)
{
	int rpos = plist->curPosition; // 삭제할 데이터의 인덱스 값 차조
	int num = plist->numOfData;
	int i;
	LData rdata = plist->arr[rpos]; // 삭제할 데이터를 임시로 저장

    // 삭제를 위한 데이터의 이동을 진행하는 반복문
	for(i=rpos; i<num-1; i++)
		plist->arr[i] = plist->arr[i+1];

	(plist->numOfData)--;      // 데이터의 수 감소
	(plist->curPosition)--;    // 참조위치를 하나 되돌린다.
	return rdata;              // 삭제된 데이터의 반환
}

int LCount(List * plist)
{
	return plist->numOfData;
}
```

### 리스트에 구조체 변수 저장하기
- Point.h

``` h
#ifndef __POINT_H__
#define __POINT_H__

typedef struct _point
{
	int xpos;
	int ypos;
} Point;

// Point 변수의 xpos, ypos 값 설정
void SetPointPos(Point * ppos, int xpos, int ypos);

// Point 변수의 xpos, ypos 정보 출력 
void ShowPointPos(Point * ppos);

// 두 Point 변수의 비교
int PointComp(Point * pos1, Point * pos2); 

#endif

```

- Point.c

``` C
#include <stdio.h>
#include "Point.h"

void SetPointPos(Point * ppos, int xpos, int ypos)
{
	ppos->xpos = xpos;
	ppos->ypos = ypos;
}

void ShowPointPos(Point * ppos)
{
	printf("[%d, %d] \n", ppos->xpos, ppos->ypos);
}

int PointComp(Point * pos1, Point * pos2)
{
	if(pos1->xpos == pos2->xpos && pos1->ypos == pos2->ypos)
		return 0;
	else if(pos1->xpos == pos2->xpos)
		return 1;
	else if(pos1->ypos == pos2->ypos)
		return 2;
	else
		return -1;
}

```

- ArrayList.h의 변경 사항 두 가지
    - typedef int LData; (typedef 선언변경)  ->  typedef Point * LData;
    - #include "Point.h" // ArrayList.h에 추가할 헤더파일 선언문
<br><br>
- 실행을 위한 파일 구성
    - Point.h, Point.c &nbsp; &nbsp; 구조체 Point를 위한 파일
    - ArrayList.h, ArrayList.c &nbsp;&nbsp; 배열 기반 리스트
    - PointListMain.c &nbsp; &nbsp; 구조체 Point의 변수 저장
<br><br>
- PointListMain.c

``` C
#include <stdio.h>
#include <stdlib.h>
#include "ArrayList.h"
#include "Point.h"

int main(void)
{
	List list;
	Point compPos;
	Point * ppos;

	ListInit(&list);

	/*** 4개의 데이터 저장 ***/
	ppos = (Point*)malloc(sizeof(Point));
	SetPointPos(ppos, 2, 1);
	LInsert(&list, ppos);

	ppos = (Point*)malloc(sizeof(Point));
	SetPointPos(ppos, 2, 2);
	LInsert(&list, ppos);

	ppos = (Point*)malloc(sizeof(Point));
	SetPointPos(ppos, 3, 1);
	LInsert(&list, ppos);

	ppos = (Point*)malloc(sizeof(Point));
	SetPointPos(ppos, 3, 2);
	LInsert(&list, ppos);

	/*** 저장된 데이터의 출력 ***/
	printf("현재 데이터의 수: %d \n", LCount(&list));

	if(LFirst(&list, &ppos))
	{
		ShowPointPos(ppos);
		
		while(LNext(&list, &ppos))
			ShowPointPos(ppos);
	}
	printf("\n");

	/*** xpos가 2인 모든 데이터 삭제 ***/
	compPos.xpos=2;
	compPos.ypos=0;

	if(LFirst(&list, &ppos))
	{
		if(PointComp(ppos, &compPos)==1)
		{
			ppos=LRemove(&list);
			free(ppos);
		}
		
		while(LNext(&list, &ppos)) 
		{
			if(PointComp(ppos, &compPos)==1)
			{
				ppos=LRemove(&list);
				free(ppos);
			}
		}
	}

	/*** 삭제 후 남은 데이터 전체 출력 ***/
	printf("현재 데이터의 수: %d \n", LCount(&list));

	if(LFirst(&list, &ppos))
	{
		ShowPointPos(ppos);
		
		while(LNext(&list, &ppos))
			ShowPointPos(ppos);
	}
	printf("\n");

	return 0;
}

```

### 배열 기반 리스트의 장점과 단점
- 배열 기반 리스트의 단점
    - 배열의 길이가 초기에 결정되어야 한다. 변경이 불가능하다.
    - 삭제의 과정에서 데이터의 이동(복사)가 매우 빈번히 일어난다.
<br><br>
- 배열 기반 리스트의 장점
    - 데이터 참조가 쉽다. 인덱스 값 기준으로 어디든 한 번에 참조 가능!
    
> 배열의 일반적인 단점과 장점 입니다.
