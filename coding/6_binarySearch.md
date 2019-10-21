<a id="markdown-二分查找" name="二分查找"></a>
# 二分查找
使用二分的前提是给定链表或数组是有序的；在此基础上的变形比如排序树组旋转；两个有序数组的中位数

---
<!-- TOC -->

- [二分查找](#二分查找)
        - [二分查找实现寻找重复数](#二分查找实现寻找重复数)
    - [有序数组某数字的出现次数](#有序数组某数字的出现次数)
    - [旋转数组的搜索target](#旋转数组的搜索target)
    - [旋转数组找min](#旋转数组找min)
    - [寻找两个有序数组中位数](#寻找两个有序数组中位数)
    - [逆序数](#逆序数)
    - [山脉三题](#山脉三题)

<!-- /TOC -->


<a id="markdown-二分查找实现寻找重复数" name="二分查找实现寻找重复数"></a>
### 二分查找实现寻找重复数
循环排序改变了原数组，O(N)，二分查找O(N*logN)
>>start,middle,end范围在：0、1、2、3、4、5、6.对区间二分

```cpp
int countRange(int *numbers, int length, int start, int end) {
    int count = 0;
    for (int i = 0; i < length; i++)
        if (numbers[i] >= start && numbers[i] <= end)
            ++count;
    return count;
}
int getDuplication(const int *numbers, int length) {
    int start = 0;
    int end = length - 1;
    while (end >= start) {
        int middle = (end + start)/2;
        int count = countRange(numbers, length, start, middle);
        if (end == start) {
            if (count > 1)
                return start;
            else
                break;
        }
        //该范围有重数
        if (count > (middle - start + 1))
            end = middle;
        else
            start = middle + 1;
    }
    return -1;
}
```

<a id="markdown-有序数组某数字的出现次数" name="有序数组某数字的出现次数"></a>
## 有序数组某数字的出现次数
>>两次二分查找下标相减

```cpp
int biSearch(vector<int> nums, float num) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = (right + left) / 2;
        if (nums[mid] < num)
            left = mid + 1;
        else if (nums[mid] > num)
            right = mid - 1;
    }
    return left;
}
int find(vector<int> nums, int target) {
    return biSearch(nums, target + 0.5f) - biSearch(nums, target - 0.5f);
}
```

<a id="markdown-旋转数组的搜索target" name="旋转数组的搜索target"></a>
## 旋转数组的搜索target
>>含重复元素
```cpp
while(left <= right){
    if(num[mid]==target)
        return true;
    else if (num[mid] > num[right]){
        if(num[left] <= target < num[mid])
            right=mid-1;
        else
            left=mid+1;
    } 
    else if (num[mid] < num[right]){
        if(num[mid] < target <= num[right])
            left=mid+1;
        else
            right=mid-1;
    }
    else
        right--;
return false;
```

<a id="markdown-旋转数组找min" name="旋转数组找min"></a>
## 旋转数组找min
>>（含重复元素）

```cpp
int left = 0, right = nums.size() - 1, res = nums[0];
while (left < right - 1) {
    int mid = (right + left) / 2;
    if (nums[left] < nums[mid]) {
        res = min(res, nums[left]);
        left = mid + 1;
    } else if (nums[left] > nums[mid]) {
        res = min(res, nums[right]);
        right = mid;
    } else 
        ++left;
}
res = min(res, nums[left]，nums[right]);
return res;
```


<a id="markdown-寻找两个有序数组中位数" name="寻找两个有序数组中位数"></a>
## 寻找两个有序数组中位数
```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int m = nums1.size(), n = nums2.size();
    int left = (m + n + 1) / 2, right = (m + n + 2) / 2;
    return (findKth(nums1, 0, nums2, 0, left) + findKth(nums1, 0, nums2, 0, right)) / 2.0;
}
int findKth(vector<int>& nums1, int i, vector<int>& nums2, int j, int k) {
    if (i >= nums1.size()) 
        return nums2[j + k - 1];
    if (j >= nums2.size()) 
        return nums1[i + k - 1];
    if (k == 1) 
        return min(nums1[i], nums2[j]);
        
    int m1 = (i + k / 2 - 1 < nums1.size()) ? nums1[i + k / 2 - 1] : INT_MAX;
    int m2 = (j + k / 2 - 1 < nums2.size()) ? nums2[j + k / 2 - 1] : INT_MAX;
        
    if (m1 < m2) {
        return findKth(nums1, i + k / 2, nums2, j, k - k / 2);
    } else {
        return findKth(nums1, i, nums2, j + k / 2, k - k / 2);
    }
}
```

<a id="markdown-逆序数" name="逆序数"></a>
## 逆序数
3-2-4-1
```cpp
//对数组nums倒序构造vector
vector<Node*>node;
for(int i=nums.size()-1;i>=0;i--;){
    node.push_back(new Node(nums[i]));
}
for(int i=0;i < node.size();i++;){
    count_small=0;
    BST_insert(node[0],node[i],count_small); //递归
    tmp.push_back(count_small);
}
return vector<int>(tmp.rbegin(),tmp.rend());
/*****************************************************/
//二叉搜索树的插入操作：
BST_insert(root,node,count_small){
    if(node->val<root->val){
        root->count++;
        if(root->left)
            BST_insert(root->left,node,count_small);
        else
            root->left=node;
    }else{
        count_small+=(root->count+1);//count_small记录比待插入节点的小个数，因为是倒序插入的
        if(root->right)
            BST_insert(root->right,node,count_small);
        else
            root->right=node;
    }
}
```

<a id="markdown-山脉三题" name="山脉三题"></a>
## 山脉三题
```cpp
//判断山峰
bool validMountainArray(int[] A) {
    if (A == null || A.size() < 3)
        return false;
    int n = A.size() - 1;
    int l = 0, r = n;
    while (l < n) {
        if (A[l] < A[l + 1])
            l++;
        else
            break;
    }
    while (r >= 1) {
        if (A[r] < A[r - 1])
            r--;
        else
            break;
    }
    return l > 0 && r < n && l == r;
}

//寻找峰顶
int peakIndexInMountainArray(int[] A) {
    int l = 1, r = A.size() - 1;
    while (l < r) {
        int mid = l + (r - l) / 2;
        if (A[mid] > A[mid - 1] && A[mid] < A[mid + 1]) {
            l = mid + 1;
        } else {
            r = mid;
        }
    }
    return l;
}

//寻找最长山脉-上下长度
int longestMountain(vector<int>& A) {
    int res = 0, up = 0, down = 0, n = A.size();
    for (int i = 1; i < n; ++i) {
        if ((down && A[i - 1] < A[i]) || (A[i - 1] == A[i])) {
            up = down = 0;
        }
        if (A[i - 1] < A[i]) ++up;
        if (A[i - 1] > A[i]) ++down;
        if (up > 0 && down > 0) res = max(res, up + down + 1);
    }
    return res;
}

//寻找最长山脉-左右索引
int longestMountain(vector<int>& A) {
    int res = 0, n = A.size();int l,r
    for (int i = 1; i < n - 1; ++i) {
        if (A[i - 1] < A[i] && A[i + 1] < A[i]) {
            int left = i - 1, right = i + 1;
            while (left > 0 && A[left - 1] < A[left]) --left;
            while (right < n - 1 && A[right] > A[right + 1]) ++right;
            if(right-left+1>res){
                res=right-left+1;
                l=left;
                r=right;
            }
        }
    }
    return res;
}
//两种方法复杂度分析：上下长度：不能得到左右下标，仅能通过长度相加得到山脉长度；左右索引：通过索引得到长度。
上下长度：O(N)时间；
左右索引：峰的个数m；平均峰的长度为n/m；所以每个峰在while中循环n/m次。最终复杂度也为O(N)。

```
