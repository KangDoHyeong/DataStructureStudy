# Chapter 6. 스택(Stack)

## 6-1. 스택이란?
Last-In, First-Out 형태의 자료구조<br>
가장 먼저 들어간 데이터가 가장 마지막에 출력된다.<br> 
반대로 가장 늦게 들어간 데이터가 가장 먼저 출력된다. <br>


### 스택 ADT (Abstract Data Type) 정의
StackInit 
- 스택의 초기화를 진행한다.
- 스택 생성 후 제일 먼저 호출되어야 하는 함수

IsEmpty
- 스택이 비어있는지 한 개 이상의 데이터가 들어가 있는지 확인하는 함수
- 스택이 빈 경우에는 TRUE(1)을, 그렇지 않은 경우에는 FALSE(0)을 반환한다.

Push
- 스택에 매개변수로 전달된 데이터를 저장한다. 
- 매개변수로 전달된 데이터는 스택의 맨 위에 저장된다.

Pop 
- 마지막으로 저장된 요소를 반환한다.
- 마지막으로 저장된 요소를 삭제한다.
- IsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

Peek
- 마지막으로 저장된 요소를 반환한다.
- 마지막으로 저장된 요소를 삭제하지 않는다.
- IsEmpty의 반환값이 FALSE(0)임을 전제로 한다.
<br></br>
## 6-2. 스택의 배열 기반 구현
스택을 배열 기반으로 구현할 때는 index 변수가 필요하다. <br>
index 변수는 마지막으로 저장된 데이터의 위치를 기억하는 변수이다. <br>
index 변수가 0일 때에는 스택이 비어있다는 것을 의미한다. <br>
index 변수는 코드에서는 topIndex 라는 변수로 정의하였다. <br>

```C
#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0
#define STACK_LEN 100

typedef int Data;

typedef struct _arrayStack
{
	Data stackArr[STACK_LEN];
	int topIndex;
}ArrayStack;

typedef ArrayStack Stack;

void StackInit(Stack* pstack) 
{
	pstack->topIndex = -1;
}

int IsEmpty(Stack* pstack)
{
	if (pstack->topIndex == -1)
		return TRUE;
	else
		return FALSE;
}

void Push(Stack * pstack, Data data)
{
	pstack->topIndex += 1;
	pstack->stackArr[pstack->topIndex] = data;
}

Data Pop(Stack * pstack)
{
	int rIdx;

	if (IsEmpty(pstack)) {
		printf("STACK MEMORY ERROR!\n");
		exit(-1);
	}
	rIdx = pstack->topIndex;
	pstack->topIndex -= 1;

	return pstack->stackArr[rIdx];

}

Data Peek(Stack * pstack)
{
	if (IsEmpty(pstack)) {
		printf("STACK MEMORY ERROR!\n");
		exit(-1);
	}

	return pstack->stackArr[pstack->topIndex];
}

int main(void)
{
	
	Stack stack;
	StackInit(&stack);

	Push(&stack, 1);
	Push(&stack, 2);
	Push(&stack, 3);
	Push(&stack, 4);
	Push(&stack, 5);
	
	printf("%d\n", Peek(&stack));
	while (!IsEmpty(&stack))
		printf("%d\n", Pop(&stack));


	return 0;
  
}
```

## 6-3 스택의 연결리스트 기반 구현
노드를 머리가 아닌 꼬리에 삽입하는 연결리스트를 구현하면 된다.<br>
노드 <br>

```c
#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0

typedef int Data;

typedef struct _node
{
	Data data;
	struct _node * next;
} Node;

typedef struct _listStack
{
	Node * head;

} ListStack;

typedef ListStack Stack;

void StackInit(Stack* pstack) 
{
	pstack->head = NULL;
}

int IsEmpty(Stack* pstack)
{
	if (pstack->head == NULL)
		return TRUE;
	else
		return FALSE;
}
void Push(Stack * pstack, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));

	newNode->data = data;
	newNode->next = pstack->head;

	pstack->head = newNode;
}
Data Pop(Stack * pstack)
{
	Data rdata;
	Node * rnode;

	if (IsEmpty(pstack)) {
		printf("STACK MEMORY ERROR!\n");
		exit(-1);
	}
	rdata = pstack->head->data;
	rnode = pstack->head;

	pstack->head = pstack->head->next;
	free(rnode);

	return rdata;

}
Data Peek(Stack * pstack)
{
	if (IsEmpty(pstack)) {
		printf("STACK MEMORY ERROR!\n");
		exit(-1);
	}

	return pstack->head->data;
}

int main(void)
{
	
	Stack stack;
	StackInit(&stack);
	
	Push(&stack, 1);
	Push(&stack, 2);
	Push(&stack, 3);
	Push(&stack, 4);
	Push(&stack, 5);
	
	printf("%d\n", Peek(&stack));
	while (!IsEmpty(&stack))
		printf("%d\n", Pop(&stack));


	return 0;
}
```

## 6-3 스택을 활용한 계산기 프로그램 구현

스택을 활용한 계산기를 구현하기 위해서는 먼저 후위표기법에 대한 개념을 갖고 있어야 한다.<br><br>
중위 표기법 : A * (( B + C ) / ( D - E )) <br>
후위 표기법 : ABC+DE+/* <br><br>
우리가 흔히 사용하는 중위 표기법은 변수와 변수 사이에 연산자를 삽입하지만 <br>
후위 표기법은 위와 같이, 두 개의 변수를 먼저 쓰고  뒤에 연산자를 삽입한다. <br>
컴퓨터는 후위표기법을 통한 연산을 진행하므로, 중위표기법으로 표시되어있는 식을 후위표기법으로 변환해주어야한다.<br>
여기서 사용되는 것이 스택을 활용한 표기법 변환이다.<br>

