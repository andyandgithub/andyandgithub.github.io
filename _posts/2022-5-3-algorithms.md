---
layout: post
title: 数据结构与算法 以Java为例
categories: study
tags : algorithms
toc: true
---
# 算法复杂度
## 时间复杂度
O(1)常数阶 < O(logn)对数阶 < O(n)线性阶 < O(n^2)平方阶 < O(n^3)(立方阶) < O(2^n) (指数阶)
## 空间复杂度
# 算法
## 二分查找
前提是已经有序的数组 35
```java
public int searchInsert(int []nums,int target){}
    int head=0,tail=nums.length-1,mid=tail/2;
    // int index=0;
    if(target>nums[tail])
        return tail+1;
    if(target<nums[head])
        return 0; 
    while(head<=tail){
        if(nums[mid]==target)
            return mid;
        if(nums[mid]<target){
            head=mid+1;
            mid=head+(tail-head)/2;//防止溢出，等同于(tail-head)/2

        }else{
            tail=mid-1;
            mid=head+(tail-head)/2;
        }
    }
    return head;
    //返回数组序列的索引
``` 

## 双指针法
安静两个循环变成一个循环 27
```java
public int removeElement(int[] nums, int val) {
    if(nums.length==0){
        return 0;
    }
    int first=0;//last=0;
    for(int last=0;last<nums.length;last++){
        if(nums[last]==val){
            
            continue;
        }
        nums[first++]=nums[last];
    }
    return first;
}
```
## 排序
### 快速排序
```java
 public void quickSort(int [] nums,int low,int high){
        if(low>=high)
        return ;
        int index=nums[low],left=low,right=high,temp;
        while(left<right){
            while(left<right&&nums[right]>=index){//必须先循环右边
                right--;
            }
            while(left<right&&nums[left]<=index){
                left++;
            }
            temp=nums[left];
            nums[left]=nums[right];
            nums[right]=temp;
        } 
        nums[low]=nums[left];
        nums[left]=index;
       
        quickSort(nums, low, left-1);
        quickSort(nums, left+1, high);
    }

```











