# 子集问题
要明确的问题在于，递归+回溯得到所有子集，就是指数级复杂度，等同于brute force。将子集问题变为迭代问题，并且保证结果无重复子集。

---
- 包含重复元素的数组得到所有子集
```cpp
res={{}},对nums排序,last=s[0]
遍历nums：
    if s[i]!=last
        last=s[i];
        len=res.size();
    for(int j=res.size()-len;j< res.size();j++) //如果有重复则赋值后一半，再添加元素；无重复则从头赋值
        res.push_back(s[j]);
        res.back().push_back(s[i])   //对新复制的子集添加新元素
return res;
```

- 通过转变大小写获得新的字符串求此集合
```cpp
res={""};
for(char c:string){
    int len=res.size;
    if(s[i]-'a'>=0 && <=9)：
        for(c:subset):
            c.push_back(s[i]);
    else: //若是字母
        for(i in range(len)){ //下标遍历当前res
            res.push(res[i]);
            res[i].push(tolower(c));
            res.back().push(toupper(c));
        }
}
return res;
```