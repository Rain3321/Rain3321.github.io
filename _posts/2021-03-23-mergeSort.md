---
layout: post
title: 다양한 정렬 정리(3) - 병합 정렬
comments: true
categories: algorithm
---
- - -
> 합병 정렬이라고도 부르며, **분할-정복** 방법을 통해 구현한다.
> 분할 정복이란 ```큰 문제를 작은 문제 단위로 쪼개가며 해결해나가는 방식```이다.
>
> 다음에 다룬 퀵 정렬과 함께 많이 언급되는 정렬 방식으로 ``` 안정 정렬``` 에 속한다.

- - -

### **병합 정렬**

- - - 

**병합 정렬**은 요소들을 반으로 쪼갠 이후에 다시 합병시키면서 정렬 해나가는 방식이다.
```mergeSort()``` 메소드와 ```merge()``` 메소드 2가지를 활용하여 재귀와 비슷한 형식을 띈다.

영역을 쪼갤 수 있을 만큼 쪼갠 이후에 각각 쪼갠 부분에 대해 정렬을 실행하고 다시 합치는 과정을 갖는다.


정렬 과정으로는 
```
1. 제일 작은 단위까지 분할한 이후에 분할된 중간값을 기준으로 2개의 배열을 비교한다.
2. 병합을 하기 위해 양쪽의 배열에서 가장 작은값부터 하나씩 채워나간다.
3. 병합된 배열과 비교해나가면서 채우고 한쪽 집합이 모두 끝나면 나머지 집합의 원소를 그대로 채운다.
4. 분할한 2개의 배열이 병합되면 분할된 다음 배열을 찾고 위의 과정을 반복한다.
```
### **자바 코드**
- - -

- #### mergeSort (쪼개기)

``` java

void mergeSort(int[] arr, int start, int end){
    
    if(start == end) return;
    int middle = (start + end ) /2;

    mergeSort(arr, start, middle);
    mergeSort(arr, middle+1, end);
    merge(arr, start, middle, end);
     
}
```

- ####  merge (정렬 및 합치기)

``` java
void merge(int[] arr, int start, int middle, int end){
    int[] newArr = new int[end - start + 1];
    int left = start, right = middle + 1;
    int i = 0;
    do{
        if(arr[left] < arr[right]){
            newArr[i++] = arr[left++];
        }else{
            newArr[i++] = arr[right++];
        }

    } while(left <= middle && right <= end);
    
    while(left <= middle){
        newArr[i++] = arr[left++];
    }
    while(right <= end){
        newArr[i++] = arr[right++];
    }
    System.arraycopy(newArr, 0, arr, start, newArr.length);
}

```


### **시간 복잡도**
- - -
시간복잡도는 **O(nlogn)** 이다 


- - - 
