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
    int bill5000Num;   // 5,000원짜리 지페의 수
} Wallet;
```

>" 완전한 자료형의 정의로 인식되기 위해서는 해당 자료형과 관련이 있는 연산이 함께 정의도어야 한다!"

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