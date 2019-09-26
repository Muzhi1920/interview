<a id="markdown-二分查找" name="二分查找"></a>
# 二分查找
使用二分的前提是给定链表或数组是有序的；在此基础上的变形比如排序树组旋转；两个有序数组的中位数

---
<!-- TOC -->

- [二分查找](#二分查找)
    - [旋转数组的搜索target](#旋转数组的搜索target)
    - [旋转数组找min](#旋转数组找min)
    - [寻找两个有序数组中位数](#寻找两个有序数组中位数)
    - [逆序数](#逆序数)

<!-- /TOC -->

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
while (left < right-1){
    if(num[mid] > num[left]) //说明中值在左侧递增内,与左侧值比
        res=min(res,num[left]);
        left=mid+1;
    else if(num[mid] < num[left]) //说明在右侧递增区间内，与右侧值比
        res=min(res,num[right]);
        right=mid;
    else
        left++;
}
return min(num[left],num[right],num[res]);//若是重复元素处切分
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
