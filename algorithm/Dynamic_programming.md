## Dynamic Programming

DP(Dynamic Programming, 동적 계획법) 이란 복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법을 말합니다. 이것은 부분 문제 반복과 최적 부분 구조를 가지고 있는 알고리즘을 일반적인 방법에 비해 더욱 적은 시간 내에 풀 때 사용합니다.

DP 의 원리는 일반적으로 주어진 문제를 풀기 위해서, 문제를 여러 개의 하위 문제(subproblem)로 나누어 푼 다음, 그것을 결합하여 최종적인 목적에 도달하는 것입니다. 각 하위 문제의 해결을 계산한 뒤, 그 해결책을 저장하여 후에 같은 하위 문제가 나왔을 경우 그것을 간단하게 해결하여 계산 횟수를 줄일 수 있습니다. 특히 이 방법은 하위 문제의 수가 기하급수적으로 증가할 때 유용합니다.

<br/>

## 예제 : 피보나치 수열

보통 피보나치 수열을 구하는 함수는 다음과 같이 작성합니다.

```javascript
function fib(n)
 if n = 0
  return 0
 else if n=1
  return 1
 else
  return fib(n-1) + fib(n-2)
```

이때, fib(5)를 구한다고 한다면 계산은 다음과 같이 이루어집니다.

1. `fib(5)`
2. `fib(4) + fib(3)`
3. `(fib(3) + fib(2)) + (fib(2) + fib(1))`
4. `((fib(2) + fib(1)) + (fib(1) + fib(0))) + ((fib(1) + fib(0)) + fib(1))`
5. `(((fib(1) + fib(0)) + fib(1)) + (fib(1) + fib(0))) + ((fib(1) + fib(0)) + fib(1))`

여기에서 세 번째 줄의 fib(2)가 중복되어 계산되고, 이것은 전체적인 계산 속도를 떨어뜨린다. 이 알고리즘의 시간 복잡도는 지수 함수가 됩니다.여기에서 각 함수의 계산값을 저장하는 객체 *m*을 추가하면, 이 알고리즘은 다음과 같이 바뀝니다.

```javascript
var m := map(0 → 1, 1 → 1)
```

```javascript
function fib(n)
 if n not in keys(m)
  m[n] := fib(n-1) + fib(n-2)
 return m[n]
```

이렇게 각 계산값을 저장하면, 중복 계산이 줄어들고 시간 복잡도는 [O](https://ko.wikipedia.org/wiki/점근_표기법)(n)이 됩니다.

<br/>

> 참고하면 좋은 알고리즘 문제 : [백준 10870 : 피보나치수 5](https://github.com/choidam/Algorithm-study/blob/master/posts/boj-10870.md) , [백준 11052: 카드 구매하기](https://github.com/choidam/Algorithm-study/blob/master/posts/boj-11052.md), [백준 1003: 피보나치수](https://silver-g-0114.tistory.com/11)

