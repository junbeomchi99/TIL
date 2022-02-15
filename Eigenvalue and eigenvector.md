# Eigenvalue (고유값) & Eigenvector (고유벡터)

참고 link: https://losskatsu.github.io/linear-algebra/eigen/#
참고 link2: https://angeloyeo.github.io/2019/07/27/PCA.html


Linear Algebra (선형대수)의 컨셉중 하나. 

- 교과서 정의: 
>  Ax =λx
> λ가 실수 공간에 속한때 λ∈R), 정방행렬(square matrix) A의 고유벡터는 영벡터가 아닌 벡터(nonzero vector) x이다. 또한, λ는 A의 고유값이다.

뭔소리지...?

# 정방행렬 A (Square Matrix)의 의미
고유값, 고유벡터를 논할때 정방 행렬은 선형변환 (Linear Map)을 의미한
Linear Map은 뭐지..?

# 선형변환의 의미
= 좌표공간 내에서 일어날 수 있는 모든 변환. 

e.g. 좌표평면에 백터 하나가 존재한다고 가장할시: 
- 벡터 확대
- 벡터 축소
- 백터 회전
- 백터 반사

등은 모두 선형변환 (엄밀히 말하면 틀린 설명이라지만 일단 그렇게 알고있자...)

= 원본 벡터 v는 다양한 선형변환을 통해 새로운 모양(?)의 백터 v1,v2,v3,v4..... 로 변활할수 있다 

![예시그림](https://losskatsu.github.io/assets/images/eigen/lineartransformation.JPG)

# Ax 의 의미
그럼 Ax 는 무엇을 의미하냐.
위 설명처럼 Ax는 x라는 백터에 선형변환(A)을 취한 것. 
(e.g. 백터 x를 늘리거나, 줄이거나, 회전등 '변화'주기)

# '고유', '벡터'의 의미
벡터 구성 요소
- 방향 (DIRECTION) + 크기 (MAGNITUDE)

고유(eigen) 
- 특유의, 특징 
- tmi: 독일어로 "own", "peculiar to", "characterist", "individual" 이란뜻

그래서 고유벡터란...?

# 고유 벡터

고유벡터 = 방향(direction)은 변하지 않고, 크기 (magnitude)만 변하는 벡터
- 어떠한 선형변환을 취해도 방향은 변치않고 크기만 변한다

# 고유 값의 의미 
고유 값 = 앞서서 고유벡터의 크기만 변한다 했는데, 그 변환 크기
- e.g. if eigenvector = 2, 기존벡터는 2배로 커질것이며, 고유값이 1/3이라면 기존 벡터 크의 1/3만큼 줄것이다. 

# 그래서 왜 중요한거지..? 

고유값과 고유벡터는 <b>임의의 벡터를 어느 방향으로 변화시켰는지, 변화 과정에서 변화 없이 유지 되는 부분은 어느 부분인지 알려준다</b>

그게 왜 중요하지...?
- e.g. 어떠한 물체나 영상등을 무수히 많은 벡터 뭉치라고 이야기했을때:
- 고유값과 고유벡터로 인해 영상이나 물체가 어떤식으로 변화되고 중심축은 어디인지에 관한 중요한 정보들을 파악할수있음

# 응용분야
PCA, EigenFace 등등...

여기서 PCA에 대해서 간단히 얘기해보겠다 (왜냐: 학교에서 배워서 써봤는데 기억이 안나서..)

# PCA (to be continuted....)
= Principle Components Analysis
- 데이터 분석 (주성분 찾기)
- 데이터 압축 (차원감소)
- 노이즈 제거

등에 활용되며 Collinearity (공선성)을 해결하여 회귀모델 문제 해결

PCA가 말하는것: 데이터들을 정사영 시켜 차원을 낮춘다면, 어떤 벡터에 데이터들을 정사영 시켜야 원래의 데이터 구조를 제일 잘 유지할 수 있을까? 

# 공분산 행력의 의미
= 일종의 행렬 (matrix), 데이터 구조를 설명해주며 특히 특징 쌍 (feature pairs)들의 변동이 얼마나 닮았는가 (혹은 얼마나 함께 변하는가)를 행렬로 나타내줌
