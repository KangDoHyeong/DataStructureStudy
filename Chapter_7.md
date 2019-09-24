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

Data Dequeue (Queue * pq); 
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
	// Queue의 생성 및 초기화 ///////
	Queue q;
	QueueInit(&q);

	// 데이터 넣기 ///////
	Enqueue(&q, 1);  Enqueue(&q, 2);
	Enqueue(&q, 3);  Enqueue(&q, 4);
	Enqueue(&q, 5);


	printf("%d\n", QPeek(&q));

	// 데이터 꺼내기 ///////
	while (!QIsEmpty(&q))
		printf("%d ", Dequeue(&q));



	return 0;
}
```
