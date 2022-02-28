참고 링크 = https://velog.io/@_dodo_hee/4%EC%B0%A8-%EC%84%B8%EB%AF%B8%EB%82%98-Part-1-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC

# 동기 (synchronous) vs 비동기 (Asynchronous)

## 동기 (synchronous)
- 요청 (request)를 보내고 응답 (response)이 오면 그 다음 동작을 처리하는 방식
- 요청과 결과가 한 자리에서 동시에 일어남
- 한번에 한번의 요청밖에 처리 못함 
- 요청을 하면 시간이 얼마가 걸리던지 요청한 자리에서 결과가 주어져야 함

## 비동기 (Asynchronous)
- 요청을 보낸 후 응답과 관계없이 다음 동작을 실행하는 방식
- 요청한 그 자리에서 결과가 주어지지 않음
- 한번에 여러가지 요청을 처리 할수 있으며
- 요청과 결과가 동시에 일어나지 않음

## 동기와 비동기의 장단점
> 동기
- 장점 = 설계가 매우 간단하고 직관적
- 단검 = 결과가 주어질 떄까지 아무것도 못하고 대기 해야함

> 비동기
- 장점: 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 하수 있으므로 자원을 효율적으로 사용 가능
- 단점: 동기보다 복잡

# 웹에서 비동기 처리

![image](https://media.vlpt.us/images/_dodo_hee/post/68e8f9a0-5efd-4ea1-9100-b416c2c22442/image.png)


## 1. 자바스크립트 엔진 구조
자바 스크립트 엔진 구조는 Memory Heap과 Call Stack 으로 구성되어 있음
- Memory Heap = 메모리 할당이 이루어지는 곳
- Call stack = 코드가 실행될 때 호출 스택이 쌓이는 곳
= 호출된 함수가 call stack에 push 되어 쌓인다 (stack은 Last in First Out 구조로 데이터를 push/append로 넣고 pop으로 꺼낸다) 

## 2. Web API 
> 브라우저가 제공하는 API
자바 스크립트 언어 자체는 비동기 동작을 지원하지 않음. 핵심은 브라우저가 가지고 있음

API 안에는 해당 구성품이 존재:
- DOM (document)
- Ajax (XMLHttpRequest)
- Timeout (setTimeout)

Callstack 에서 실행된 비동기 함수는 Web API를 호출하게된다 <br>
-> Web API는 콜백 함수를 Callback queue에 밀어 넣는다
(Queue는 first in first out구조로 append/enqueue로 넣고 popleft/dequeue로 꺼냄)

## 3. Callback Queue
> JavaScript 런타임은 처리 할 메시지 목록인 메시지 대기열을 사용

비동기적으로 이벤트 발생 시 실행해야 할 callback 함수가 Callback Queue에 추가됨

<b>선입선출 방식</b> 으로 Callback Queue에 들어있는 함수들을 호출 ([참고 que vs stack](https://velog.io/@filoscoder/Data-Structure-Stack-vs.-Queue))

함수를 호출하면 새롭게 Call Stack에 쌓음 

## 4. Event Loop
> Call stack과 Callback Queue의 상태를 체크하여,
> Call stack이 빈 상태가 되면, Callback Queue의 첫번째 콜백을 Call stack으로 넣음

# 그래서 웹에서 비동기는 어떻게 진행이 되는지
1. V8 엔진에서 코드가 실행되면
2. Call stack에 쌓인다
3. Stack의 <b>선입후출</b>의 룰에 따라 제일 마지막에 들어온 함수가 먼저 실행되며,
4. Stack에 쌓여진 함수가 모두 실행된다.
5. 비동기함수가 실행된다면, Web API가 호출됨
6. Web API는 비동기함수의 콜백함수를 Callback Queue에 밀어넣음
7. Evenet Loop는 Call Stack이 빈 상태가 되면
8. Callback Queue에 있는 첫번쨰 콜백을 Call Stack으로 이동시킴

= 이러한 반복적인 행동을 틱(tick) 이라 한다.
