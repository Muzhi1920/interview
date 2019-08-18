# 数据结构与算法
有的一面会问数据结构与算法的基础：线性表，堆，栈，队列，树，图定义，最短路径算法等等

---
```cpp
//字符与数字
val=atoi(s.substr(0, k))
str=to_string(val)
//数据结构
multiset<double> ms(nums.begin(), nums.begin() + k);//维护一个可重复有序的set。



//二分函数
lower_bound( begin,end,num)：//从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
upper_bound( begin,end,num)：//从数组的begin位置到end-1位置二分查找第一个大于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
在从大到小的排序数组中，重载lower_bound()和upper_bound()
lower_bound( begin,end,num,greater<type>() )://从数组的begin位置到end-1位置二分查找第一个小于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
upper_bound( begin,end,num,greater<type>() )://从数组的begin位置到end-1位置二分查找第一个小于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
//建堆
std::priority queue<int,std::vectot<int>,std::greater<int>> small_heap;  //构造最小堆
std::priority queue<int,std::vectot<int>,std::less<int>> big_heap;  //构造最大堆    empty();pop();push();top();size()
priority_queue<int, vector<int>, greater<int> > p;  //默认最大堆。
//插入logK，n个；logk为高度




```


