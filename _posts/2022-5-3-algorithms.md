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
## 数组
- 二维数据在内存中不是 3*4 的连续地址空间，而是四条连续的地址空间组成！」
### 二分查找
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

### 双指针法
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
### 滑动窗口
滑动窗口在于可以动态的变化窗口的左边界
209
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int length=Integer.MAX_VALUE;
        int sum=0;
        int i=0;
        int temp=0;
        for(int j=0;j<nums.length;j++){
            sum+=nums[j];
            while(sum>=target){
                temp=j-i+1;
                length=temp<length? temp:length;
                sum-=nums[i++];
            }
            // sum+=nums[--i];
        }
        return length==Integer.MAX_VALUE? 0:length;
    }
}
```

## 链表

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

```
### 虚拟头节点
203
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












# 哈希表
数组，集合，映射

# 字符串
## 拼接字符串
```java
String ss=s1+s2;
```
## kmp算法
>也就是说我们要计算子串每一个位置j对应的k，所以用一个数组next来保存，next[j] = k，表示当T[i] != P[j]时，j指针的下一个位置。另一个非常有用且恒等的定义，因为下标从0开始的，k值实际是j位前的子串的最大重复子串的长度

最常见的代码

求 next
```java

    public static int[] getNext2(String ps) {

        char[] p = ps.toCharArray();
    
        int[] next = new int[p.length];
    
        next[0] = -1;
    
        int j = 0;
    
        int k = -1;
    
        while (j < p.length - 1) {
    
           if (k == -1 || p[j] == p[k]) {
    
               if (p[++j] == p[++k]) { // 当两个字符相等时要跳过
    
                  next[j] = next[k];
    
               } else {
    
                  next[j] = k;
    
               }
    
           } else {
    
               k = next[k];
    
           }
    
        }
    
        return next;
    
    }
   
    
```
常见的改进过的代码

```java
 public static int[] getNext3(String ps) {
 
        char[] p = ps.toCharArray();
    
        int[] next = new int[p.length];
    
        next[0] = -1;
    
        int j = 0;
    
        int k = -1;
    
        while (j < p.length - 1) {
    
           if (k == -1 || p[j] == p[k]) {
    
               next[++j] = ++k;
    
           } else {
    
               k = next[k];
    
           }
    
        }
    
        return next;
    
    }

```

自己写的
```java
 public void getNext(char[] s, int[] next) {
        int k = -1;
        int j = 0;
        next[0] = -1;
        while (j < s.length-1) {
            while (k >= 0 && s[k] != s[j]) {
                k = next[k];
            }
            next[++j] =++k;
        }
    }

```
kmp

```java

 public int kmpSubstr(String s1, String s2) {
        char[] ss1 = s1.toCharArray();
        char[] ss2 = s2.toCharArray();
        int[] next = new int[s2.length()];
        getNext(ss2, next);
        System.out.println(Arrays.toString(next));
        System.out.println(Arrays.toString(getNext2(s2)));
        System.out.println(Arrays.toString(getNext3(s2)));
        int i = 0, j = 0;
        while (i < s1.length() && j < s2.length()) {
            // int k=j;

            if (j!=-1&&ss2[j] != ss1[i])
                j = next[j];
            else {
                i++;
                j++;
            }

        }
        if(j==ss2.length){
            return i-ss2.length;
        }
        else{
            return -1;
        }
    }

   
```

# 二叉树
```java
public class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    public Treenode(val){this.val=val;left=null;right=null;}
}

```
## 遍历
### 前序遍历
>中左右
#### 递归
```java
public void preorder(TreeNode root){
    if(root==null)return ;
    System.out.println(root.val);
    preorder(root.left);
    preorder(root.right);
}
```
#### 栈
```java
public List<Integer> preorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TreeNode> st=new Stack<TreeNode>();
    
    st.push(root);
    while(!st.empty()){
        TreeNode tn=st.pop();
        result.add(tn.val);
        if(tn.right!=null)st.push(tn.right);
        if(tn.left!=null)st.push(tn.left);
    }
    return result;
}
```

#### 栈，可转化
```java
public List<Integer> preorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TeeeNode> st=new Stack<TreeNode>();
    st.push(root);
    while(!st.empty()){
        TreeNode tn=st.peek();
        if(tn!=null){
            tn=st.pop();
            if(tn.right!=null) st.push(tn.right);
            if(tn.left!=null) st.push(tn.left);
            st.push(tn);
            st.push(null);
        }else{
            st.pop();
            tn=st.pop();
            result.add(tn.val);
        }
    }
    return result;
}   

```
#### 
### 中序遍历
>左中右
#### 递归
```java
public void  preorder(TreeNode root){
    if(root==null)return ;
    
    preorder(root.left);
    System.out.println(root.val);
    preorder(root.right);
}
```
#### 栈
```java
public List<Integer>  inorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TreeNode> st=new Stack<TreeNode>();
    // st.push(root);
    TreeNode cur=root;
    while(cur!=null||!st.empty()){
        if(cur!=null){
            st.push(cur);
             cur=cur.left;
        }else{
            cur=st.pop();
            result.add(cur.val);
            // st.push(tn.left);
            cur=cur.right;
        }  
    }
    return  result;
}
```


#### 栈，可转化
```java
public List<Integer> preorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TeeeNode> st=new Stack<TreeNode>();
    st.push(root);
    while(!st.empty()){
        TreeNode tn=st.peek();
        if(tn!=null){
            tn=st.pop();
            if(tn.right!=null) st.push(tn.right);
            st.push(tn);
            st.push(null);
            if(tn.left!=null) st.push(tn.left);
        }else{
            st.pop();
            tn=st.pop();
            result.add(tn.val);
        }
    }
    return result;
}   

```
### 后序遍历
>左右中
#### 递归
```java
public preorder(TreeNode root){
    if(root==null)return ;
    
    preorder(root.left);
    preorder(root.right);
    System.out.println(root.val);
}
```
#### 栈
```java
public  List<Integer> postorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TreeNode> st=new Stack<TreeNode>();
    st.push(root);
    while(!st.empty()){
        TreeNode tn=st.pop();
        result.add(tn.val);
       
        if(tn.left!=null)st.push(tn.left); 
        if(tn.right!=null)st.push(tn.right);
    }
     
    Collections.reverse(result);return result;
}
```
#### 栈，可转化
```java
public List<Integer> preorder(TreeNode root){
    List<Integer> result=new ArrayList<>();
    if(root==null)return result;
    Stack<TeeeNode> st=new Stack<TreeNode>();
    st.push(root);
    while(!st.empty()){
        TreeNode tn=st.peek();
        if(tn!=null){
            tn=st.pop();
            if(tn.right!=null) st.push(tn.right);
            st.push(tn);
            st.push(null);
            if(tn.left!=null) st.push(tn.left);
        }else{
            st.pop();
            tn=st.pop();
            result.add(tn.val);
        }
    }
    return result;
}   

```
### 层次遍历
#### queue.size()
```java
public List<List<Integer>> levelOrder(Node root) {
    List<List<Integer>> result=new LinkedList<>();
    Queue<Node>q=new LinkedList<>();
    if(root!=null) q.offer(root);
    while(!q.isEmpty()){
        int k= q.size();
        List<Integer> newl=new LinkedList<>();
        Node node;
        for(int i=0;i<k;i++){
            node=q.poll();
            newl.add(node.val);
            for(Node n:node.children){
                if(n!=null)
                q.offer(n);
            }
        }
        result.add(newl);
    }
    return result;
}

```
#### last存储
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    Queue<TreeNode> q=new LinkedList<>();
    List<List<Integer>> result=new ArrayList<>();
    if(root==null)return result;
    TreeNode last=null;
    q.offer(root);
    while(!q.isEmpty()){
        TreeNode rear=q.poll();
        TreeNode tail=last;
        List<Integer> newl=new ArrayList<>();
        while(rear!=tail){
            if(rear==null)continue;
            newl.add(rear.val);
            if(rear.left!=null){q.offer(rear.left);last=rear.left}
            if(rear.right!=null){q.offer(rear.right);last=rear.right}
        }
        reault.add(newl);
    }
}
```

## 回溯算法
```java
public List<List<String>> results=new ArraysList<>();
public List<String> result=new ArraysList<>();
public void trace(){
    if()
    return ;
    for(){

    }
}
public List<List<String>> fun(){
    if(){

    }
    trace();
    return results;
}
```

## 贪心算法
尝试寻找一个比较合适局部最优算法推广到全局最优，并且找不到反例，可以试一试。

在没有目标的情况下，可以试一试解一下题目，找到对多种例子可行的方案。可以尝试分情况讨论

对存在多个维度、方向的问题，不要一次综合两个维度，会顾此失彼，尝试每一次只针对一个维度。

## 动态规划

- 确定dp数组以及下标的含义
- 确定递推公式
- dp数组如何初始化
- 确定遍历顺序
- 举例推导dp数组
  
### 01背包

我们知道01背包内嵌的循环是从大到小遍历，为了保证每个物品仅被添加一次。

```java
for(int i = 0; i < weight.length; i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = dp[j]>dp[j - weight[i]] + value[i] ?dp[j]:dp[j - weight[i]] + value[i];
    }
}

```

### 完全背包
```java
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}

```

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。



