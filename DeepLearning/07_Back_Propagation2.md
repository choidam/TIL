# Back Propagation (2)

> 선수 환경 : [여기](https://github.com/ChoiEunji0114/TIL/blob/master/DeepLearning/07_Back_Propagation.md)

앞서 만든 layer 들을 가지고 network 를 만들어보자❗️

1. Network

```python
class Network1:
def __init__(self):
    self.layers = []

def add(self,layer):
    self.layers.append(layer)

def predict(self, x):
    for layer in self.layers:
        x = layer.forward(x)
    return x

# 마지막 layer(softmax) 의 loss값
def loss(self, t):
    return self.layers[-1].loss(t) 

def accuracy(self, x, t):
    y = self.predict(x)
    y = np.argmax(y, axis=1)
    if t.ndim !=1 : t = np.argmax(t, axis=1)
    return np.sum(y==t) / float(x.shape[0])

def forward_pass(self, x):
    self.predict(x)

def backward_pass(self, learning_rate):
    d_out = 1 # 가장 바깥의 gradient 는 언제나 1이다.
    for layer in reversed(self.layers):
        d_out = layer.backward(d_out, learning_rate)

def evaluate(self, x_test, t_test):
    test_acc = self.accuracy(x_test, t_test)
    print("Test accuracy = {0}".format(test_acc))

def train(self, x_train, t_train):
    batch_size = 128 # 한 번에 처리할 양 (임의로 데이터를 뽑아 학습시킬 양)
    epoches = 20 # back - forward 횟수 (전체 데이터를 도는 횟수)
    train_size = x_train.shape[0]
    learning_rate = 0.1 # hyper parameter
    train_errors = []
    train_acc_list = []
    iter_per_epoch = int(math.ceil(train_size / batch_size)) # 한 번 돌 때 사이즈
    for epoch in range(1, epoches+1):
        print("Epoch {0}/{1}".format(epoch, epoches))
        for _ in range(iter_per_epoch):
            batch_mask = np.random.choice(train_size, batch_size) # 데이터를 랜덤하게 뽑음
            x_batch = x_train[batch_mask] # 문제
            t_batch = t_train[batch_mask] # 정답

            self.forward_pass(x_batch)
            loss = self.loss(t_batch)
            train_errors.append(loss)
            self.backward_pass(learning_rate)
        train_acc = self.accuracy(x_train, t_train)
        train_acc_list.append(train_acc)
        print("Train accuracy={0}".format(train_acc))
    return train_errors

def plot_error(self, train_errors):
    n = len(train_errors)
    training, = plt.plot(range(n), train_errors, label="Training Error")
    plt.legend(handles=[training])
    plt.title("Error plot")
    plt.ylabel('Error')
    plt.xlabel('Iterations')
    plt.show()

def show_failures(self, x_test, t_test):
    y = self.predict(x_test)
    y = np.argmax(y, axis=1)
    if t_test.ndim !=1 : t_test = np.argmax(t_test, axis=1)
    failures = []
    for idx in range(x_test.shape[0]) :
        if y[idx] != t_test[idx]:
            failures.append([x_test[idx], y[idx], t_test[idx]])
    for i in range(min(len(failures), 60)):
        img, y, _ = failures[i]
        if (i%10 ==0 ) : print()
        print(y, end=", ")
        img = img.reshape(28,28)
        plt.subplot(6,10,i+1)
        plt.imshow(img, cmap='gray')
    print()
    plt.show()
```


<br/>

2. Add Layers to Network

```python
network = Network1()
network.add(Dense(784, 50)) # input layer
network.add(Relu()) # activation function
network.add(Dense(50,10)) # output layer
network.add(SoftmaxWithLoss())
```
hidden layer1개, output layer 1개 만듦

<br/>

3. Visualization

<img src="./screenshot/07_back1.png" width="600">

위는 에러율 그래프이다. 시간이 지날수록 점차 낮아짐을 확인할 수 있다.

```profile
Epoch 1/20
Train accuracy=0.9028666666666667
Epoch 2/20
Train accuracy=0.92345
Epoch 3/20
Train accuracy=0.9360166666666667
Epoch 4/20
Train accuracy=0.9419833333333333
Epoch 5/20
Train accuracy=0.9462833333333334
Epoch 6/20
Train accuracy=0.95145
Epoch 7/20
Train accuracy=0.955
Epoch 8/20
Train accuracy=0.9594333333333334
Epoch 9/20
Train accuracy=0.9628333333333333
Epoch 10/20
Train accuracy=0.9648
Epoch 11/20
Train accuracy=0.9667833333333333
Epoch 12/20
Train accuracy=0.9689333333333333
Epoch 13/20
Train accuracy=0.9703833333333334
Epoch 14/20
Train accuracy=0.9724833333333334
Epoch 15/20
Train accuracy=0.9733333333333334
Epoch 16/20
Train accuracy=0.9740666666666666
Epoch 17/20
Train accuracy=0.9756333333333334
Epoch 18/20
Train accuracy=0.9764166666666667
Epoch 19/20
Train accuracy=0.9774666666666667
Epoch 20/20
Train accuracy=0.9778166666666667
Test accuracy = 0.9686
```

위는 한 epoch을 돌 때마다 정답률을 프린트 한 것이다. 정답률이 점차 높아짐을 확인할 수 있다.


<img src="./screenshot/07_back1.png" width="600">

위는 분류에 실패한 예의 이미지를 그린 것이다.
