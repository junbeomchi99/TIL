# Shallow Copy vs Deep Copy

참고 링크: https://www.kowanas.com/coding/2021/01/02/deep-copy%EC%99%80-shallow-copy/

참고링크: https://blockdmask.tistory.com/576



원래 값을 다른 변수에 지정할때 (e.g. B = A로 a에 값을 B라는 변수에 저장할때)
- 1. Shallow Copy = 하나의 메모리만 놓고 2개의 변수를 가르킴
- 2. Deep Copy = 하나의 메모리가 새로 생선되어 2개의 메모리가 2개의 변수를 가르킴

![Alt text](https://www.kowanas.com/coding/wp-content/uploads/sites/5/2021/01/image-10.png)

## 주의점
Shallow Copy를 사용할 경우 원래 값이 변하면 복사된 값을 포함하여 2개의 값 모두 변경됨
<br>
Deep copy를 다수의 위치에서 참조하도록 사용하면 연결이 끊어질수 있음 
+ Deep copy가 너무 많이 발생하면 멤모리 낭비가 발생함

## Tip
```python 
import copy
b = copy.deepcopy(a)
```
를 사용하여서 deepcopy를할수있음

mutable 하고 immuatable한 data type들이 조금씩 다름 (조금더 알아볼것)

그림으로 정리:
![Alt text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcYFFT4%2FbtrhZnFeE1q%2F72WfN762lRdIpZhkNkpER0%2Fimg.png)
![Alt text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FyCzgY%2Fbtrh7ykYNpJ%2FxdF4vdL5sFBKLWRARCVfF0%2Fimg.png)
![Alt Text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FS6TlO%2Fbtrh4LZqfOc%2F4omkkPXdasUPnI4DarIrik%2Fimg.png)