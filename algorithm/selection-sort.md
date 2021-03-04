# Selection Sort (선택 정렬)

1. 주어진 배열 중에 최소값을 찾습니다.
2. 그 값을 맨 앞에 위치한 값과 교체합니다. (pass)
3. 맨 처음 위치를 뺀 나머지 배열을 같은 방법으로 교체합니다.

## 장점

- 알고리즘이 단순하다.
- 정렬을 위한 비교는 여러번 수행되지만, 실제로 교환 횟수는 적기 때문에 많은 교환이 일어나야 하는 자료 상태에서 비교적 효율적인 면이 있다.
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다.

## 단점

- 시간 복잡도가 O(N^2)이므로 비효율적이다.
- **불안정 정렬(Unstable Sort)이다.**

```java
int[] arr = {7, 6, 2, 4, 3, 9, 1};

private static void sort(int[] arr) {
        for (int i = 0; i < arr.length; i++) { // 1
            int standard = i;
            for (int j = i + 1; j < arr.length; j++) { // 2
                if (arr[j] < arr[standard]) standard = j; // 3
            }
              
              // 4
            int temp = arr[standard];
            arr[standard] = arr[i];
            arr[i] = temp;

            print(arr);
        }
    }

private static void print(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
```

```
// 단계별 결과.
1단계 : 1 6 2 4 3 9 7 
2단계 : 1 2 6 4 3 9 7 
3단계 : 1 2 3 4 6 9 7 
4단계 : 1 2 3 4 6 9 7 
5단계 : 1 2 3 4 6 9 7 
6단계 : 1 2 3 4 6 7 9 
7단계 : 1 2 3 4 6 7 9
```
