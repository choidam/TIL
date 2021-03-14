# Binary Search Tree (이진 탐색 트리)

## 이진 탐색의 조건

1. 원소가 정렬이 되어 있을 것 (오름차순, 내림차순)
2. 원소의 Random Access 가 가능할 것

- 이미 정렬되어 있는 자료 구조에서 특정 값을 찾을 때, 탐색 범위를 절반씩 나눠가면서 해당 값을 찾아가는 것이다.
- 즉, 탐색 범위를 두 부분으로 분할하면서 찾는 방식이다.
- 처음부터 끝까지 돌면서 탐색하는 것보다 훨씬 빠르다는 장점을 가지고 있다.

<br/>

## 시간 복잡도

- 전체 탐색 : O(N)
- 이분 탐색 : O(logN)

<br/>

## 진행 순서

1. 정렬을 한다.
2.  left와 right로 mid 값을 설정한다.
3.  mid와 구하고자 하는 값을 비교한다.
4. 구할 값이 mid보다 크면 -> left = mid + 1
5. 구할 값이 mid보다 낮으면 -> right = mid - 1
6. right < left가 될 때까지 계속 반복한다.


```java
package Study;

import java.util.Arrays;

/**
 * created by victory_woo on 2020/04/30
 */
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {2, 13, 6, 5, 12, 15, 23, 17, 19, 10,};
        System.out.println(solution(17, arr));
    }

    private static int solution(int target, int[] arr) {
        Arrays.sort(arr);
        int left = 0, right = arr.length - 1, mid = 0;

        while (left < right) {
            mid = (left + right) / 2;

            if (target == arr[mid]) {
                System.out.println("Find Target : " + target + ", Value : " + arr[mid]);
                return arr[mid];
            }

            if (target < arr[mid]) right = mid - 1;
            else left = mid + 1;
        }

        return -1;
    }
}
```
