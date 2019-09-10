<a id="markdown-滑动窗口" name="滑动窗口"></a>
# 滑动窗口
分固定窗口和不固定窗口。
窗口问题一般是对窗口执行所需操作。一般要求O(N)：i持续往后遍历期间会通过字符映射判断字符是否足够；还会对左指针加一些判断然后右移。

<!-- TOC -->

- [滑动窗口](#滑动窗口)
  - [固定窗口问题](#固定窗口问题)
    - [滑动窗口最大值](#滑动窗口最大值)
    - [滑动窗口中位数：](#滑动窗口中位数)
    - [两个索引满足下标差和数值差](#两个索引满足下标差和数值差)
  - [不固定窗口问题](#不固定窗口问题)
    - [含有不重复字符的最长的子串](#含有不重复字符的最长的子串)
    - [最小包含T的排列的子串*](#最小包含t的排列的子串)
    - [字符串包含另一子串的排列*](#字符串包含另一子串的排列)
    - [替换K次字符后最长的重复字符构成的子串长度](#替换k次字符后最长的重复字符构成的子串长度)
    - [长度最小的子数组。和相同（双指针）](#长度最小的子数组和相同双指针)
    - [重复的DNA序列](#重复的dna序列)

<!-- /TOC -->

<a id="markdown-固定窗口问题" name="固定窗口问题"></a>
## 固定窗口问题
<a id="markdown-滑动窗口最大值" name="滑动窗口最大值"></a>
### 滑动窗口最大值
>>一般窗口固定的话，就是一次移一个得到某个结果值
- Q1：何时将结果push到res?i-left>=-1时此刻开始push第一个。
- Q2：如何判断窗口后移之前最大值pop?保留的是下标，下标top()=i-k时pop掉。
- Q3：期间有大的还要替换前面比它小的
```cpp
//双向队列保存当前最大值下标
deque<int>q;
for(int i=0;i < nums.size();i++){
    if(!q.empty() && q.front()<=i-k)//较大值窗口之外
        q.pop_front();
    while(!q.empty() && nums[i]>q.back())
        q.pop_back();
    q.push(i);
    if(i-k>=-1)
        res.push(q.front())
}
```
<a id="markdown-滑动窗口中位数" name="滑动窗口中位数"></a>
### 滑动窗口中位数：
>>直观的解法就是每个窗口排序取中位数。借用multiset，可重复的有序集合，mid指针始终指向中间位置；而且中间位置的元素也有技巧去唯一获取。
```cpp
multiset<int> ms(nums.begin(), nums.begin() + k);//初始化
auto mid = next(ms.begin(), k / 2);//取当前中间或偏右位置
res.push((*mid+*prev(mid,1-k%2))/2);//取当前中位数
for (int i = k;i<nums.size(); ++i) {
    ms.insert(nums[i]);
    //新插入元素在左边
    if (nums[i] < *mid)
        --mid;
    //新删除元素在左边
    if (nums[i - k] <= *mid)
        ++mid;
    //二分查找删除元素
    ms.erase(ms.lower_bound(nums[i - k]));
    res.push_back((*mid + *prev(mid,  1 - k % 2)) / 2);
}
```

<a id="markdown-两个索引满足下标差和数值差" name="两个索引满足下标差和数值差"></a>
### 两个索引满足下标差和数值差
```cpp
map<int int> m;int j = 0;
for (int i = 0; i < nums.size(); ++i) {
    if (i - j > k)
        m.erase(nums[j]);
        j++;
    //寻找第一个大于等于(nums[i]-t)的数,小于的nums[i]的差的绝对值会大于t 
    auto a = m.lower_bound((long long)nums[i] - t);
    if (a != m.end() && abs(a->first - nums[i]) <= t)
        return true;
    m[nums[i]] = i;
}
return false;
```

<a id="markdown-不固定窗口问题" name="不固定窗口问题"></a>
## 不固定窗口问题

<a id="markdown-含有不重复字符的最长的子串" name="含有不重复字符的最长的子串"></a>
### 含有不重复字符的最长的子串
Q1：当有重复的时候左更新；不重复的时候窗口长度累加。所以O(N)的遍历即可
```cpp
char c[126]=-1,left=-1;
for(int i=0;i<s.length();i++){
    left=max(c[nums[i],left]); //不是-1则重复
    res=max(res,i-left);
    c[nums[i]]=i;
}
return res;
```

<a id="markdown-最小包含t的排列的子串" name="最小包含t的排列的子串"></a>
### 最小包含T的排列的子串*
>>包含其他字符
```cpp
s,t
char c[126]=0;//对t遍历统计c各字符次数
for(int i=0;i<s.length();i++){
    c[s[i]]--;
    if c[s[i]]>=0：//说明属于子串t的字符
        cnt++;
    while(cnt==t.size()){ //子串可构成t；然后左侧右移取最小窗口
        minLen=min(minLen,i-left+1);
        res=res.substr(left,minLen);
        c[s[left]]++; //从左往右不相关加上
        if(c[s[left]]>0) //如果加的是相关字符则跳出
            cnt--;
        left++;
    }
}
return res;
```

<a id="markdown-字符串包含另一子串的排列" name="字符串包含另一子串的排列"></a>
### 字符串包含另一子串的排列*
>>不包含其他字符
```cpp
for(auto c :t)
    m[c]++;//保存t中字符次数
for(int i=0;i< s.length();i++){
    m[s[i]]--;
    if(m[s[i]]<0){ //说明此时没匹配对
        while(++m[left]!=0) //则对之前匹配对的再补上+1
            left++;
    }
    else{ 
        if (i-left+1==t.size())//窗口大小==t长度相同
            return true;
    }
}
return t.szie()==0;
```

<a id="markdown-替换k次字符后最长的重复字符构成的子串长度" name="替换k次字符后最长的重复字符构成的子串长度"></a>
### 替换K次字符后最长的重复字符构成的子串长度
```cpp
int count,res,left=0;
map<char,int>m;
for(int i=0;i< s.length();i++){
    m[s[i]]++;
    count=max(count,m[s[i]]);
    while(i-left+1 > (count+k)) //达到了最大长度，左边界右移
        m[s[left]--;
        left++;
    res=max(res,i-left+1);
}
```

<a id="markdown-长度最小的子数组和相同双指针" name="长度最小的子数组和相同双指针"></a>
### 长度最小的子数组。和相同（双指针）
>>一直累加，当超过目标值时左边移动
```cpp
int sum,res,left;
for(int i=0;i<nums.size();i++){
    sum+=nums[i];
    while(left<=i && sum>=Target){
        res=min(res,i-left+1);
        sum-=num[left];
        left++;
    }
}
return res==INT_MAX?0:res;
```

<a id="markdown-重复的dna序列" name="重复的dna序列"></a>
### 重复的DNA序列
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