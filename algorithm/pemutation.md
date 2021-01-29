## Permutation

C++ 에서 `next_permutation` 혹은 `prev_permutaion` 함수를 통해 순열을 구할 수 있습니다.

```c++
#include <algorithm>
```

위와 같이 algorithm 헤더 파일을 추가하면 순열을 구할 수 있습니다. 

<br/>

## `Next_permutation()`

현재 나와 있는 수열에서 인자로 넘어간 범위에 해당하는 **다음 순열** 을 구하고 true를 반환합니다. 다음 순열이 없다면 (다음에 나온 순열이 순서상 이전 순열보다 작다면) false를 반환합니다.

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main(){
	vector<int> v = {1,2,3,4}

	do{
		for(int i=0; i<4; i++){
			cout << v[i] << " ";
		}
		cout << '\n';
	}while(next_permutation(v.begin(),v.end()));

	return 0;

}

```

```
1 2 3 4
1 2 4 3
1 3 2 4

...

4 2 3 1
4 3 1 2
4 3 2 1
```

<br/>

## `Prev_permutation()`

현재 나와 있는 수열에서 인자로 넘어간 범위에 해당하는 **이전 순열** 을 구하고 true를 반환합니다. 이전 순열이 없다면 (다음에 나온 순열이 순서상 이전 순열보다 크다면) false를 반환합니다.

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main(){
	vector<int> v = {1,2,3,4}

	do{
		for(int i=0; i<4; i++){
			cout << v[i] << " ";
		}
		cout << '\n';
	}while(prev_permutation(v.begin(),v.end()));

	return 0;

}

```

```
4 3 2 1
4 3 1 2
4 2 3 1

...

1 3 2 4
1 2 4 3
1 2 3 4
```

<br/>

> 응용 알고리즘 문제 : [백준 2529 - 부등호]()
