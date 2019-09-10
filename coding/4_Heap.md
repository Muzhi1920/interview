<a id="markdown-堆" name="堆"></a>
# 堆
维护一个堆一般是为了使用堆排序，减去反复排序步骤。
```cpp
priority_queue<int,vector<int>> big;  //默认最大堆。
priority_queue<int,vectot<int>,greater<int>> small;  //构造最小堆
```
---

<!-- TOC -->

- [堆](#堆)
  - [建堆](#建堆)
  - [合并K个有序列表O(N*logk)](#合并k个有序列表onlogk)
  - [数据流中位数](#数据流中位数)

<!-- /TOC -->



<a id="markdown-建堆" name="建堆"></a>
## 建堆
![avatar](img/640_50.gif)
插入时间复杂度O(logN)，排序时间复杂度O(nlogn)
典型的问题是topK，免去反复排序的步骤，一直入堆自动完成排序。弹出头部。

<a id="markdown-合并k个有序列表onlogk" name="合并k个有序列表onlogk"></a>
## 合并K个有序列表O(N*logk)
>>对各链表头部维护一个最小堆
```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    auto cmp = [](ListNode*& a, ListNode*& b) {
        return a->val > b->val;
    };
    priority_queue<ListNode*, vector<ListNode*>, decltype(cmp) > q(cmp);
    
    for (auto node : lists) {
        if (node) q.push(node);
    }
    ListNode *dummy = new ListNode(-1), *cur = dummy;
    while (!q.empty()) {
        auto t = q.top();q.pop();
        cur->next = t;
        cur = cur->next;
        if (cur->next) 
            q.push(cur->next);
    }
    return dummy->next;
}
```

<a id="markdown-数据流中位数" name="数据流中位数"></a>
## 数据流中位数
数据流或者数组获取中位数不知道有什么区别，感觉可以通用
```cpp
priority_queue<int,vector<int>,greater<int>>left,right;
void addNum(int num){
    right.push(num)
    left.push(-right.top());right.pop();//负右入左
    if right.size < left.size
        right.push(-left.top());left.pop();//负左入右
}
void getMedian(){
    if(right.size()>left.size())
        return right.top();
    else
        return (right.top-left.top)/2;
}
//最终保证，两个最小堆。左堆绝对值递增，但实数值是减小的（符合最小堆的定义）；右堆按照最小堆递减，实数值也是递减。
```
