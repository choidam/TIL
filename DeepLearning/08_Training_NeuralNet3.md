# Training NeuralNet (3) 

## Optimization

딥러닝에서 optimization 은 학습속도를 안정적이게 하는 것을 의미한다.

<img src="screenshot/opt.gif" width="400">

> 협곡에서 optimizer 별 경로

<img src="screenshot/opt2.gif" width="400">

> 말안장점에서 optimizer 별 경로

### 1. SGD (Stochastic gradient descent)

Gradient Descent 는 전체 dataset 을 가지고 한 발자국 전진할 때마다 (learning rate) 최적의 값을 찾아 나간다.   
그러나 SGD는 **Mini-batch** 사이즈 만큼 조금씩 돌려서 최적의 값으로 찾아간다.

이 방법은 GD 보다 다소 부정확할 수는 있지만, 훨씬 계산 속도가 빠르기 때문에 같은 시간에 더 많은 step 을 갈 수 있으며, 여러 번 반복할 경우 보통 batch 의 결과와 유사한 결과로 수렴한다. 또한, GD 에서 빠질 local minima 에 빠지지 않고 더 좋은 방향으로 수렴할 가능성도 있다.

<img src="./screenshot/08_opt1.png" height=300"> 


```python
import numpy as np

class Optimizer:
    def __init__(self, learning_rate=0.01):
        self.lr = learning_rate

    def update(self, weight, grad):
        weight

""" Stochastic gradient descent """
class SGD(Optimizer):
    def __init__(self, learning_rate=0.01):
        super().__init__(learning_rate)

    def update(self, weight, grad):
        weight -= self.lr * grad

```
<img src="./screenshot/08_opt3.png" width="300">

그러나 문제점이 존재한다. mini batch 를 통해 학습시키는 경우 최적의 값을 찾아 가기 위한 방향 설정이 뒤죽박죽이기 때문이다.

### 2. Momentum

`momentum` 은 누적된 과거 gradient 가 지향하고 있는 방향을 현재 gradient 에 보정하는 방식이다.   
기울기 업데이트 시 폭을 조절하는 역할을 하며 이에 따라 velocity 가 변한다.

```python
class Momentum(Optimizer):
def __init__(self, learning_rate=0.01, momentum=0.9):
    super().__init__(learning_rate)
    self.momentum = momentum
    self.v = None

def update(self, weight, grad):
    if self.v is None:
        self.v = np.zeros_like(weight)
    
    self.v = self.momentum * self.v - self.lr * grad
    weight += self.v
```

### 3. NAG (Nestrov Accelerated Gradient)

<img src="./screenshot/08_opt4.png" width="600">

> `momentum` 과 `nag` 를 한 번에 비교한 그림

`momentum` 은 이동 벡터를 계산할 때 현재 위치에서의 gradient 와 momentum step 을 독립적으로 계산하고 합친다.   
반면 `NAG` 에서는 momentum step을 먼저 이동했다고 생각한 후 그 자리에서의 gradient 를 구해서 gradient step 을 이동한다. 

`momentum` 방식의 따른 이동에 대한 이점은 누리면서도, 멈춰야 할 적절한 시점에 제동을 거는 데에 매우 용이하다.

### 4. AdaGrad (Adaptive Gradient Algorithm)

`Adagrad` 는 변수들을 update 할 때 각각의 변수마다 step size 를 다르게 설정해서 이동하는 방식이다.   

즉, 자주 등장하거나 변화를 많이 한 변수들의 경우 optimum 에 가까이 있을 확률이 높기 때문에 작은 크기로 이동하면서 세밀한 값을 조정하고, 적게 변화한 변수들은 optimum 에 도달하기 위해 많이 이동해야 할 확률이 높기 때문에 먼저 빠르게 loss 값을 줄이는 방향으로 이동하는 방식이다.

```python
class AdaGrad(Optimizer):
def __init__(self, learning_rate=0.01):
    super().__init__(learning_rate)
    self.cache = None # 각 차원의 gradient 가 cache에 누적됨

def update(self, weight, grad):
    if self.cache is None:
        self.cache = np.zeros_like(weight)
    self.cache += grad ** 2
    weight -= self.lr * grad / (np.sqrt(self.cache) + 1e-7)
```

`AdaGrad` 를 사용하면 step size decay 를 신경쓰지 않아도 된다는 장점이 있다.

그러나, `AdaGrad` 로 학습을 오래 진행할 경우 step size 가 너무 적어져서 결국 거의 움직이지 않게 된다.


### 5. RMSProp (Root Mean Square Propagation)

`AdaGrad` 단점을 해결하기 위한 방법이다.

```python
class RMSProp(Optimizer):
def __init__(self, learning_rate=0.01, decay_rate=0.9):
    super().__init__(learning_rate)
    self.decay_rate = decay_rate
    self.cache = None

def update(self, weight, grad):
    if self.cache is None:
        self.cache = np.zeros_like(weight)
    self.cache = self.decay_rate * self.cache + (1 - self.decay_rate) * (grad ** 2)
    weight -= self.lr * grad / (np.sqrt(self.cache) + 1e-7)

```

```python
    # AdaGrad 의 변수 update
    self.cache += grad ** 2
    weight -= self.lr * grad / (np.sqrt(self.cache) + 1e-7)
```
```python
    # RMSProp 의 변수 update
    self.cache = self.decay_rate * self.cache + (1 - self.decay_rate) * (grad ** 2)
    weight -= self.lr * grad / (np.sqrt(self.cache) + 1e-7)
```

`AdaGrad` 식에서 gradient 제곱값을 더해나가면서 구한 부분을 지수 평균으로 바꾸어 대체한 방법이다. G가 무한정 커지지 않으면서 최근 변화량의 변수 간 상대적인 크기 차이를 유지할 수 있다.

### 6. Adam (Adaptive Moment Estimation)

기본 idea : `RMSProp` + `Momentum`

```python
class Adam(Optimizer):
def __init__(self, learning_rate=0.001, beta1=0.9, beta2=0.999):
    super().__init__(learning_rate)
    self.beta1 = beta1
    self.beta2 = beta2
    self.iter = 0
    self.m = None # 기울기의 지수 평균 (momentum)
    self.v = None # 기울기 제곱값의 지수 평균 (RMSProp)

def update(self, weight, grad):
    if self.m is None:
        self.m = np.zeros_like(weight)
        self.v = np.zeros_like(weight)

    self.iter += 1
    lr_t = self.lr * np.sqrt(1.0 - self.beta2 ** self.iter) / (1.0 - self.beta1 ** self.iter)

    self.m += (1 - self.beta1) * (grad - self.m)
    self.v += (1 - self.beta2) * (grad ** 2 - self.v)
    weight -= lr_t * self.m / (np.sqrt(self.v) + 1e-7)
```

`Adam` 에서는 m과 v가 처음에 0으로 초기화되어 있기 때문에 학습의 초반부에서는 0에 가깝게 bias 되어있을 것이라고 판단하여 이를 unbiased 하게 만들어주는 작업을 거친다.

<br/>

## Ensemble

- 여러 개의 모델을 독립적으로 학습시킴
- 각 모델 결과의 평균을 가지고 predict
- 평균 2% 정도의 성능 향상
- 단점 : 여러 모델 관리, 저장, 학습, test 할 때도 여러 모델을 evaluate 해야 함

<br/>

## Regularization

### Dropout

forward pass 에서 일부 뉴런을 0으로 바꾸는 기법이다.

<img src="./screenshot/08_dropout.png" width="500">

