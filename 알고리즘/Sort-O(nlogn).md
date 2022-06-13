# Sorting 알고리즘의 기본개념

## 안정 정렬과 불안정 정렬
#### 안정 정렬이란, 중복된 값이 있을 때 입력 순서와 동일하게 정렬되는 것을 의미( ex) 삽입, 버블, 병합 정렬)
#### 불안정 정렬이란, 중복된 값이 있을 때 입력 순서와 관계 없이 무작위로 정렬되는 것을 의미.( ex) 선택, 퀵, 힙 정렬)

## 참조 지역성의 원리(Locality)
#### 주로 알고리즘의 효율성을 표기할 때, 빅오 표기법을 활용하는데 보다 자세히 들어가면 C x nlogn + ⍺ 라고 표기할 수 있습니다.
#### 이때 같은 nlogn이라 하더라도 상수 C 값에 의해 시간 차이가 발생할 수 있는데 C에 영향을 미치는 요소 중 하나로 참조 지역성이 있습니다. 
#### 참조 지역성이란 동일한 값, 또는 해당 값에 관계된 스토리지 위치가 자주 참조되는 특성으로 지역성의 원리라고도 불립니다.
#### 참조 지역성의 3가지 기본형으로는 시간, 공간, 순차 지역성이 있습니다. 
#### 그 중에서 공간 지역성의 경우 특정 클러스터의 기억 장소에서 참조가 집중적으로 이루어지는 경향이라고 할 수 있습니다.

# O(nlogn)의 대표적인 정렬

## 힙 정렬(Heap Sort)
#### "힙(heap)" 자료구조란 완전 이진트리의 일종으로 우선순위 큐를 위해 만들어집니다. 최댓값과 최솟값을 쉽게 구할 수 있는 자료 구조
#### 최대 힙 구조를 만들어 준다.
#### 가장 큰 수(루트 노드와 가장 작은 수의 위치를 바꿔준다)
#### 힙의 크기를 하나 줄여준 뒤의 트리를 최대 힙 구조로 다시 바꿔준다.
#### 2번과 3번을 반복한다.
#### 장점 : 추가적인 메모리 사용이 없고, 최악의 경우에도 O(nlogn)의 시간복잡도를 가진다.
#### 단점 : 불안정 정렬에 해당하며 참조 지역성 원리에 해당하지 않음.
#### Java코드 첨부

```java
import java.util.Arrays;
import java.util.Random;

public class HeapSort {

    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        heapSort(arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    private static void heapSort(int[] arr) {
        int n = arr.length;

        // maxHeap을 구성
        // n/2-1 : 부모노드의 인덱스를 기준으로 왼쪽(i*2+1) 오른쪽(i*2+2)
        // 즉 자식노드를 갖는 노트의 최대 개수부터
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i); // 일반 배열을 힙으로 구성
        }

        for (int i = n - 1; i > 0; i--) {
            swap(arr, 0, i);
            heapify(arr, i, 0); // 요소를 제거한 뒤 다시 최대힙을 구성
        }
    }

    private static void heapify(int[] arr, int n, int i) {
        int p = i;
        int l = i * 2 + 1;
        int r = i * 2 + 2;

        // 왼쪽 자식노드
        if (l < n && arr[p] < arr[l])
            p = l;
        // 오른쪽 자식노드
        if (r < n && arr[p] < arr[r])
            p = r;

        // 부모노드 < 자식노드
        if (i != p) {
            swap(arr, p, i);
            heapify(arr, n, p);
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```

## 병합 정렬(Merge Sort)
#### 배열을 반으로 나눈 후 나누어진 부분 내에서 정렬한 후 병합
#### 분할 : 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 리스트로 나눔.
#### 정복 : 나누어진 리스트를 재귀적으로 정렬
#### 결합 : 두 부분의 리스트를 다시 하나의 정렬 리스트로 병합하과 이 결과를 임시 배열에 저장
#### 복사 : 임시배열에 저장된 결과를 원래의 배열에 복사
#### 장점 : 안정 정렬에 해당하며 최악의 경우에도 O(nlogn)의 시간 복잡도를 가진다.
#### 단점 : 병합 과정에서 추가적인 메모리 필요
#### Java 코드 첨부
```java
import java.util.Arrays;
import java.util.Random;

public class MergeSort {

    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        mergeSort(0, N - 1, arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    // divide
    private static void mergeSort(int start, int end, int[] arr) {
        if (start >= end)
            return;

        int mid = (start + end) / 2;
        mergeSort(start, mid, arr); // left
        mergeSort(mid + 1, end, arr); // right

        merge(start, mid, end, arr);
    }

    // conquer
    private static void merge(int start, int mid, int end, int[] arr) {
        int[] temp = new int[end - start + 1];
        int i = start, j = mid + 1, k = 0;

        // combine
        while (i <= mid && j <= end) {
            if (arr[i] < arr[j])
                temp[k++] = arr[i++];
            else
                temp[k++] = arr[j++];
        }
        while (i <= mid)
            temp[k++] = arr[i++];
        while (j <= end)
            temp[k++] = arr[j++];

        // copy
        while (k-- > 0)
            arr[start + k] = temp[k];
    }
}
```

## 퀵 정렬(Quick Sort)
#### 분할 정복의 방식으로 정렬
#### 리스트에서 하나의 원소를 고르고 이를 pivot이라고 함.
#### pivot의 앞에는 pivot보다 작은 원소들이, 뒤에는 큰 원소들이 오도록 하고 pivot을 기준으로 두 개의 리스트로 나눔
#### 분할된 리스트에서 1번부터 2번의 과정을 리스트의 크기가 0 혹은 1이 될 때까지 반복
#### 장점 : 참조 지역성의 특성을 가지고 있어 다른 O(nlogn) 정렬들과 비교해 가장 빠른 속도를 보여줌. 또한 병합 정렬과 비슷하지만 추가 메모리 공간을 필요로 하지 않음
#### 단점 :  불안정 정렬에 해당하고, 정렬된 배열이 주어진 경우 시간복잡도가 O(n^2)로 가장 커질 수 있음
#### Java 코드 첨부
```java
import java.util.Arrays;
import java.util.Random;

public class QuickSort {
    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        quickSort(0, N - 1, arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    private static void quickSort(int start, int end, int[] arr) {
        if (start >= end)
            return;

        int left = start + 1, right = end;
        int pivot = arr[start];

        while (left <= right) {
            while (left <= end && arr[left] <= pivot)
                left++;
            while (right > start && arr[right] >= pivot)
                right--;

            if (left <= right) {
                swap(arr, left, right);
            } else {
                swap(arr, start, right);
            }
        }
        quickSort(start, right - 1, arr);
        quickSort(right + 1, end, arr);
    }

    private static void swap(int[] arr, int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```

## Tim Sort(Insertion Sort + Merge Sort)
#### 왜 팀 소트인가? -> C의 값이 너무 커지지 않도록 하면서 추가 메모리도 많이 필요하지 않음. 또한 최악의 경우에도 O(nlogn)의 시간 복잡도롤 만족시키는 정렬
#### Tim Sort에 삽입정렬(O(n^2))을 활용하는 이유는?
#### 삽입 정렬이란 새로운 원소를 기존의 정렬된 카드 사이의 올바른 자리에 찾아 삽입하는 알고리즘을 의미
#### 새로 삽입될 카드의 수만큼 반복하게 되면 전체 카드가 정렬
#### 삽입 정렬은 인접한 메모리와 비교를 반복하기 때문에 참조 지역성의 특성을 지니고 n이 작을 경우 퀵정렬보다도 빠른 성능을 보여줍니다.
#### 전체를 작은 그룹(2^x개의 원소를 가집니다.) 으로 나눠서 각각을 삽입정렬로 정렬한 뒤 병합하면 더 빠른 성능을 보여줌. 이 때, 너무 작게 나누는 경우에는 병합 동작이 많이 생기기 때문에 주로 32나 64개의 원소를 가진 그룹으로 나누어 줍니다.
#### Binary Insertion Sort : 그룹의 맨 앞 두 원소를 비교하여 증가/감소 배열을 만들지 결정합니다. 이후 각각의 리스트를 정렬하는데, 이진삽입 정렬의 경우 시간복잡도가 O(logn) 으로 일반적인 삽입정렬(O(n))보다 빠르기 때문에 이를 활용해 시간을 절약합니다. 하나의 그룹이 이미 정렬되어 있을 경우, 최악의 시간 복잡도는 O(n)이 될 수 있습니다.

![다운로드](https://user-images.githubusercontent.com/79031788/173339127-f9f3e329-1648-4298-9499-7adc943fb24d.png)

#### Merge : 스택을 사용해 특정 조건을 만족하는 병합할 그룹을 찾는다. 조건을 만족할 경우 그룹의 수를 작게 유지할 수 있고, 비슷한 크기의 그룹과 병합할 수 있음. (최소의 메모리를 이용한 최대의 효율.)
#### 장점 : 속도가 빠르며, 실제 데이터를 정렬할 때 효율적이고, 최악의 경우에도 성능 유지

![다운로드 (1)](https://user-images.githubusercontent.com/79031788/173339372-de034f52-e0fd-4e43-a8cb-149982091b44.png)
