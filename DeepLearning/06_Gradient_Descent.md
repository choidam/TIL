# Gradient Descent

### Gradient Descent (경사 하강법) 이란?

일단 `w`, `b` 를 임의로 설정한 일차함수와 데이터 사이의 **평균 제곱 오차 (MSE : Mean Squared Error)** 를 구한다.   
이 평균 제곱 오차를 **비용 함수(cost function)** 이라고 부른다.

기계의 입장에서는 이 비용 함수가 최소가 되게 하는 `w` 와 `b`를 찾아야 한다.

<img src="./screenshot/06_grad1.png" width="400">

초기 w의 값이 임의로 설정 되었다. (최적의 w값과는 거리가 있다.)

최적의 `w`값을 찾아가기 위해서 비용함수를 `w`에 대해서 편미분 해준 것에 학습률 (`learning rate`) 파라미터를 곱한 것을 초기 설정된 `w`값에서 빼준다.
> 편미분 : 자신을 제외한 모든 변수를 상수 취급하고 미분함

```
w = w + (-grad) * learning_rate
```

이 때 `learning_rate` 는 적절한 값으로 사용자가 설정해 줘야 한다.   
학습률이 너무 작으면 최적의 `w` 를 찾아가는 데 너무 오래 걸릴 가능성이 크고, 너무 크면 최적의 지점을 건너뛸 위험이 있기 때문이다.

<img src="./screenshot/06_grad2.png" width="600">

비용 함수를 `w` 에 대해 편미분 하면 현재 `w` 위치에서 접선의 기울기와 같다.    
이 접선 기울기의 절대값이 0이 될 때가 최적의 `w` 값이 된다. 

<br/>

### Example

<img src="./screenshot/06_grad3.png" width="400">

```
f(x,y,z) = (x+y)z
q = x + y
```
x = -2, y = 5, z = -4 이라고 한다면   
x에 대한 편미분 값은 -4, y는 -4, z는 3일 것이다.

<img src="./screenshot/06_grad4.png" width="600">

이는 **Chain Rule** 로 확인할 수 있다.   
한 변수에 대한 편미분 값은 upstream gradient 와 local gradient 를 곱한 값이다.

<img src="./screenshot/06_grad5.png" width="600">

위 초록색은 계산 값이고, 초록색은 편미분 값이다.   

add의 local gradient 는 언제나 1이므로 upstream gradient 와 동일하고,   
multiple 의 local gradient 는 곱하는 값 * upstream gradient 임을 확인할 수 있다.   
