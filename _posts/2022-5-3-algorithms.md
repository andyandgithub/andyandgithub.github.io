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
        
## 回溯算法

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

回溯算法的基本操作的就是通过递归来`枚举`，通常是由`排列`和`组合`等不同的算法思想。
回溯枚举的基本操作展开就是一个树，通过不同的操作剪辑。
去重的操作方法包含有 `树枝` 去重， `树层`去重

回溯算法需要注意点
- 在for循环的截止条件中，可以改变以减少已经不可能的分支，缩短时间
- 在for循环的起始条件`i=start`中，start的值可以用来区分多种操作,子序列，集合，排列组合等
- trace函数一般为没有返回值，有时候需要返回`boolean`类型进行条件判断

```java
public List<List<String>> results=new ArraysList<>();
public List<String> result=new ArraysList<>();
public void trace(int start){
    if()
    return ;
    for(i=start;条件;i++){
        if(条件){
           
              
            result.add(nums[i]);
            trace(i+1);
            result.remove(result.size()-1);

         
        }
    }
}
public List<List<String>> fun(){
    if(){

    }
    trace();
    return results;
}
```


### 类型
#### 排列

### 剪枝
主要目的是去除掉已经无用枝杈防止递归下去耗费时间
主要在for循环的判断语句中剪


`i<=n`与`i<=n-(k-result.size())+1`去除掉已经不能够达成条件的分支
```java
import java.util.LinkedList;
import java.util.List;

class Solution {
    List<List<Integer>>results;
    List<Integer> result;
    public void trace(int start,int n,int k){
        // if(start>n) return ;
        if(result.size()==k){
            results.add(new ArrayList<>(result));
            return;
        }
        for(int i=start;i<=n-(k-result.size())+1;i++){//剪断，去除剩下去除剩下即使全部加上也不够个数的分支
            result.add(i);
            trace(i+1, n, k);
            result.remove(result.size()-1);
        }
    }
    public List<List<Integer>> combine(int n, int k) {
        results=new ArrayList<>();
        result=new ArrayList<>();
        trace(1,n,k);
        return results;
    }
}

```




### 去重
#### 树枝去重

```java
public List<List<String>> results=new ArraysList<>();
public List<String> result=new ArraysList<>();
boolean []used;
public void trace(){
    if()
    return ;

    if()
    results.add(new Linked<>(result));

    //树枝去重
    for(int i=start;;i++){
        if(条件){
            if(!used[i]){
                used[i]=true;
                result.add(nums[i]);
                trace(i+1);
                result.remove(result.size()-1);
                used[i]=faLse;
            }
        }
    }
}
public List<List<String>> fun(){
    used=new boolean[size];
    if(){

    }
    trace();
    return results;
}
```

#### 树层去重
```java
public List<List<String>> results=new ArraysList<>();
public List<String> result=new ArraysList<>();
boolean []used;
public void trace(){
    if()
    return ;

    if()
    results.add(new Linked<>(result));

    boolean []usedtemp=Arrays.copyOfRange(used,0,size);//本层去重
    for(int i=start;;i++){
        if(条件){
            if(!usedtemp[i]){
                usedtemp[i]=true;
                result.add(nums[i]);
                trace(i+1);
                result.remove(result.size()-1);

            }
        }
    }
}
public List<List<String>> fun(int []nums){
    used=new boolean[size];
    if(){

    }
    Arrays.sort(nums);
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


## 单调栈
通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了。

## 前缀和

```java
import javax.naming.NamingEnumeration;

/*
 * [303] 区域和检索 - 数组不可变
 */
class NumArray {

   public int [] sums;
    public NumArray(int[] nums) {
       
        sums=new int[nums.length+1];
        sums[0]=nums[0];
        for(int i=0;i<nums.length;i++){
            sums[i+1]=sums[i]+nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
       
        return sums[right+1]-sums[left];
    }
}

```

### 常用技巧
#### 求子数组
- 一般计算`left`和`right`数组，左到右、右到左的连乘、连加的时候
就是某个位置左右两边的和或者乘积的时候。
- 使用`Deque`双端队列保存一个滑动窗口
- 前缀和与哈希表一起使用
## 线段树
可用数组实现，一般大小为原数组的四倍大小。

```java


```

## 树状数组











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


# 图
## 遍历
Bfs
```java
//以多个点开始遍历
for(int i=0;graph.length;i++){
    LinkedList<Integer> path=new LinkedList<>();
    trace(graph,i,path);
}
// 记录被遍历过的节点
boolean[] visited;
// 记录从起点到当前节点的路径
boolean[] onPath;

/* 图遍历框架 */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // 经过节点 s，标记为已遍历
    visited[s] = true;
    // 做选择：标记节点 s 在路径上
    onPath[s] = true;
    for (int neighbor : graph.neighbors(s)) {
        traverse(graph, neighbor);
    }
    // 撤销选择：节点 s 离开路径
    onPath[s] = false;
}
```

##  并查集
> Union-Find 算法，也就是常说的并查集（Disjoint Set）结构，主要是解决图论中「动态连通性」问题的


```java
class UF {
    /* 将 p 和 q 连接 */
    public void union(int p, int q);
    /* 判断 p 和 q 是否连通 */
    public boolean connected(int p, int q);
    /* 返回图中有多少个连通分量 */
    public int count();

    
     // 记录连通分量
    private int count;
    // 节点 x 的父节点是 parent[x]
    private int[] parent;

    /* 构造函数，n 为图的节点总数 */
    public UF(int n) {
        // 一开始互不连通
        this.count = n;
        // 父节点指针初始指向自己
        parent = new int[n];
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }


    /* 返回当前的连通分量个数 */
    public int count() { 
        return count;
    }

}
```

需要实现的三个函数`count`，`union`，`connected`
主要`union`,`connected`


最重要的函数`find`寻找一个节点的根节点，不是父节点
```java
public void union(int p,int q){
    int rootp=find(p);
    int rootq=find(q);
    if(rootp==rootq){
        return ;
    }else{
        parent[rootp]=rootq;
        count--;
    }
   
}


public boolean connected(int p,int q){
    int rootp=find(p);
    int rootq=find(q);
    return rootp==rootq;
}


public int find(int x){
    while(x!=parent(x)){
        x=parent[x];
    }
    return x;
}
```
### 平衡性优化
#### 希望是小树挂在大树下面
引入size数组记录每个节点之下的节点数量

```java
public int[]size;
/* 构造函数，n 为图的节点总数 */
public UF(int n) {
    // 一开始互不连通
    this.count = n;
    // 父节点指针初始指向自己
    parent = new int[n];
    size=new int[n];
    for (int i = 0; i < n; i++){
         parent[i] = i;
         size[i]=1;
    }
       
}


```
修改过后的新`union`

```java
public void union(int p,int q){
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ)
        return;
    
    // 小树接到大树下面，较平衡
    if (size[rootP] > size[rootQ]) {
        parent[rootQ] = rootP;
        size[rootP] += size[rootQ];
    } else {
        parent[rootP] = rootQ;
        size[rootQ] += size[rootP];
    }
    count--;
}
```
通过比较树的重量，就可以保证树的生长相对平衡，树的高度大致在 logN 这个数量级，极大提升执行效率。

此时，find , union , connected 的时间复杂度都下降为 O(logN)，即便数据规模上亿，所需时间也非常少。


#### 压缩路径

这样每个节点的父节点就是整棵树的根节点，find 就能以 O(1) 的时间找到某一节点的根节点，相应的，connected 和 union 复杂度都下降为 O(1)。

要做到这一点主要是修改 find 函数逻辑，非常简单，但你可能会看到两种不同的写法。

第一种是在 find 中加一行代码：
```java
private int find(int x) {
    while (parent[x] != x) {
        // 这行代码进行路径压缩
        parent[x] = parent[parent[x]];
        x = parent[x];
    }
    return x;
}

```
```java
// 第二种路径压缩的 find 方法
每个分量应该只有一个根节点，其余所有的节点都挂靠在这个节点上面，这就不要太需要size数组了

public int find(int x) {
    if (parent[x] != x) {
        parent[x] = find(parent[x]);
    }
    return parent[x];
}

```

### 常见并查集算法
```java
class UF {
    // 连通分量个数
    private int count;
    // 存储每个节点的父节点
    private int[] parent;

    // n 为图中节点的个数
    public UF(int n) {
        this.count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    // 将节点 p 和节点 q 连通
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        
        if (rootP == rootQ)
            return;
        
        parent[rootQ] = rootP;
        // 两个连通分量合并成一个连通分量
        count--;
    }

    // 判断节点 p 和节点 q 是否连通
    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    // 返回图中的连通分量个数
    public int count() {
        return count;
    }
}


```




## 最小生成树算法
> 包含图的所有顶点和部分边的树
> 在图的所有生成树当中，权重最小的那棵树。

- 包含全部节点
- 权重最小
- 没有环

### 并查集算法

并查集实现kruskal算法
```java
edge//边的集合
Collections.sort(edge,(a,b)->{
    return a[2]-b[2];
});
imt sum=0;//总权重
for(int [] e:edge){
    if(!connected(e[0],e[1])){
        union(i,j);
        sum+=e[2];//此条边权重
    }
}
```
### Kruskal算法
> 贪心算法思想
将所有边按照权重从小到大排序，从权重最小的边开始遍历，如果这条边和mst中的其它边不会形成环，则这条边是最小生成树的一部分，将它加入mst集合；否则，这条边不是最小生成树的一部分，不要把它加入mst集合。
### Prim算法
> 贪心算法 切分定理

Kruskal 算法是在一开始的时候就把所有的边排序，然后从权重最小的边开始挑选属于最小生成树的边，组建最小生成树。

Prim 算法是从一个起点的切分（一组横切边）开始执行类似 BFS 算法的逻辑，借助切分定理和优先级队列动态排序的特性，从这个起点「生长」出一棵最小生成树。
## 求最短路径   

权重为1都相同的的图可以使用bfs遍历。记录下来depth也就是深度可以
### Dijkstra 算法（一般音译成迪杰斯特拉算法）
> 无非就是一个 BFS 算法的加强版
- 标准的 Dijkstra 算法会把从起点 start 到所有其他节点的最短路径都算出来。

Dijkstra算法采用的是一种贪心的策略，声明一个数组dis来保存源点到各个顶点的最短距离和一个保存已经找到了最短路径的顶点的集合：T，
初始时，原点 s 的路径权重被赋为 0 （dis[s] = 0）。若对于顶点 s 存在能直接到达的边（s,m），则把dis[m]设为w（s, m）,同时把所有其他（s不能直接到达的）顶点的路径长度设为无穷大。初始时，集合T只有顶点s。
然后，从dis数组选择最小值，则该值就是源点s到该值对应的顶点的最短路径，并且把该点加入到T中，OK，此时完成一个顶点，
然后，我们需要看看新加入的顶点是否可以到达其他顶点并且看看通过该顶点到达其他点的路径长度是否比源点直接到达短，如果是，那么就替换这些顶点在dis中的值。
然后，又从dis中找出最小值，重复上述动作，直到T中包含了图的所有顶点

#### 记录从start到其他节点的最短距离

使用优先级队列排序选择一个距离start节点当前距离最短的节点。
```java
//返回s节点的相邻节点
List<Integer> adj(int s);
int[] Dijkstra(int start,,int [][]graph){
    int n=graph.length;
    //start到其他节点的距离
    int []dist=new int[n];
    //初始化，start到start为0其他默认为最大值
    Arrays.fill(dist,Integer.MAX_VALUE);
    dist[start]=0;
    //优先级队列，将较小边放在前边  int [3]== from,to,weights; from 默认为atart
    Queue<int[]> pq = new PriorityQueue<>((a, b) -> {
        return a[2] - b[2];
    });
    //从start开始
    pq.offer(new int[]{start,start,0};)
    while(!pq.empty()){
        int []cur=pq.poll();
        if(cur[2]>dist[cur[1]]){
            //已经有一条更近的路径从 start到cur[1]了
            continue;
        }
        //cur[1]的i相邻节点
        for(int nextnode:adj(cur[1]){
            int distToNextNode=dist[cur[1]]+graoh[cur[1][nextnode]];//start到cur[1]的距离加上cur[1]到nextnode的距离,即为一种start到nextnodede 距离
            if(dist[nextnode]>distToNextNode){//新距离更小，可更新
                dist[nextnode]=distToNextNode;
                pq.offer(new int []{start,nextNode,distToNextNode});
            }   
        })
    }

     return dist;
}

```



#### start到某个特定点的最短距离
```java
List<Integer>adj(int s);//返回一个顶点的    
int Dijkstra(int start,int end,int [][]graph){
     int n=graph.length;
    //start到其他节点的距离
    int []dist=new int[n];
    //初始化，start到start为0其他默认为最大值
    Arrays.fill(dist,Integer.MAX_VALUE);
    dist[start]=0;
    //优先级队列，将较小边放在前边  int [3]== from,to,weights; from 默认为atart
    Queue<int[]> pq = new PriorityQueue<>((a, b) -> {
        return a[2] - b[2];
    });
    //从start开始
    pq.offer(new int[]{start,start,0};)
    while(!pq.empty()){
        int []cur=pq.poll();
        if(cur[1]==end){
            return dist[end];
        }


        if(cur[2]>dist[cur[1]]){
            //已经有一条更近的路径从 start到cur[1]了
            continue;
        }
        //cur[1]的i相邻节点
        for(int nextnode:adj(cur[1]){
            int distToNextNode=dist[cur[1]]+graoh[cur[1][nextnode]];//start到cur[1]的距离加上cur[1]到nextnode的距离,即为一种start到nextnodede 距离
            if(dist[nextnode]>distToNextNode){//新距离更小，可更新
                dist[nextnode]=distToNextNode;
                pq.offer(new int []{start,nextNode,distToNextNode});
            }   
        })
    }

     return Integer.MAX_VALUE;
}

```








### Flod算法

