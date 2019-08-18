# 堆
维护一个堆一般是为了使用堆排序，减去反复排序步骤。
```cpp
priority_queue<int,vector<int>> big;  //默认最大堆。
priority_queue<int,vectot<int>,greater<int>> small;  //构造最小堆

```
---
## 建堆
![avatar](img/640_50.gif)
插入时间复杂度O(logN)，排序时间复杂度O(nlogn)
典型的问题是topK，免去反复排序的步骤，一直入堆自动完成排序。弹出头部。

- 合并K个有序列表
对各链表头部维护一个最小堆
```cpp
auto cmp=[](ListNode*& a,ListNode*& b){
    return a->val>b->val;
    };
priority_queue<ListNode*,vector<ListNode*>,decltype(cmp)>q(cmp);
列表的头指针构造最小堆；
while(q):
    tmp=q.top,pop;
    cur-next=tmp;cur=cur->next;
    if(tmp->next):
        q.push(tmp->next);
```
- 数据流中位数
数据流或者数组获取中位数不知道有什么区别，感觉可以通用
```cpp
构造左右两个最小堆；
void addNum(int num){
    right_q.push(num)
    left_q.push(右堆的负top值);right_q.pop;
    if right_q.size < left_q.size：
        right_q.push(左堆的负top值);left_q.pop;
}
void getMedian(){
    右堆大则返回右堆top，否则(右堆top-左堆top)/2
}
//最终保证，两个最小堆。左堆绝对值递增，但实数值是减小的（符合最小堆的定义）；右堆按照最小堆递减，实数值也是递减。
```
- 第N个丑数：给定几个数定义为丑数，所有的数都是基于丑数的累乘。

brute force：第n个丑数；每个丑数累乘到之前，取大于dp[n-1]的最小的那个值。
如果记住了丑数累乘到之前的第几个数，那么可以从当前累乘去比较，因为之前的一定太小。
```cpp
遍历1~i~N：
    //构造第i个丑数
    初始化dp[i]为INT_MAX
    遍历丑数：
        dp[i] = min(dp[i], 丑数[j] * dp[idx[j]]);
    遍历丑数：
        if dp[i] == dp[idx[j]] * primes[j]：
            idx[j]++; //下次累乘时，从比当前大一个的丑数开始
```