## 유클리드 호제법

📌 2개의 자연수 또는 정식의 최대공약수를 구하는 알고리즘의 하나이다.  

📌 호제법이란 말은 두 수가 서로 상대방 수를 나누어서 결국 원하는 수를 얻는 알고리즘을 나타낸다.   

📌 2개의 자연수(또는 정식) a, b에 대해서 a를 b로 나눈 나머지를 r이라 하면(단, a>b), a와 b의 최대공약수는 b와 r의 최대공약수와 같다.
이 성질에 따라, b를 r로 나눈 나머지 r'를 구하고, 다시 r을 r'로 나눈 나머지를 구하는 과정을 반복하여 나머지가 0이 되었을 때
나누는 수가 a와 b의 최대공약수이다. 


<br/>

----

### 예시
1071과 1029의 최대 공약수를 구하면,


- 1071은 1029로 나누어떨어지지 않기 때문에 1071을 1029로 나눈 나머지를 구한다 >> 42
- 1029는 42로 나누어떨어지지 않기 때문에, 1029를 42로 나눈 나머지를 구한다 >> 21
- 42는 21로 나누어 떨어진다.

따라서, 최대공약수는 21이다.

<br/>

----

### source code

```c++
int gcd(int a, int b)
{
	int c;
	while (b != 0)
	{
		c = a % b;
		a = b;
		b = c;
	}
	return a;
}
```


### 최소공배수 구하기

```c++
int lcm(int a, int b){
   return a*b / gcd(a,b);
}
```
