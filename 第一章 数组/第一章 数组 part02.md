# 第一章 数组 part02

Date: August 21, 2024
Tags: 数组, 知识总结

## **209.长度最小的子数组**

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int left = 0;
        int right = 0;
        int sum = 0;
        int len = INT32_MAX;
        while(right<nums.size()){
            sum+=nums[right];
            while(sum>=target){
                len=len<(right-left+1)?len:(right-left+1);
                sum-=nums[left];
                left++;
            }
            right++;
        }
        if(len!=INT32_MAX){
            return len;
        }
        return 0;
    }
};
```

感觉我的逻辑一塌糊涂。建议把这段代码多看几遍，因为抠了很久才抠出来。。

两个循环把我整的懵懵的。

首先要判断是哪个指针带着往上加sum的

比如我这个解法就是right，当right不超限制时，判断sum到没到目标

到了目标就left++，所以这里用了第二层循环。

## **59.螺旋矩阵II**

[代码随想录](https://programmercarl.com/0059.螺旋矩阵II.html)

## **58. 区间和**

[58. 区间和 | 代码随想录](https://programmercarl.com/kamacoder/0058.区间和.html)

## **44. 开发商购买土地**

[44. 开发商购买土地 | 代码随想录](https://programmercarl.com/kamacoder/0044.开发商购买土地.html)