# 滑动窗口
分固定窗口和不固定窗口。
窗口问题一般是对窗口执行所需操作。一般要求O(N)：i持续往后遍历期间会通过字符映射判断字符是否足够；还会对左指针加一些判断然后右移。

## 固定窗口问题
- 滑动窗口最大值：一般窗口固定的话，就是一次移一个得到某个结果值
Q1：何时将结果push到res?i-left>=-1时此刻push第一个。
Q2：如何判断窗口后移之前最大值pop?保留的是下标，下标top()=i-k时pop掉。
Q3：期间有大的还要替换前面比它小的
```cpp
//双向队列保存当前最大值下标
deque<int>q;
遍历nums{
    if q and q.front==i-k
        pop_front();
    while(q and nums[i]>q.back())
        pop();
    q.push(i);
    if(i-k>=-1)      //说明窗口开始移动，res该存储结果
        res.push(q.front)
}
```
- 滑动窗口中位数：
直观的解法就是每个窗口排序取中位数。借用multiset，可重复的有序集合，mid指针始终指向中间位置；而且中间位置的元素也有技巧去唯一获取。
```cpp
multiset<double> ms(nums.begin(), nums.begin() + k);//维护一个可重复有序的set。/取k个元素构造multiset
auto mid = next(ms.begin(), k / 2);
遍历nums：
    res.push(mid+*prev(mid,1-k%2))/2  //K为奇偶?保存当前中位数
    if i==nums.size()
        return res;
    ms.insert(nums[i]);
     //判断新进值和刚出值移动mid指针
    if *mid > nums[i]
        mid--;
    if *mid >= nums[i-k]
        mid++;
    ms.erase(ms.lower_bound(nums[i-k]));
```

- 两个索引满足下标差和数值差
```cpp
map<long long,int >m; int left; //默认有序map，保存K个数字
遍历nums{
    if(i-left>k)
        m.erase[nums[left]];
        left++;
    //对map的key二分查找(nums[i]-t)
    auto a=m.lower_bound((long long)nums[i]-t);
    if(a and a->first-nums[i]<=t)
        return true;
    m[nums[i]]=i;
}
return false;
```

## 不固定窗口问题

- 含有不重复字符的最长的子串
Q1：当有重复的时候左更新；不重复的时候窗口长度累加。所以O(N)的遍历即可
```cpp
char c[126]=-1,left=-1;
遍历nums{
    left=max(c[nums[i],left]); //不是-1则重复,重新计算最大长度
    c[nums[i]]=i;
    res=max(res,i-left);
    }
return res;
```

- 最小包含（覆盖）子串

```cpp
s,t
char c[126]=0;//对t遍历统计c各字符次数
遍历nums{
    c[nums[i]]--;
    if c[nums[i]]>=0：//说明属于子串t的字符
        cnt++;
    while cnt==t.size{ //子串可构成t；然后左侧右移取最小窗口
        minLen=min(minLen,i-left+1);
        res=res.substr(left,minLen);
        c[nums[left]]++; //从左往右不相关加上
        if(c[nums[left]]>0) //如果加的是相关字符则跳出
            cnt--;
        left++;
    }
}
return res;
```
- 替换K次字符后最长的重复字符构成的子串长度
```cpp
int count,res,left=0;
遍历c:nums{
    map[c]++;
    count=max(count,map[c]);
    while(i-left+1 > (count+k)) //达到了最大长度，左边界右移
        map[left]--;
        left++;
    res=max(res,i-left+1);
}
```
- 字符串包含另一子串的排列
```cpp
遍历t:
    m[t[i]]++;//保存t中字符次数
遍历s:
    m[s[i]]--;
    if(m[s[i]]<0) //说明此时没匹配对
        while(++m[left]!=0)//则对之前匹配对的再补上+1
            left++;
    else if (i-left+1==t.size())//窗口大小==t长度相同则匹配到了最后一个字符
        return true;
return t.szie()==0;
```
- 长度最小的子数组。和相同（双指针）
>>一直累加，当超过目标值时左边移动
```cpp
遍历nums{
    sum+=nums[i];
    while left<=i and sum>=Target{
        res=min(res,i-left+1);
        sum-=num[left];
        left++;
    }
}
return res==INT_MAX?0:res;
```

- 重复的DNA序列
```python
def findRepeatedDnaSequences(self, s: str) -> List[str]:
    from collections import defaultdict
    visited = set()
    res = set()
    for i in range(0, len(s) - 9):
        tmp = s[i:i+10]
        if tmp in visited:
            res.add(tmp)
        visited.add(tmp)
    return list(res)
```