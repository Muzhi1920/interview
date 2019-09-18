# 子集问题
要明确的问题在于，递归+回溯得到所有子集，就是指数级复杂度，等同于brute force。将子集问题变为迭代问题，并且保证结果无重复子集。

---
<!-- TOC -->

- [子集问题](#子集问题)
  - [对数组取子集](#对数组取子集)
  - [对字符串取子集](#对字符串取子集)
  - [移除K个数字使得值更小](#移除k个数字使得值更小)

<!-- /TOC -->

## 对数组取子集
>>包含重复元素
```cpp
res={{}},last=s[0];
sort(nums.begin(),nums.end());
for(int i=0;i<nums.size();i++){
    //保存不重复起始位置，有重复则是之前的长度
    if(s[i]!=last)
        last=s[i];
        len=res.size();
    //如果有重复则赋值后一半，再添加元素；无重复则从头赋值
    for(int j=res.size()-len;j< res.size();j++) 
        res.push_back(s[j]);
        res.back().push_back(s[i])
}
return res;
```

## 对字符串取子集
>>转变大小写获得新的字符串
```cpp
res={""};
for (char c : S) {
    int len = res.size();
    if (c >= '0' && c <= '9') {
        for (string &str : res) 
            str.push_back(c);
    } else {
        for (int i = 0; i < len; ++i) {
            res.push_back(res[i]);//对当前备份
            res[i].push_back(tolower(c));//对当前加小写
            res.back().push_back(toupper(c));//对备份加大写
        }
    }
}
return res;
```

## 移除K个数字使得值更小
```cpp
for (char c : num) {
    //从前往后，有比当前大的就替换。保证排在前面的更小
    while (k && res.size() && res.back() > c)
        res.pop_back();
        --k;
    if (res.size()==0 && c== '0')
        continue;
    res.push_back(c);
}
while (res.size() && k--) 
    res.pop_back();
return res.empty() ? "0" : res;
```