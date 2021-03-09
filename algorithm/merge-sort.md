# Merge Sort (병합 정렬)

**분할 정복** 방법을 통해 구현한다.

### 1. 분할(Divide)

- 입력 배열을 같은 크기의 2개의 부분 배열로 분할한다.

### 2. 정복(Conquer)

- 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 **순환 호출**을 이용하여 다시 분할 정복 방법을 적용한다.

### 3. 결합(Combine)

- 정렬된 부분 배열들을 하나의 배열로 합병한다.

<br/>

## 장점

- 데이터의 분포에 영향을 덜 받는다. 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. -> O(N logN)
- 크기가 큰 레코드를 정렬한 경우, LinkedList를 사용한다면 병합 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 효율적이다.
- 안정 정렬에 속한다.

<br/>

## 단점

- 레코드를 배열로 구성한다면, 임시 배열이 필요하다. (추가적인 리스트가 필요)
- → 메모리 낭비를 초래한다.
- → 제자리 정렬이 아니다.
- 레코드의 크기가 큰 경우에는 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래한다.

<br/>

```java
private static void mergeSort(int[] a, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;

            mergeSort(a, left, mid); // 왼쪽 부분 배열을 나눈다.
            mergeSort(a, mid + 1, right); // 오른쪽 부분 배열을 나눈다.
            merge(a, left, mid, right);
            // 나눈 부분 배열을 합친다.
            // 핵심 로직이다.
        }
    }
```

```java
/*
    * i : 왼쪽 부분 배열을 관리하는 인덱스
    * j : 오른쪽 부분 배열을 관리하는 인덱스
    * */
    private static void merge(int[] a, int left, int mid, int right) {
        int i, j, k, l;
        i = left;
        j = (mid + 1);
        k = left;

        // 왼쪽 부분 배열과 오른쪽 부분 배열을 비교하면서
        // 각각의 원소 중에서 작은 원소가 sorted 배열에 들어간다.
        // 왼쪽 혹은 오른쪽의 부분 배열 중 하나의 배열이라도 모든 원소가 sorted 배열에 들어간다면
        // 아래의 반복문은 조건을 만족하지 하지 않기 때문에 빠져 나온다.
        // sorted 배열에 들어가지 못한 부분 배열은 아래 쪽에서 채워진다.
        while (i <= mid && j <= right) {
            if (a[i] < a[j]) sorted[k++] = a[i++];
            else sorted[k++] = a[j++];
        }

        // mid < i 인 순간, i는 왼쪽 부분 배열의 인덱스를 관리하므로
        // 즉, 왼쪽 부분 배열이 sorted 에 모두 채워졌음을 의미한다.
        // 따라서 남아 있는 오른쪽 부분 배열의 값을 일괄 복사한다.
        if (mid < i) {
            for (l = j; l <= right; l++) sorted[k++] = a[l];
        } else {
            // mid > i 인 것은 오른쪽 부분 배열이 sorted 배열에 정렬된 상태로 모두 채워졌음을 의미한다.
            // 따라서, 왼쪽 부분 배열은 아직 남아 있는 상태이다.
            // 즉, 남아 있는 왼쪽 부분 배열의 값을 일괄 복사한다.
            for (l = i; l <= mid; l++) sorted[k++] = a[l];
        }

        // sorted 배열에 들어간 부분 배열을 합하여 정렬한 배열은
        // 원본 배열인 a 배열로 다시 복사한다.
        for (l = left; l <= right; l++) a[l] = sorted[l];
    }
```

<br/>

### **시간 복잡도**

- 평균 : O(N logN)
- 최악 : O(N logN)
- 최선 : O(N logN)
