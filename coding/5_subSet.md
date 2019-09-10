<a id="markdown-子集问题" name="子集问题"></a>
# 子集问题
要明确的问题在于，递归+回溯得到所有子集，就是指数级复杂度，等同于brute force。将子集问题变为迭代问题，并且保证结果无重复子集。

---

<!-- TOC -->

- [子集问题](#子集问题)
  - [包含重复元素的数组得到所有子集](#包含重复元素的数组得到所有子集)
  - [通过转变大小写获得新的字符串求此集合](#通过转变大小写获得新的字符串求此集合)

<!-- /TOC -->

<a id="markdown-包含重复元素的数组得到所有子集" name="包含重复元素的数组得到所有子集"></a>
## 包含重复元素的数组得到所有子集
```cpp
res={{}},last=s[0];
sort(nums.begin(),nums.end());
for(int i=0;i<nums.size();i++){
    //保存不重复起始位置
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

<a id="markdown-通过转变大小写获得新的字符串求此集合" name="通过转变大小写获得新的字符串求此集合"></a>
## 通过转变大小写获得新的字符串求此集合
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