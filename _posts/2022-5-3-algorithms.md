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
- 二维数据在内存中不是 3*4 的连续地址空间，而是四条连续的地址空间组成！
  

### 二分查找

前提是已经有序的数组 

最大的最小，最小的最大，二分关键词


#### 普通模式
```java
public int searchInsert(int []nums,int target){}
    int head=0,tail=nums.length-1,mid=tail/2;
    // int index=0;
    if(target>nums[tail])   
        return tail+1;
    if(target<nums[head])
        return 0; 
    while(head<=tail){
        mid=head+(tail-head)/2;//防止溢出，等同于(tail+head)/2
        if(nums[mid]==target)
            return mid;
        if(nums[mid]<target){
            head=mid+1;
        }else{
            tail=mid-1;
        }
    }
    return -1;//未找到
    //返回数组序列的索引
``` 
#### 查找最左边界
有多个相同元素查找最左边界
重点`left<right` ,` right=mid`，`return left`
```java
public int binarySearch(int nums[],int target){
    int left=0,right=nums.length;   
    while(left<right){
        int mid=left+(right-left)/2;
        if(nums[mid]>=target){
            right=mid;
        }else{
            left=mid+1;
        }
    }
    return left;

}

```

#### 查找最右边界
```java
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1; // 注意
}
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

基本格式
增加窗口，根据条件缩小窗口，记录下合适的子数组

```java
int left = 0, right = 0;

while (right < s.size()) {
    // 增大窗口
    window.add(s[right]);
    right++;
    
    while (window needs shrink) {
        // 缩小窗口
        window.remove(s[left]);
        left++;
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

- 主要目的是去除掉已经无用枝杈防止递归下去耗费时间
- 主要在for循环的判断语句中剪


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

在有相同元素且结果集合不能够重复的情况下，可以通过固定相同元素的相对位置去重
```java

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) {
            continue;
        }
        // 新添加的剪枝逻辑，固定相同的元素在排列中的相对位置
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        track.add(nums[i]);
        used[i] = true;
        backtrack(nums);
        track.removeLast();
        used[i] = false;
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

    // boolean []usedtemp=Arrays.copyOfRange(used,0,size);//本层去重
    boolean []usedtemp=new boolean[n];//本层去重
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

利用排序树层去重
```java

public List<List<String>> results=new ArraysList<>();
public List<String> result=new ArraysList<>();
boolean []used;
public void trace(){
    if()
    return ;

    if()
    results.add(new Linked<>(result));

    
    for(int i=start;;i++){
        if(条件){
            if(i>start&&nums[i]!=nums[i-1]){//去除与  已经验证过且比后来相同的树枝更有可能成功的  相同的树枝
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

> 有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。


**先遍历 物品还是先遍历背包重量其实都可以**
对于背包问题，有一种写法， 是使用二维数组，即`dp[i][j] `表示从下标为`[0-i]`的物品里任意取，放进容量为`j`的背包，价值总和最大是多少。
```java
 //遍历顺序：先遍历物品，再遍历背包容量
for (int i = 1; i <= weight.length; i++){
    for (int j = 1; j <= bagsize; j++){
        if (j < weight[i - 1]){
            dp[i][j] = dp[i - 1][j];
        }else{
            dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i - 1]] + value[i - 1]);
        }
    }
}

```
对于一维数组dp，我们知道01背包内嵌的循环是**从大到小**遍历，为了保证每个物品仅被添加一次。
```java
for(int i = 0; i < weight.length; i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = dp[j]>dp[j - weight[i]] + value[i] ?dp[j]:dp[j - weight[i]] + value[i];
    }
}

```

### 完全背包
>有N件物品和一个最多能背重量为W的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品都有无限个（也就是可以放入背包多次），求解将哪些物品装入背包里物品价值总和最大。

> 完全背包和01背包问题唯一不同的地方就是，每种物品有无限件。


```java
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}

```

- 如果求组合数就是外层for循环遍历物品，内层for遍历背包。
- 如果求排列数就是外层for遍历背包，内层for循环遍历物品。



### 多层dp
动态规划，二维数组`dp[][]`
每一层的dp结果有上一层的dp决定，每一个`dp[i][j]`都可由上层的每一个`dp[i-1][k]`决定。
是一种简单暴力方法
### 反方向dp
### 根据之前的状态选择dp的结果打家劫舍
### 根据选择划分多个dp的类，股票

## 状态压缩dp

状态一共有n种
使用n位数字来表示n个种类，n一般不会特比大，一般在16一下
二进制表示状态集合，使用十进制存储

实例
例如一共5种状态，某个时刻状态比较多有3，2，4三种状态
可以写成

```java
4,3,2 
1,1,1,0,0
16+8+4+0+0=28//使用28来表示当前的状态集合

```

### 状态压缩动态规划常用位操作
```java
int c;
int A;
//一般由右向左数
A|=(1<<c);//A的c位变为1，即加入c位的状态
//未知c位有状态
A&=~(1<<c);//A的c位变为0，即去掉c位的状态
//已知c位有状态
A^=1<<c;//A的c位变为0，即去掉c位的状态

a&(-a);//寻找最低位1 返回结果比如2（1），4（2），8（3）


//取最高位
int highbit(){
    int p=lowbit(x);
    while(p!=x){
        x-=p;
        p=lowbit(x);
    }
    return p;
}


A=0;//值为空集合
A|=B;//并集合
A&=B;//交集合
ALL=(1<<(c+1))-1;//全1全集
All^A;//求A的补集
(A&B)==B;//判断B是否位A的子集

//枚举某一个集合的子集
int subset=A;
do{
    subset=(subset-1)&A;
}while(subset!=A);


//数一下某个集合里面的元素一共有多少个
int count=0;
for(int i=0;i<n;i++){
    if(A&(1<<i)!=0)
        count++;
}

int count=0;
for(int i=A;i;i>>=1)
    count+=i&1;



//判断是否为2的幂次
x&(x-1)==0;
```


### 记忆化搜索
> 记录下来搜索过的路径，以避免重复搜索
`Map,Set`之类的数据结构



## 单调栈

>通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了。

1. 可以记录右边或者左边**第一个**比自己大或者小的元素，或者元素位置。是**第一个较而不是最大、小**
2. 通常使用`sta栈`记录栈的状态，使用一个一维数组记录每个位置的结果
3. 可以采用赋值为0的做法，每次遇到单调栈顶元素++可以求解，某方向上单调递增、递减的元素个数
4. 可以同时使用左右单调栈求左右两个较大、较小位置的元素


>以寻找元素右边第一个大的元素为例

### 自右向左 
```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    // 存放答案的数组
    int[] res = new int[n];
    Stack<Integer> s = new Stack<>(); 
    // 倒着往栈里放
    for (int i = n - 1; i >= 0; i--) {
        // 判定个子高矮
        while (!s.isEmpty() && s.peek() <= nums[i]) {
            // 矮个起开，反正也被挡着了。。。
            s.pop();
        }
        // nums[i] 身后的更大元素
        res[i] = s.isEmpty() ? -1 : s.peek();
        s.push(nums[i]);
    }
    return res;
}

```

### 自左向右

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    // 存放答案的数组
    int[] res = new int[n];
    Stack<Integer> s = new Stack<>();
    s.push(0);// 先把第一个元素放在里面
    for (int i = 1; i < n; i++) {
        if (nums[s.peek()] > num[i]) {// 情况一，新元素比栈内元素小,入栈
            s.push(i);
        } else if (nums[s.peek()] == num[i]) {// 情况一，新元素与栈内元素一样,入栈
            s.push(i);
        } else { // 情况一，新元素比栈内元素大,修改栈内元素
            while (!s.isEmpty() && s.peek() <= nums[i]) {
                res[s.peek()] = i;
                s.pop();
            }
        }
        s.push(i);

    }
    return res;
}

```   

## 单调队列

1. 单调队列用于求解，需要使用已经路过的几个位置中的**最值**。
2. 滑动窗口可以记录路过的路径但是不能记录下来最值信息。
3. 单调队列与滑动窗口类似可分析之后取用的是最左端`left`的值还是路过路径（或者说是当前窗口）的最值
4. 常与动态规划连用

一般来讲，单调队列为双端队列，按照降序或者升序纪律窗口内容，按照限制去掉`First`未知的最值，按照单调性对比去除`last`位置的元素，加入到`last`中当前元素


```java

class Solution {
    public int fun(int[] nums) {
        
        
        //单掉队列
        Deque<Integer> dq = new ArrayDeque<>();
        dq.addLast(0);
        for (int i = 1; i <= n; i++) { 
            while (!dq.isEmpty() && nums[dq.peekLast()] >= nums[i]) {
                dq.pollLast();
            }
            while (!dq.isEmpty() && nums[i]-nums[dq.peekFirst()]>=k) {
                // dq.pollFirst(); 
                answer=Math.min(answer,i-dq.pollFirst());
            }
            // if(!dq.isEmpty()&&sums[i]-sums[dq.peekFirst()]>=k){
               
            // }

           
            dq.addLast(i);
        }
        return answer;
    }
}

```


## 前缀和
> 前缀和主要适用的场景是原始数组不会被修改的情况下，频繁查询某个区间的累加和
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






## 差分数组
> 差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减

记录下来每次在什么位置的改变情况，增减。最后结果由累加和的形式给出

```java
public class Difference {
    int[] diff = new int[n];

    public void Difference(int[] nums) {
        for (int i = 0; i < n; i++) {
            diff[i] = nums[i + 1] + nums[i];
        }

    }

    public void increment(int i, int j, int val) {
        diff[i] += val;
        diff[j] -= val;
    }

    public int[] result() {
        int[] answer = new int[n];
        answer[0] = diff[0];
        for (int i = 1; i < n; i++) {
            answer[i] += answer[i - 1] + diff[i];
        }
        return answer;
    }
}


```



## 线段树
可用数组实现，一般大小为原数组的四倍大小。


```java


```

## 树状数组
s树状数组
## 区间技巧
将二维数组按照一定顺序排列
```java
Arrays.sort(nums,(a,b)->{
    return a[0]==b[0]?b[1]-a[1]:a[0]-b[0];
});

```
先0位置由小到大，再1位置由大到小，确保之后计算的时候不会重复计算

## 扫描线算法

1. 针对于求二维面积等问题
2. 将二维的问题转化为两个一维的问题


1. 先按照x排序，利用每个间隔或者每个x进行下一步处理
2. 在y轴用一条线自左向右扫描，乘或者取和。
3. y轴运算可以采用区间技巧 
## 一些算法技巧

### 模幂运算
运算数字太大
```java
b=c+d;
d=e*f;
a**b=(a**c)*(a**d);
a**b=(a**c)*(a**e)**f
```
### 取余
模运算与基本四则运算有些相似，但是除法例外。其规则如下：

`(a + b) % p = (a % p + b % p) % p` 
`(a - b) % p = (a % p - b % p) % p` 
`(a * b) % p = (a % p * b % p) % p` 
`a ^ b % p = ((a % p)^b) % p `

结合律：

`((a+b) % p + c) % p = (a + (b+c) % p) % p `
`((a*b) % p * c)% p = (a * (b*c) % p) % p `

交换律：

`(a + b) % p = (b+a) % p `
`(a * b) % p = (b * a) % p `

分配律：

`((a +b)% p * c) % p = ((a * c) % p + (b * c) % p) % p `


### 随机数技巧

> 概率算法的蒙特卡罗方法

在面临未知的链表或者数组的时候可以采用,
保证每次在面临新的元素的时候概率是一致的

```java
Random ram=new Random();
int index=0;
for(int i=0;i<nums.length;i++){
    if(nums[i]==target){
        index++;    
        res=(0==ram.nextInt(index))?nums[i]:res;

    }

}

```

### 求最大公约数
#### 辗转相除法
时间复杂度不太好计算，可以近似为O(log(max(a, b)))，但是取模运算性能较差
```java
public int gcd(int a,int b){
    return (b==0)?a:gcd(b,a%b);
}
```
#### 更相减损术
避免了取模运算，但是算法性能不稳定，最坏时间复杂度为O(max(a, b)))
```java
public int gcd(int a,int b){
    if(a==b){
        return a;
    }else 
    if(a>b){
        return gcd(a-b,b);
    }
    else if(a<b){
        return gcd(b-a,a);
    }

}
```

#### 更损相减术与移位结合
避免了取模运算，但是算法性能不稳定，最坏时间复杂度为O(max(a, b)))
```java
public int gcd(int a,int b){
    if(a==b){
        return a;
    }
    if(a<b){
        return gcd(b,a);
    }
    if(a<b){
        return gcd(b-a,a);
    }
    boolean aiseven=((a&1)!=1);
    boolean biseven=((b&1)!=1);
    if(aiseven&&biseven){
        return gcd(a>>1,b>>1)<<1;
    }else if(!aiseven&&biseven){
        return gcd(a,b>>1);
    }
    }else if(aiseven&&!biseven){
        return gcd(a>>1,b);
    }else {
        return gcd(b,a-b);
    }
}
```

## 格雷码 

格雷码，相邻的码之间只有一个位不同

000，001，011，010，110，100，101，111

求格雷码
```java
int n;
int m=(1<<n);
List<Integer> res=new ArrayList<>(m);
for(int i=0;i<m;i++){
    res.add(i^(i>>1));
}

```





# TrieMap 字典树，前缀树，单词查找树

> val字段存储键对应的值，children数组存储指向子节点的指针。

### 基本结构
```java
class TrieNode<Template> {
    Template val=null;
    TrieNode<Template>[] children=-new TrieNode[256];
}
```

### 基本方法
```java
class TrieMap<Template> {
  // 在 Map 中添加 key
    public void put(String key, Template val);

    /***** 删 *****/

    // 删除键 key 以及对应的值
    public void remove(String key);

    /***** 查 *****/

    // 搜索 key 对应的值，不存在则返回 null
    // get("the") -> 4
    // get("tha") -> null
    public Template get(String key);

    // 判断 key 是否存在在 Map 中
    // containsKey("tea") -> false
    // containsKey("team") -> true
    public boolean containsKey(String key);

    // 在 Map 的所有键中搜索 query 的最短前缀
    // shortestPrefixOf("themxyz") -> "the"
    public String shortestPrefixOf(String query);

    // 在 Map 的所有键中搜索 query 的最长前缀
    // longestPrefixOf("themxyz") -> "them"
    public String longestPrefixOf(String query);

    // 搜索所有前缀为 prefix 的键
    // keysWithPrefix("th") -> ["that", "the", "them"]
    public List<String> keysWithPrefix(String prefix);

    // 判断是和否存在前缀为 prefix 的键
    // hasKeyWithPrefix("tha") -> true
    // hasKeyWithPrefix("apple") -> false
    public boolean hasKeyWithPrefix(String prefix);

    // 通配符 . 匹配任意字符，搜索所有匹配的键
    // keysWithPattern("t.a.") -> ["team", "that"]
    public List<String> keysWithPattern(String pattern);

    // 通配符 . 匹配任意字符，判断是否存在匹配的键
    // hasKeyWithPattern(".ip") -> true
    // hasKeyWithPattern(".i") -> false
    public boolean hasKeyWithPattern(String pattern);

    // 返回 Map 中键值对的数量
    public int getSize();
}

```



### 方法实现

```java
class TrieMap<Template>{
    private TrieNode<Template> root = null;
    private static final int R = 256;
    // 当前存在 Map 中的键值对个数
    private int size = 0;
    public class TrieMap<Template>(){
        Template val=null;

        TrieNode<Template>[] children = new TrieNode[R];
    }
    public int getSize(){
        return size();
    }
    //工具函数
    public TrieNode<Template> getNode(TrieNode<Template> node, String key){
        TrieNode<Template> p=node;
        for(int i=0;i<key.length();i++){
            if(p==null) return null;
            p=p.children[key.charAt(i)];
        }
        return p;
    }

    // 搜索 key 对应的值，不存在则返回 null
    public Template get(String key) {
        // 从 root 开始搜索 key
        TrieNode<Template> x = getNode(root, key);
        if (x == null || x.val == null) {
            // x 为空或 x 的 val 字段为空都说明 key 没有对应的值
            return null;
        }
        return x.val;
    }

    // 判断 key 是否存在在 Map 中
    public boolean containsKey(String key) {
        return get(key) != null;
    }

    // 判断是和否存在前缀为 prefix 的键
    public boolean hasKeyWithPrefix(String prefix) {
        // 只要能找到 prefix 对应的节点，就是存在前缀
        return getNode(root, prefix) != null;
    }

    // 在所有键中寻找 query 的最短前缀
    public String shortestPrefixOf(String query) {
        TrieNode<Template> p = root;
        // 从节点 node 开始搜索 key
        for (int i = 0; i < query.length(); i++) {
            if (p == null) {
                // 无法向下搜索
                return "";
            }
            if (p.val != null) {
                // 找到一个键是 query 的前缀
                return query.substring(0, i);
            }
            // 向下搜索
            char c = query.charAt(i);
            p = p.children[c];
        }

        if (p != null && p.val != null) {
            // 如果 query 本身就是一个键
            return query;
        }
        return "";
    }

    // 在所有键中寻找 query 的最长前缀
    public String longestPrefixOf(String query) {
        TrieNode<Template> p = root;
        // 记录前缀的最大长度
        int max_len = 0;

        // 从节点 node 开始搜索 key
        for (int i = 0; i < query.length(); i++) {
            if (p == null) {
                // 无法向下搜索
                break;
            }
            if (p.val != null) {
                // 找到一个键是 query 的前缀，更新前缀的最大长度
                max_len = i;
            }
            // 向下搜索
            char c = query.charAt(i);
            p = p.children[c];
        }

        if (p != null && p.val != null) {
            // 如果 query 本身就是一个键
            return query;
        }
        return query.substring(0, max_len);
    }

    // 搜索前缀为 prefix 的所有键
    public List<String> keysWithPrefix(String prefix) {
        List<String> res = new LinkedList<>();
        // 找到匹配 prefix 在 Trie 树中的那个节点
        TrieNode<V> x = getNode(root, prefix);
        if (x == null) {
            return res;
        }
        // DFS 遍历以 x 为根的这棵 Trie 树
        traverse(x, new StringBuilder(prefix), res);
        return res;
    }

    // 遍历以 node 节点为根的 Trie 树，找到所有键
    private void traverse(TrieNode<V> node, StringBuilder path, List<String> res) {
        if (node == null) {
            // 到达 Trie 树底部叶子结点
            return;
        }

        if (node.val != null) {
            // 找到一个 key，添加到结果列表中
            res.add(path.toString());
        }

        // 回溯算法遍历框架
        for (char c = 0; c < R; c++) {
            // 做选择
            path.append(c);
            traverse(node.children[c], path, res);
            // 撤销选择
            path.deleteCharAt(path.length() - 1);
        }
    }



    // 通配符 . 匹配任意字符
    public List<String> keysWithPattern(String pattern) {
        List<String> res = new LinkedList<>();
        traverse(root, new StringBuilder(), pattern, 0, res);
        return res;
    }

    // 遍历函数，尝试在「以 node 为根的 Trie 树中」匹配 pattern[i..]
    private void traverse(TrieNode<V> node, StringBuilder path, String pattern, int i, List<String> res) {
        if (node == null) {
            // 树枝不存在，即匹配失败
            return;
        }
        if (i == pattern.length()) {
            // pattern 匹配完成
            if (node.val != null) {
                // 如果这个节点存储着 val，则找到一个匹配的键
                res.add(path.toString());
            }
            return;
        }
        char c = pattern.charAt(i);
        if (c == '.') {
            // pattern[i] 是通配符，可以变化成任意字符
            // 多叉树（回溯算法）遍历框架
            for (char j = 0; j < R; j++) {
                path.append(j);
                traverse(node.children[j], path, pattern, i + 1, res);
                path.deleteCharAt(path.length() - 1);
            }
        } else {
            // pattern[i] 是普通字符 c
            path.append(c);
            traverse(node.children[c], path, pattern, i + 1, res);
            path.deleteCharAt(path.length() - 1);
        }
    }





    // 判断是和否存在前缀为 prefix 的键
    public boolean hasKeyWithPattern(String pattern) {
        // 从 root 节点开始匹配 pattern[0..]
        return hasKeyWithPattern(root, pattern, 0);
    }

    // 函数定义：从 node 节点开始匹配 pattern[i..]，返回是否成功匹配
    private boolean hasKeyWithPattern(TrieNode<Template> node, String pattern, int i) {
        if (node == null) {
            // 树枝不存在，即匹配失败
            return false;
        }
        if (i == pattern.length()) {
            // 模式串走到头了，看看匹配到的是否是一个键
            return node.val != null;
        }
        char c = pattern.charAt(i);
        // 没有遇到通配符
        if (c != '.') {
            // 从 node.children[c] 节点开始匹配 pattern[i+1..]
            return hasKeyWithPattern(node.children[c], pattern, i + 1);
        }
        // 遇到通配符
        for (int j = 0; j < R; j++) {
            // pattern[i] 可以变化成任意字符，尝试所有可能，只要遇到一个匹配成功就返回
            if (hasKeyWithPattern(node.children[j], pattern, i + 1)) {
                return true;
            }
        } 
        // 都没有匹配
        return false;
    }



    // 在 map 中添加或修改键值对
    public void put(String key, Template val) {
        if (!containsKey(key)) {
            // 新增键值对
            size++;
        }
        // 需要一个额外的辅助函数，并接收其返回值
        root = put(root, key, val, 0);
    }

    // 定义：向以 node 为根的 Trie 树中插入 key[i..]，返回插入完成后的根节点
    private TrieNode<Template> put(TrieNode<Template> node, String key,Template val, int i) {
        if (node == null) {
            // 如果树枝不存在，新建
            node = new TrieNode<>();
        }
        if (i == key.length()) {
            // key 的路径已插入完成，将值 val 存入节点
            node.val = val;
            return node;
        }
        char c = key.charAt(i);
        // 递归插入子节点，并接收返回值
        node.children[c] = put(node.children[c], key, val, i + 1);
        return node;
    }




    // 在 Map 中删除 key
    public void remove(String key) {
        if (!containsKey(key)) {
            return;
        }
        // 递归修改数据结构要接收函数的返回值
        root = remove(root, key, 0);
        size--;
    }

    // 定义：在以 node 为根的 Trie 树中删除 key[i..]，返回删除后的根节点
    private TrieNode<Template> remove(TrieNode<Template> node, String key, int i) {
        if (node == null) {
            return null;
        }
        if (i == key.length()) {
            // 找到了 key 对应的 TrieNode，删除 val
            node.val = null;
        } else {
            char c = key.charAt(i);
            // 递归去子树进行删除
            node.children[c] = remove(node.children[c], key, i + 1);
        }
        // 后序位置，递归路径上的节点可能需要被清理
        if (node.val != null) {
            // 如果该 TireNode 存储着 val，不需要被清理
            return node;
        }
        // 检查该 TrieNode 是否还有后缀
        for (int c = 0; c < R; c++) {
            if (node.children[c] != null) {
                // 只要存在一个子节点（后缀树枝），就不需要被清理
                return node;
            }
        }
        // 既没有存储 val，也没有后缀树枝，则该节点需要被清理
        return null;
    }


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


求 `next`

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


最常见的代码
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


## Pabin-Karp算法

> 字符编码

```java
public int getint(char c) {
        int x = 0;
        switch (c) {
            case 'A':
                x = 0;
                break;
            case 'C':
                x = 1;
                break;
            case 'G':
                x = 2;
                break;
            case 'T':
                x = 3;
                break;
            default:
                break;
        }
        return x;
}

public pabin(String s1,String s2){
    char[] sc1 = s1.toCharArray();
    char[] sc2= s2.toCharArray();
    int target=0;
    int RE=进制;
    int x=0;
    for(int i=0;i<sc1.length;i++){
        target*=RE;
        target+=getint(sc1[i]);
        if(i==sc2.length) x=target;
    }
    int loop=0;
    for(int i=0;i<sc2.length;i++){
        loop*=RE;
        loop+=getint(sc2[i]);
    }
    int right=RE+1;
    int left=1;


    while (right <= sc1.length) {
            int y = getint(sc1[left - 1]);

            // System.out.println(y);
            x -= RE^(sc2.length-1) * y;
            x *= RE;
            x += getdna(sc[right - 1]);
           
            result.add(left);
            
            left++;
            right++;
    }
    return ;
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

### Bfs
> 广度遍历

```java
void Bfs(){
    Queue que;
    // 记录被遍历过的节点
    boolean []visited;
    for(int i=0;i<graph.length;i++){
        if(!visited[i]){
            visited[i]=true;
            EnQueue(i);
            while(!que.empty()){
                int j=Dequeue();
                for(int child:j.children){
                    if(!visitd[child]){
                        visited[child]=true;
                        EnQueue(child);
                    }
                }
            }
        }
    }


}
```

### Dfs
> 深度遍历


```java

//以多个点开始遍历
for(int i=0;graph.length;i++){
    LinkedList<Integer> path=new LinkedList<>();
    trace(graph,i,path);
}
// 记录被遍历过的节点
boolean[] visited;//若是没有环可以省略，相当于多叉树
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

##  拓扑排序
重点不是`（a,b）`并不是绝对的`a->b`或者`b->a`是指b到a有条路径或者说，b在a之前
注意路径上添加的节点顺序，是在遍历外后续节点之后添加。

```java
class Solution {
    int numCourses;
    List<Integer> graph[],path,indegree;
    boolean hascycle,visited[], onpath[];

    public void build(int [][]prerequisites) {
        graph = new ArrayList[numCourses];
        indegree = new ArrayList<>();
        for (int i=0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }
        int count[]=new int[numCourses];
        for (int []prere : prerequisites) {
            graph[prere[0]].add(prere[1]);
            count[prere[1]]++;
        }
        for(int i=0;i<numCourses;i++){
            if(count[i]==0){
                indegree.add(i);
            }
        }
    }
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        this.numCourses = numCourses;
        visited = new boolean[numCourses];
        onpath = new boolean[numCourses];
        path = new ArrayList<>();

        build(prerequisites);

        for (int i : indegree) {
            trace(i);
        }
        if (hascycle || path.size() < numCourses) return new int[] {};
        int answer[] = new int[numCourses];
         System.out.println(indegree);
        System.out.println(Arrays.toString(graph));

        for (int i = 0; i < numCourses; i++) {
            // answer[i] = path.get(numCourses-1-i);
            answer[i] = path.get(i);
        }
        return answer;

    }
    public void trace(int course){
        if(onpath[course]){
            hascycle=true;
            return ;
        }
        if(hascycle||visited[course]) return ;
        visited[course]=true;
        onpath[course]=true;
      
        for(int precourse:graph[course]){
            trace(precourse);
        }  path.add(course);

        onpath[course]=false;
    }
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

```java
class Prim {
    // 核心数据结构，存储「横切边」的优先级队列
    private PriorityQueue<int[]> pq;
    // 类似 visited 数组的作用，记录哪些节点已经成为最小生成树的一部分
    private boolean[] inMST;
    // 记录最小生成树的权重和
    private int weightSum = 0;
    // graph 是用邻接表表示的一幅图，
    // graph[s] 记录节点 s 所有相邻的边，
    // 三元组 int[]{from, to, weight} 表示一条边
    private List<int[]>[] graph;

    public Prim(List<int[]>[] graph) {
        this.graph = graph;
        this.pq = new PriorityQueue<>((a, b) -> {
            // 按照边的权重从小到大排序
            return a[2] - b[2];
        });
        // 图中有 n 个节点
        int n = graph.length;
        this.inMST = new boolean[n];

        // 随便从一个点开始切分都可以，我们不妨从节点 0 开始
        inMST[0] = true;
        cut(0);
        // 不断进行切分，向最小生成树中添加边
        while (!pq.isEmpty()) {
            int[] edge = pq.poll();
            int to = edge[1];
            int weight = edge[2];
            if (inMST[to]) {
                // 节点 to 已经在最小生成树中，跳过
                // 否则这条边会产生环
                continue;
            }
            // 将边 edge 加入最小生成树
            weightSum += weight;
            inMST[to] = true;
            // 节点 to 加入后，进行新一轮切分，会产生更多横切边
            cut(to);
        }
    }

    // 将 s 的横切边加入优先队列
    private void cut(int s) {
        // 遍历 s 的邻边
        for (int[] edge : graph[s]) {
            int to = edge[1];
            if (inMST[to]) {
                // 相邻接点 to 已经在最小生成树中，跳过
                // 否则这条边会产生环
                continue;
            }
            // 加入横切边队列
            pq.offer(edge);
        }
    }

    // 最小生成树的权重和
    public int weightSum() {
        return weightSum;
    }

    // 判断最小生成树是否包含图中的所有节点
    public boolean allConnected() {
        for (int i = 0; i < inMST.length; i++) {
            if (!inMST[i]) {
                return false;
            }
        }
        return true;
    }
}

```


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


# 最大堆
```java
class MaxPQ {
    int pq[];
    int size;

    MaxPQ(int capacity) {
        pq = new int[capacity + 1];
        size = 0;
    }

    public int MAX() {
        return pq[1];
    }

    public void insert(int x) {
        size++;
        pq[size] = x;
        swim(size);
    }

    public int delMax() {
        int m = pq[1];
        swap(1, size);
        size--;
        sink(1);
        return m;
    }

    // 上浮
    private void swim(int x) {
        while (x > 1 && less(parent(x), x)) {
            swap(x, parent(x));
            x = parent(x);
        }
    }

    // 下潜
    private void sink(int x) {
        while (x < size && left(x) <= size) {
            int max = left(x);
            if (right(x) <= size && less(left(x), right(x))) {
                max = right(x);
            }
            if (less(max, x))
                break;
            swap(x, max);
            x = max;

        }

    }

    private int left(int x) {
        return x * 2 > size ? -1 : x * 2;
    }

    private int right(int x) {
        return x * 2 + 1 > size ? -1 : x * 2 + 1;
    }

    private int parent(int x) {
        return x / 2;
    }

    private void swap(int x, int y) {
        int temp = pq[x];
        pq[x] = pq[y];
        pq[y] = temp;
    }

    private boolean less(int x, int y) {
        return pq[x] < pq[y];
    }

}

```

# 操作系统算法

## LRU,最近没被使用过
### 自写
```java

import java.util.HashMap;
import java.util.Map;

/*
 * @lc app=leetcode.cn id=146 lang=java
 *
 * [146] LRU 缓存
 */

// @lc code=start
class LRUCache {
    class Node {
        int val, key;
        Node pre, next;

        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    class DoubleList {
        int size;
        Node head, tail;

        DoubleList() {
            this.size = 0;
            head = new Node(0, 0);
            tail = new Node(0, 0);
            head.next = tail;
            tail.pre = head;
        }

        Node removeNode(Node node) {
            Node prenode = node.pre, nextnode = node.next;
            prenode.next = nextnode;
            nextnode.pre = prenode;
            size--;
            return node;
        }

        Node removeFirst() {
            if (size != 0) {
                return removeNode(head.next);
                // size--;
            }
            return null;
        }

        void addLast(Node node) {
            node.next = tail;
            node.pre = tail.pre;
            tail.pre.next = node;
            tail.pre = node;
            size++;
        }

    }

    int capacity;
    Map<Integer, Node> map;
    DoubleList cache;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        cache = new DoubleList();
    }

    public int get(int key) {
        // System.out.println(map);

        if (!map.containsKey(key))
            return -1;
        Node node = map.get(key);
        cache.removeNode(node);
        cache.addLast(node);
        return node.val;
    }

    public void put(int key, int value) {
        // System.out.println(map);
        if (map.containsKey(key)) {
            Node node = new Node(key,value);
            cache.removeNode(map.get(key));
            cache.addLast(node);
            map.put(key,node);
            return;
        }
        if (cache.size == capacity) {
            Node delnode = cache.removeFirst();
            map.remove(delnode.key);
        }
        Node node = new Node(key, value);
        cache.addLast(node);
        map.put(key, node);
    }
    public void makeRecently(int key){
        //确定一定有
        Node node=map.get(key);
        cache.removeNode(node);
        cache.addLast(node);

    }
    public void addRecently(int key,int val){
        if(cache.size==capacity){
            Node node=cache.removeFirst();
            map.remove(node.key);
        }
        Node newnode=new Node(key,val);
        cache.addLast(newnode);
        map.put(key,newnode);
    }
    public int  deleteKey(int key) {
        Node x = map.get(key);
        cache.removeNode(x);
        map.remove(key);
        return x.val;
    }
    public int reremoveLastRecently() {
        Node deletenode = cache.removeFirst();
        map.remove(deletenode.key);
        return deletenode.val;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
// @lc code=end

```



### java内置LinkedHashMap
```java
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;

class LRUCache {

    int capacity;
    LinkedHashMap<Integer, Integer> cache;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new LinkedHashMap<>();
    }

    public int get(int key) {
        if (!cache.containsKey(key))
            return -1;
        makeRecently(key);
        return cache.get(key);
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            cache.put(key, value);
            makeRecently(key);
            return;
        }
        if (cache.size() >= capacity) {
            removeLastRecently();
        }
        cache.put(key, value);

    }

    public void makeRecently(int key) {
        int val = cache.get(key);
        cache.remove(key);
        cache.put(key, val);

    }

    public void addRecently(int key, int val) {
        if (cache.size() >= capacity) {
            removeLastRecently();
        }
        cache.put(key, val);
    }

    public void deleteKey(int key) {
        cache.remove(key);
    }

    public int removeLastRecently() {
        int deleteKey = cache.keySet().iterator().next();
        int val = cache.get(deleteKey);
        cache.remove(deleteKey);
        return val;
    }
}

```

## LFU算法 使用次数最少淘汰

```java
/*
 * @lc app=leetcode.cn id=460 lang=java
 *
 * [460] LFU 缓存
 */

// @lc code=start
class LFUCache {
    int capacity, minFreq;
    //键到值的映射
    HashMap<Integer, Integer> keyToVal;
    //键到频率的映射
    HashMap<Integer, Integer> keyToFreq;
    //每个频率之下的
    HashMap<Integer, LinkedHashSet<Integer>> freqToKeys;

    public LFUCache(int capacity) {
        this.capacity = capacity;
        keyToFreq = new HashMap<>();
        keyToVal = new HashMap<>();
        freqToKeys = new HashMap<>();
        minFreq = 1;
    }

    public int get(int key) {
        if (!keyToVal.containsKey(key))
            return -1;

        increaseKey(key);

        return keyToVal.get(key);
    }

    public void put(int key, int val) {
        if (capacity <= 0)
            return;

        if (keyToVal.containsKey(key)) {
            keyToVal.put(key, val);
            increaseKey(key);
            return ;
          
        }

        if (capacity <= keyToVal.size()) {
            removeMinFreq();
        }
        keyToVal.put(key, val);
        if (!freqToKeys.containsKey(1))
            freqToKeys.put(1, new LinkedHashSet<>());
        freqToKeys.get(1).add(key);
        keyToFreq.put(key, 1);

        minFreq = 1;
    }

    public void increaseKey(int key) {
        int freq = keyToFreq.get(key);
        keyToFreq.put(key, freq + 1);
        if (!freqToKeys.containsKey(freq + 1)) {
            freqToKeys.put(freq + 1, new LinkedHashSet<>());
        }
        freqToKeys.get(freq + 1).add(key);
        freqToKeys.get(freq).remove(key);

        if (freqToKeys.get(freq).isEmpty()) {
            freqToKeys.remove(freq);
        }
        if (freq == minFreq && freqToKeys.get(freq) == null) {
            
            minFreq++;
        }
     
    }

    public void removeMinFreq() {
        LinkedHashSet<Integer> keyList = freqToKeys.get(minFreq);
        int deleteKey = keyList.iterator().next();
        keyList.remove(deleteKey);
        if (keyList.isEmpty()) {
            freqToKeys.remove(minFreq);
        }
        keyToVal.remove(deleteKey);
        keyToFreq.remove(deleteKey);
    }
}

```












