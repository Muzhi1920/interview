# 循环排序
处理给定范围内数字的数组问题；一般要求O(N)解决。思路就是对应数字放到对应位置。

---

<!-- TOC -->

- [循环排序](#循环排序)
    - [寻找重复数](#寻找重复数)
        - [环的快慢指针实现寻找重复数](#环的快慢指针实现寻找重复数)
        - [循环排序实现寻找重复数](#循环排序实现寻找重复数)
        - [最小的k个数](#最小的k个数)
    - [众数](#众数)

<!-- /TOC -->

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

## 寻找重复数
### 环的快慢指针实现寻找重复数
>>在下标范围内，对数值索引会得到一个环，环的起点就是重复数字；同有环链表。查找N+1个数字，范围在[1,N]
```cpp
while(1){
    slow=num[slow];
    fast=num[num[fast]];
    if(slow ==falst) //在环中相遇
        break;
}
slow=head;
while(1){
    slow=num[slow];
    fast=num[fast];
    if(slow ==fast)  //环中继续走，头开始走。走相同距离相遇即为起点
        break;
}
return slow;
```

### 循环排序实现寻找重复数
>>数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。N个数字都在[0,N-1]]的范围内。
```cpp
for(int i = 0; i < nums.size(); ++i){
    while(nums[i] != i){
        if(nums[i] == nums[nums[i]])
            *duplication = nums[i];
            return true;
        swap(nums[i],nums[nums[i]]);
    }
}
```

### 最小的k个数
>>4,5,1,6,2,7,3,8  O(k*n)=O(n)
```cpp
int partition(vector<int> nums, int start, int end){
    int key = nums[start];
    while (start < end) {
        //从右找小于key的值，赋给start
        while (start < end and nums[end] >= key)
            end --;
        nums[start] = nums[end];
        //从左找大于key的值，赋给end
        while (start < end and nums[start] <= key)
            start ++;
        nums[end] = nums[start];

        //key赋给start
        nums[start] = key;
    }
    return start;
}

vector<int> GetLeastNumbers(vector<int> input, int k){
    vector<int>output;
    int start = 0;
    int end = input.size() - 1;
    int index = partition(input,start,end);
    while(index != k - 1){
        if(index > k - 1){
            end = index - 1;
            index = partition(input,start,end);
        }
        else{
            start = index + 1;
            index = partition(input,start,end);
        }
    }
    for(int i = 0; i < k; ++i)
        output.push_back(input[i]);
}
```

## 众数
>>找到超过某个数量的数字
```cpp
int base=nums[0];
for(int i=1;i<nums.size();i++){
    if(nums[i]==base);
        times++;
    else：
        times--;
    if(times==0)
        base=nums[i];
        times=1;
}
for(int i=0;i<nums.size();i++){
    if(nums[i]==base)
        count++;
}
return count==num;
```







