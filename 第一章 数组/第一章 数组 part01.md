# 第一章 数组

Date: August 18, 2024
Tags: 数组, 知识总结

首先第一题就浅浅败了，二分查找：

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size();
        int left = 0;
        int right = n-1;
        int index = (n-1)/2;
        while(nums[index]!=target){
            index = (left+right)/2;
            if(nums[index]<target){
                left = index+1;
            }else{
                right = index-1;
            }
        }
        return nums[index];
    }
};
```

报错：超时，于是去查讲解

### 二分查找

前提：有序、无重复

重点：边界条件

题解中使用了`middle = left + ((right - left) / 2);***// 防止溢出 等同于(left + right)/2***`

---

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size();
        int left = 0;
        int right = n-1;
        int index = right;
        // while(nums[index]!=target){
        while(left<right){
            index = left+(right-left)/2;
            if(nums[index]<target){
                left = index+1;
            }else if(nums[index]>target){
                right = index-1;
            }else{
                return index;
            }
        }
        return -1;
    }
};
```

代码改过之后，直接有了新的错误，之前完全没有考虑

<aside>
<img src="https://www.notion.so/icons/code_pink.svg" alt="https://www.notion.so/icons/code_pink.svg" width="40px" /> **输入**

nums =[5]

target =5

**输出**

-1

**预期结果**

0

</aside>

。。这个情况下，应该在while前就判断一下index是不是所求的值

所以为了避免这个情况，还是直接在while的时候 把条件设置成`while(left<=right)`更不容易错。。