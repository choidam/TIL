# Perceptron & Neural Network

> 강의 자료 : 딥러닝 (2020-1) 

## Perceptron

다수의 신호 (Input) 을 입력받아서 하나의 신호 (Output) 을 출력한다.    
perceptron 의 출력값은 1 또는 0 이기 때문에 선형 분류 (linear classifier) 모형이다.

<img src="./screenshot/04_nn4.png" width="700">

<br/>

처음에는 임의로 설정된 weight로 시작한다.   
학습 데이터를 perceptron 모형에 입력하며 분류가 잘 못 되었을 때 weight을 개선해 나간다. 

- **가중치** (weight) : 입력 신호가 결과 출력에 주는 영향도를 조절하는 매개변수
- **변향** (bias) : 뉴런이 얼마나 쉽게 활성화 되느냐를 조절하는 매개변수

퍼셉트론은 선형분류는 할 수 있지만 비선형분류는 불가능하다는 한계점이 있다.   
이를 극복하기 위해 다층 퍼셉트론을 만든다. 층을 겹겹이 쌓아나가는 것이다.   

단층 퍼셉트론은 `step function` (임계값을 넘어섰을 때 출력을 1로 함) 을 활성화 함수로 사용한다.   
다층 퍼셉트론은 층이 여러개이며 `sigmoid function` 을 활성화 함수로 사용한다.

<br/>

## Neural Network

Perceptron이 가중치를 직접 수동으로 설정하는 작업을 한다는 한계점을 해결한 방법이 **신경망** 이다.   
알아서 가중치 값을 설정하고 조정하는 것이 (자동으로 학습) 신경망의 큰 특징이다

<img src="./screenshot/04_nn6.png" height="200"> <img src="./screenshot/04_nn7.png" height="200">

신경망은 Input(입력층), Hidden(은닉층), Output(출력층) 으로 표현할 수 있다.

<br/>

### ReLU Function

<img src="./screenshot/04_nn9.png" width="300">

입력이 0을 넘으면 그 입력을 그대 출력하고, 0 이하면 0을 출력하는 함수이다.


### Sigmoid Function 

<img src="./screenshot/04_nn5.png" width="200">

<img src="./screenshot/04_nn8.png" width="400">

가중치 값을 전달할 때 좀 더 부드럽게 양을 조절해 전달한다.

```python
import numpy as np
import matplotlib.pylab as plt

def sigmoid(x):
    return 1 / (1+np.exp(-x))

print(sigmoid(1))
print(sigmoid(np.array([0,0.5,1])))

x = np.arange(-10.0, 10.0, 0.1)
y = sigmoid(x)

plt.plot(x,y)
plt.ylim(-0.1, 1.1)
plt.show()
```
```profile
0.7310585786300049
[0.5        0.62245933 0.73105858]
```

<img src="./screenshot/04_nn.png" width="500">

<br/>

### 📌 단순 신경망 구조

<img src="./screenshot/04_nn2.png" width="400">

```python
import numpy as np

# 단순 신경망 구조
def init_network():
    network = {}
    network['W'] = np.array([
        [0.2, 0.5, 0.3],
        [0.8, 0.6, 0.4]
    ])
    return network

network = init_network()
```

<br/>

### 📌 Forward Pass

```python
# forward pass 1
def forward1(network, x):
    W = network['W']
    y = np.array([0.0, 0.0, 0.0])
    y[0] = W[0][0] * x[0] + W[1][0] * x[1]
    y[1] = W[0][1] * x[0] + W[1][1] * x[1]
    y[2] = W[0][2] * x[0] + W[1][2] * x[1]
    return y
    
# forward pass 2
def forward2(network,x):
    y = np.dot(x, network['W'])
    return y
    
network = init_network()
y = forward(network, np.array([1.0, 2.0]))
print(y)
```
```profile
[1.8, 1.7, 1.1]
```
<br/>

### 📌 Active Function 적용

```python
import numpy as np

# 단순 신경망 구조
def init_network():
    network = {}
    network['W'] = np.array([
        [0.2, 0.5, 0.3],
        [0.8, 0.6, 0.4]
    ])
    return network

network = init_network()

# forward pass 1
def forward1(network, x):
    W = network['W']
    y = np.array([0.0, 0.0, 0.0])
    y[0] = W[0][0] * x[0] + W[1][0] * x[1]
    y[1] = W[0][1] * x[0] + W[1][1] * x[1]
    y[2] = W[0][2] * x[0] + W[1][2] * x[1]
    return y


# forward pass 2
def forward2(network,x):
    y = np.dot(x, network['W'])
    return y

# sigmoid : 출력값을 0~1로 조정
def sigmoid(x):
    return 1 / ( 1 + np.exp(-x) )

# forward + sigmoid
def forward3(network,x):
    y = sigmoid(np.dot(x, network['W']))
    return y

# softmax : 마지막 출력층. 출력의 합계는 1
# overflow 조정해줌
# 마지막 출력층에서 활성화 함수로 사용 
def softmax(x)  :
    e = np.exp(x - np.max(x))
    s = np.sum(e)
    return e / s


y = forward2(network, np.array([1.0, 2.0]))
print(y)
print()

y2 = forward3(network, np.array([1.0, 2.0]))
print(y2)
print()
```
```profile
[1.8 1.7 1.1]

[0.85814894 0.84553473 0.75026011]
```

<br/>

### 📌 SoftMax Function

- Neural Network 기반 multi-label classifier 의 마지막 층에 사용
- Cross - entropy loss 와 함께 쓰임
- 출력 값이 0부터 1 사이의 값
- 출력의 합계는 1

```python
# overflow 조정해줌
# 마지막 출력층에서 활성화 함수로 사용 
def softmax(x)  :
    e = np.exp(x - np.max(x))
    s = np.sum(e)
    return e / s
```
