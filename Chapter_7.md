# Chapter 7. 큐(Queue)

## 7-1. 큐(Queue)란?
First-In, Last-In 형태의 자료구조<br>
가장 첫번째로 들어간 데이터가 가장 먼저 출력된다.<br> 
반대로 마지막에 들어간 데이터가 제일 나중에 출력된다. <br>


### 큐 ADT (Abstract Data Type) 정의
void QueueInit(Queue * pq); 
- 큐의 초기화를 진행한다.
- 큐 생성 후 제일 먼저 호출되어야 하는 함수

int IsEmpty(Queue * pq);
- 큐가 비어있는지 한 개 이상의 데이터가 들어가 있는지 확인하는 함수
- 큐가 빈 경우에는 TRUE(1)을, 그렇지 않은 경우에는 FALSE(0)을 반환한다.

void Enqueue(Queue * pq)
- 큐에 매개변수로 전달된 데이터를 저장한다. 

Data Dequeue (Queue * pq, data); 
- 저장순서가 가장 앞선 데이터를 삭제한다. 
- 삭제된 데이터는 반환된다. 
- IsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

Data Peek(Queue * pq);
- 저장순서가 가장 앞선 데이터를 반환한다. 
- 반환된 데이터를 삭제하지 않는다.
- IsEmpty의 반환값이 FALSE(0)임을 전제로 한다.
<br></br>
## 7-2. 큐의 배열 기반 구현

큐를 배열 기반으로 구현할 때는 Front 변수와 Rear 변수가 필요하다. <br>
Front 변수는 마지막으로 저장된 데이터의 위치를 기억하는 변수이다. <br>
Rear 변수는 제일 최근에 저장된 데이터의 위치를 기억하는 변수이다. <br>
Front == Rear 일 때 큐는 비어있다고 말할 수 있다. <br>

```C
#include <stdio.h>
#include <stdlib.h>


#define TRUE	1
#define FALSE	0

#define QUE_LEN	100

typedef int Data;

typedef struct _cQueue
{
	int front;
	int rear;
	Data queArr[QUE_LEN];

} CQueue;


typedef CQueue Queue;

void QueueInit(Queue * pq)
{
	pq->front = 0;
	pq->rear = 0;
}

int IsEmpty(Queue * pq)
{
	if (pq->front == pq->rear)
		return TRUE;
	else
		return FALSE;
}

int NextPosIdx(int pos)
{
	if (pos == QUE_LEN - 1)
		return 0;
	else
		return pos + 1;
}

void Enqueue(Queue * pq, Data data)
{
	if (NextPosIdx(pq->rear) == pq->front)
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	pq->rear = NextPosIdx(pq->rear);
	pq->queArr[pq->rear] = data;
}

Data Dequeue(Queue * pq)
{
	if (IsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	pq->front = NextPosIdx(pq->front);
	return pq->queArr[pq->front];
}

Data Peek(Queue * pq)
{
	if (IsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	return pq->queArr[NextPosIdx(pq->front)];
}

int main(void)
{
	Queue q;
	QueueInit(&q);

	Enqueue(&q, 1);  Enqueue(&q, 2);
	Enqueue(&q, 3);  Enqueue(&q, 4);
	Enqueue(&q, 5);

	printf("%d", Peek(&q));

	while (!IsEmpty(&q))
		printf("%d ", Dequeue(&q));



	return 0;
}
```
## 7-3. 큐의 연결리스트 기반 구현

큐를 연결리스트 기반으로 구현할 때는 *Front 포인터와 *Rear 포인터가 필요하다. <br>
*Front 포인터는 마지막으로 저장된 노드의 위치를 가리키는 포인터이다. <br>
*Rear 포인터는 제일 최근에 저장된 노드의 위치를 가리키는 포인터이다. <br>
*Front == Null 일 때 큐는 비어있다고 말할 수 있다. <br>


```C
#include <stdio.h>
#include <stdlib.h>

#define TRUE	1
#define FALSE	0

typedef int Data;

typedef struct _node
{
	Data data;
	struct _node * next;
} Node;

typedef struct _lQueue
{
	Node * front;
	Node * rear;
} LQueue;

typedef LQueue Queue;

void QueueInit(Queue * pq)
{
	pq->front = NULL;
	pq->rear = NULL;
}

int QIsEmpty(Queue * pq)
{
	if (pq->front == NULL)
		return TRUE;
	else
		return FALSE;
}

void Enqueue(Queue * pq, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->next = NULL;
	newNode->data = data;

	if (QIsEmpty(pq))
	{
		pq->front = newNode;
		pq->rear = newNode;
	}
	else
	{
		pq->rear->next = newNode;
		pq->rear = newNode;
	}
}

Data Dequeue(Queue * pq)
{
	Node * delNode;
	Data retData;

	if (QIsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	delNode = pq->front;
	retData = delNode->data;
	pq->front = pq->front->next;

	free(delNode);
	return retData;
}

Data QPeek(Queue * pq)
{
	if (QIsEmpty(pq))
	{
		printf("Queue Memory Error!");
		exit(-1);
	}

	return pq->front->data;
}

int main(void)
{

	Queue q;
	QueueInit(&q);

	Enqueue(&q, 1);  Enqueue(&q, 2);
	Enqueue(&q, 3);  Enqueue(&q, 4);
	Enqueue(&q, 5);


	printf("%d\n", QPeek(&q));

	while (!QIsEmpty(&q))
		printf("%d ", Dequeue(&q));



	return 0;
}
```
## 7-4. 덱(Deque)란?
노드를 앞(Front)에 생성할 수도 있고, 뒤(Rear)에 생성할 수도 있는 큐 형태<br>


### 덱 ADT (Abstract Data Type) 정의
void DequeInit(Queue * pdeq); 
- 덱의 초기화를 진행한다.
- 덱 생성 후 제일 먼저 호출되어야 하는 함수

int DQIsEmpty(Deque * pdeq);
- 덱이 비어있는지 한 개 이상의 데이터가 들어가 있는지 확인하는 함수
- 덱이 빈 경우에는 TRUE(1)을, 그렇지 않은 경우에는 FALSE(0)을 반환한다.

void DQAddFirst(Deque * pdeq, Data data);
- 덱의 머리 매개변수로 전달된 데이터를 저장한다. 

void DQAddLast(Deque * pdeq, Data data);
- 덱의 꼬리 매개변수로 전달된 데이터를 저장한다. 

Data DQRemoveFirst(Deque * pdeq);
- 덱의 머리에 저장된 데이터를 반환한다.
- 반환된 데이터는 삭제한다. 
- DQIsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

Data DQRemoveLast(Deque * pdeq);
- 덱의 꼬리에 저장된 데이터를 반환한다.
- 반환된 데이터는 삭제한다. 
- DQIsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

Data DQPeekFirst(Deque * pdeq);
- 덱의 머리에 저장된 데이터를 반환한다.
- 반환된 데이터는 삭제하지 않는다.
- DQIsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

Data DQPeekFirst(Deque * pdeq);
- 덱의 꼬리에 저장된 데이터를 반환한다.
- 반환된 데이터는 삭제한다. 
- DQIsEmpty의 반환값이 FALSE(0)임을 전제로 한다.

DQReverse(Deque * pdeq);
- 꼬리와 머리의 위치를 바꾼다.

<br></br>

```C
#include <stdio.h>
#include <stdlib.h>

#define TRUE	1
#define FALSE	0

typedef int Data;

typedef struct _node
{
	Data data;
	struct _node * next;
	struct _node * prev;
} Node;

typedef struct _dlDeque
{
	Node * head;
	Node * tail;
} DLDeque;

typedef DLDeque Deque;

void DequeInit(Deque * pdeq)
{
	pdeq->head = NULL;
	pdeq->tail = NULL;
}

int DQIsEmpty(Deque * pdeq)
{
	if (pdeq->head == NULL)
		return TRUE;
	else
		return FALSE;
}

void DQAddFirst(Deque * pdeq, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->next = pdeq->head;

	if (DQIsEmpty(pdeq))
		pdeq->tail = newNode;
	else
		pdeq->head->prev = newNode;

	newNode->prev = NULL;
	pdeq->head = newNode;
}

void DQAddLast(Deque * pdeq, Data data)
{
	Node * newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;

	newNode->prev = pdeq->tail;

	if (DQIsEmpty(pdeq))
		pdeq->head = newNode;
	else
		pdeq->tail->next = newNode;

	newNode->next = NULL;
	pdeq->tail = newNode;
}

Data DQRemoveFirst(Deque * pdeq)
{
	Node * rnode = pdeq->head;
	Data rdata = pdeq->head->data;

	if (DQIsEmpty(pdeq))
	{
		printf("Deque Memory Error!");
		exit(-1);
	}

	pdeq->head = pdeq->head->next;
	free(rnode);

	if (pdeq->head == NULL)
		pdeq->tail = NULL;
	else
		pdeq->head->prev = NULL;

	return rdata;
}

Data DQRemoveLast(Deque * pdeq)
{
	Node * rnode = pdeq->tail;
	Data rdata = pdeq->tail->data;

	if (DQIsEmpty(pdeq))
	{
		printf("Deque Memory Error!");
		exit(-1);
	}

	pdeq->tail = pdeq->tail->prev;
	free(rnode);

	if (pdeq->tail == NULL)
		pdeq->head = NULL;
	else
		pdeq->tail->next = NULL;

	return rdata;
}

Data DQGetFirst(Deque * pdeq)
{
	if (DQIsEmpty(pdeq))
	{
		printf("Deque Memory Error!");
		exit(-1);
	}

	return pdeq->head->data;
}

Data DQGetLast(Deque * pdeq)
{
	if (DQIsEmpty(pdeq))
	{
		printf("Deque Memory Error!");
		exit(-1);
	}

	return pdeq->tail->data;
}

int main(void)
{
	Deque deq;
	DequeInit(&deq);

	DQAddFirst(&deq, 3);
	DQAddFirst(&deq, 2);
	DQAddFirst(&deq, 1);

	DQAddLast(&deq, 4);
	DQAddLast(&deq, 5);
	DQAddLast(&deq, 6);

	printf("%d\t%d\n", DQGetFirst(&deq), DQGetLast(&deq));

	while (!DQIsEmpty(&deq))
		printf("%d ", DQRemoveFirst(&deq));

	printf("\n");


	DQAddFirst(&deq, 3);
	DQAddFirst(&deq, 2);
	DQAddFirst(&deq, 1);

	DQAddLast(&deq, 4);
	DQAddLast(&deq, 5);
	DQAddLast(&deq, 6);

	while (!DQIsEmpty(&deq))
		printf("%d ", DQRemoveLast(&deq));

	return 0;
}
```
