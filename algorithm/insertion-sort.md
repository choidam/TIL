# Insertion Sort (삽입 정렬)

Insertion Sort는 Selection Sort와 유사하지만, 좀 더 효율적인 정렬 알고리즘입니다.

최선의 경우 O(N)이라는 엄청나게 빠른 효율성을 가지고 있어, 다른 정렬 알고리즘의 일부로 사용될 만큼 좋은 정렬 알고리즘입니다.

## Process (Ascending)

1. 정렬은 2번째 위치(index)의 값을 temp에 저장합니다.
2. temp와 이전에 있는 원소들과 비교하며 삽입해나갑니다.
3. '1'번으로 돌아가 다음 위치(index)의 값을 temp에 저장하고, 반복합니다.

## 장점

- 알고리즘이 단순합니다.
- 대부분의 원소가 이미 정렬되어 있는 경우, 매우 효율적일 수 있습니다.
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않습니다. => 제자리 정렬(in-place sorting)
- **안정 정렬(Stable Sort)** 입니다.
- Selection Sort나 Bubble Sort과 같은 O(n^2) 알고리즘에 비교하여 상대적으로 빠릅니다.

## 단점

- 비교적 많은 이동을 필요로 한다.
- 평균과 최악의 시간복잡도가 O(n^2)으로 비효율적입니다.
- Bubble Sort와 Selection Sort와 마찬가지로, 배열의 길이가 길어질수록 비효율적입니다.

```java
void insertionSort(int[] arr)
{
   for(int index = 1 ; index < arr.length ; index++){ // 1.
      int temp = arr[index];
      int prev = index - 1;
      while( (prev >= 0) && (arr[prev] > temp) ) {    // 2.
         arr[prev+1] = arr[prev];
         prev--;
      }
      arr[prev + 1] = temp;                           // 3.
   }
   System.out.println(Arrays.toString(arr));
}
```
