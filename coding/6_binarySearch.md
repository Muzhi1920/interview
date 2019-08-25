# 二分查找
使用二分的前提是给定链表或数组是有序的；在此基础上的变形比如排序树组旋转；两个有序数组的中位数

---
- 旋转数组的搜索（含重复元素）
```cpp
while left <= right
    if num[mid]==target
        return true;
    else if mid < right{
        if mid < target <= right
            left=mid+1;
        else
            right=mid-1;
    }
    else if mid > right{
        if left <= target < mid
            right=mid-1;
        else
            left=mid+1;
    }
    else
        right--;
return false;
```

- 旋转数组找最小值（含重复元素）
```cpp
while (left < right - 1)
    if mid > left //说明中值在左侧递增内
        res=min(res,left);
        left=mid+1;
    else if mid < left //说明在右侧递增区间内
        res=min(res,right);
        right=mid;
    else
        left++;
return min(left,right,res);//由于有重复元素故若是重复元素处切分则最终需判断一下
```

- 寻找两个有序数组中位数

```cpp
left=(m+n+1)/2;
right=(m+n+2)/2
mid=(nums[left]+nums[right])/2
//问题转为[find(nums1, 0, nums2, 0, left) + find(nums1, 0, nums2, 0, right)] / 2.0

int find(nums1,i,nums2,j,k) //递归
    if (i==nums1,size()-1) return nums2[j+k-1];
    if (j==nums2.size()-1) return nums1[i+k-1];
    if k==1
        return min(nums1[i],nums2[j])
    m1 = nums1[i + k / 2 - 1] or INT_MAX; //获取nums1,nums2的中位数如果存在
    m2 = nums2[j + k / 2 - 1] or INT_MAX;
    if m1 < m2:
        return findKth(nums1, i + k / 2, nums2, j, k - k / 2);
    else:
        return findKth(nums1, i, nums2, j + k / 2, k - k / 2);

```
- 查找插入位置
```cpp
二分初始化index = -1
    取中值；
    if 相等：
        index=mid；
    else if target < mid:
        if mid==0 or target > left：
            index=mid;
        end=mid - 1;
    else if target > mid
        if mid==size-1 or target<right
            index=mid+1;
        begin=mid + 1;
return index;
```

- 逆序数
3-2-4-1
```cpp
对数组nums倒序构造vector  [1-4-2-3]
遍历：
    count_small=0;
    BST_insert(node[0],node[i],count_small); //递归
    count_small入tmp
倒序保存tmp的count_small到res中，返回
/*****************************************************/
二叉搜索树的插入操作：
BST_insert(node(0),node[i],count_small)
    if 插入的值小于<根：
        根->count++;
        if(根->left)
            递归插入左
        else:
            根->left=insert_node
    else:
        count_small+=根的count+1  //count_small记录比待插入节点的小个数，因为是倒序插入的
        if(根->right)
            递归插入右
        else
            根->right=insert_node
```
