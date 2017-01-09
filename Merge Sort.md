# Merge Sort

Merge Sort

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return void
     */
    public void sortIntegers2(int[] A) {
        // use a shared temp array, the extra memory is O(n) at least
        int[] temp = new int[A.length];
        mergeSort(A, 0, A.length - 1, temp);
    }
    
    private void mergeSort(int[] A, int start, int end, int[] temp) {
        if (start >= end) {
            return;
        }
        
        int left = start, right = end;
        int mid = (start + end) / 2;

        mergeSort(A, start, mid, temp);
        mergeSort(A, mid+1, end, temp);
        merge(A, start, mid, end, temp);
    }
    
    private void merge(int[] A, int start, int mid, int end, int[] temp) {
        int left = start;
        int right = mid+1;
        int index = start;
        
        // merge two sorted subarrays in A to temp array
        while (left <= mid && right <= end) {
            if (A[left] < A[right]) {
                temp[index++] = A[left++];
            } else {
                temp[index++] = A[right++];
            }
        }
        while (left <= mid) {
            temp[index++] = A[left++];
        }
        while (right <= end) {
            temp[index++] = A[right++];
        }
        
        // copy temp back to A
        for (index = start; index <= end; index++) {
            A[index] = temp[index];
        }
    }
}
```

