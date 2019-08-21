# 数据结构与算法
有的一面会问数据结构与算法的基础：线性表，堆，栈，队列，树，图定义，最短路径算法等等

---

## 常用STL函数与数据结构
```cpp
//字符与数字
val=atoi(s.substr(0, k))
str=to_string(val)
//数据结构
multiset<double> ms(nums.begin(), nums.begin() + k);//维护一个可重复有序的set。
memset
//二分函数
lower_bound(begin,end,num)：//从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
upper_bound(begin,end,num)：//从数组的begin位置到end-1位置二分查找第一个大于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
在从大到小的排序数组中，重载lower_bound()和upper_bound()
lower_bound(begin,end,num,greater<type>() )://从数组的begin位置到end-1位置二分查找第一个小于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
upper_bound(begin,end,num,greater<type>() )://从数组的begin位置到end-1位置二分查找第一个小于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
//建堆
std::priority queue<int,std::vectot<int>,std::greater<int>> small_heap;  //构造最小堆
std::priority queue<int,std::vectot<int>,std::less<int>> big_heap;  //构造最大堆    empty();pop();push();top();size()
priority_queue<int, vector<int>, greater<int> > p;  //默认最大堆。
//插入logK，n个；logk为高度

```


## 复杂数据结构构造器

```cpp
//Trie树
struct TrieNode {
    public:
        TrieNode *child[26];
        bool isWord;
        TrieNode():isWord(false) {
            for (auto &a:child) 
                a=NULL;
        }
};
//图，边，节点
typedef struct _ENode{//边：邻接链表
	int ivex;       //该边所指向的顶点的位置
	int source;		//前节点
	int weight;		//权重
	struct _ENode *next_edge;  //指向节点的下一个边
}ENode, *PENode;
typedef struct _VNode{//顶点：链接邻边
	char data;         //顶点信息
	enum Color color;//颜色
	int pre;		//前驱结点
	int category;  //是否已遍历
	int d;	//源节点到当前节点的距离
	int f;  //dfs中 d 开始时间，f 结束时间。
	ENode *first_edge; //邻接链表头结点
}
typedef struct _LGraph{ // 邻接表，图。邻接链表中存储的是当前节点相连的点
	int vexnum;             // 图的顶点的数目
	int edgnum;             // 图的边的数目
	VNode *vexs[SIZE];   //节点数组编号：0~N-1
}LGraph;

```


```cpp
class LRUCache{
public:
    LRUCache(int capacity) {
        cap = capacity;
    }
    int get(int key) {
        auto it = m.find(key);
        if (it == m.end())//没找到
            return -1;
        l.splice(l.begin(), l, it->second);//找到则拼接到l的前端
        return it->second->second;
    }
    //重新放到头部，如果有则删除；如果空间满则移除尾部
    void put(int key, int value) {
        auto it = m.find(key);
        if (it != m.end()) //找到了
            l.erase(it->second);
        l.push_front(make_pair(key, value));
        m[key] = l.begin();
        
        if (m.size() > cap) {
            int k = l.rbegin()->first;
            l.pop_back();
            m.erase(k);
        }
    }
private:
    int cap;
    list<pair<int, int>> l;//可左右端操作
    unordered_map<int, list<pair<int, int>>::iterator> m;
};
```



