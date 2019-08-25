# 循环排序
处理给定范围内数字的数组问题；一般要求O(N)解决。思路就是对应数字放到对应位置。

---

- 返回数组内缺失的第一个正整数
对应下标：0~N-1；
正整数位：1~N；

```cpp
遍历nums{
    while(1<=nums[i]<=N)and(nums[i]!=nums[nums[i]-1])：//nums[i]不在i-1位置上
        swap(num[i],nums[i-1]);
}
遍历nums{
    if(nums[i]!=i+1):
        return i+1;
}
return N+1; //都满足条件，返回N+1
```

- 寻找重复数
nums大小：0~N 数字范围：[1,N]
在下标范围内，对数值索引会得到一个环，环的起点就是重复数字；同有环链表
```cpp
while(1)
    slow=num[slow];
    fast=num[num[fast]];
    if slow ==falst //在环中相遇
        break;
slow=head;
while(1)
    slow=num[slow];
    fast=num[fast];
    if slow ==fast  //环中继续走，头开始走。走相同距离相遇即为起点
        break;
return slow;
```











