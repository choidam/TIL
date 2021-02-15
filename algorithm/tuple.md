## Tuple

C++ 에서 tuple 은 두 개 이상의 타입을 헤더 파일로 묶어주는 것을 의미합니다. **pair 의 확장 버전** 입니다.

---

### 헤더 파일

```c++
#include <tuple>
```

---

### Tuple 의 함수

- `make_tuple` : 튜플을 만드는 함수
- `get` : 튜플로부터 값을 가져오는 함수
- `swap` : 연산자 튜플의 값을 다른 변수에 전달하는 함수
-  `tie` : 튜플의 값알 가져와 값을 따로 분류할 때 사용하는 함수

---

### 사용 예시

```c++
#include <iostream>
#include <string>
#include <tuple>
using namespace std;
 
tuple<int, string> getAgeandName()
{
    int age;
    string name;
    cout << "나이를 입력하세요: ";
    cin >> age;
 
    cout << "이름을 입력하세요: ";
    cin >> name;
 
    return make_tuple(age, name);
}
 
int main()
{
    tuple<int, string> personInfo;
    personInfo = getAgeandName();
 
    cout << "나이: " << get<0>(personInfo) << endl;
    cout << "이름: " << get<1>(personInfo) << endl;
 
    return 0;
}

```

```c++
queue<tuple<int,int,int>> que;
que.push(make_tuple(0,0,0));

tie(x,y,z) = que.front();
```



