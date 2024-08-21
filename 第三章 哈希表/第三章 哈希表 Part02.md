# 第三章 哈希表 Part02

Date: August 20, 2024
Tags: 哈希表, 知识总结

## **454.四数相加II**

思路，AB两数组存储和至map中，value记数

再查找CD两数组和，每命中一次加一次value记数至count

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        std::unordered_map<int,int> sum;
        for(int i=0;i<nums1.size();i++){
            for(int j=0;j<nums2.size();j++){
                sum[nums1[i]+nums2[j]]++;
            }
        }
        int count = 0;
        for(int c : nums3){
            for(int d :nums4){
                if(sum.find(0-c-d)!=sum.end()){
                    count+=sum[0-c-d];
                }
            }
        }
        return count;
    }
};
```

小tips 尽可能用 简便的表达，例如 `for(int i=0;i<nums1.size();i++)` vs `for(int c : nums3)`

## 383. 赎金信

判断字符串magazine中的字母成分能不能构成ransomNote

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
            std::unordered_map<int,int> alphabet;
            for(int a =0; a <magazine.size();a++){
                alphabet[magazine[a]-'a']++;
            }
            for(int b = 0; b<ransomNote.size(); b++){
                if(alphabet[ransomNote[b]-'a']>0){
                    alphabet[ransomNote[b]-'a']--;
                }else{
                    return false;
                }
            }
            return true;

    }
};
```

记录第一次，一次通过。。

but

**其实在本题的情况下，使用map的空间消耗要比数组大一些的，因为map要维护红黑树或者哈希表，而且还要做哈希函数，是费时的！数据量大的话就能体现出来差别了。 所以数组更加简单直接有效！**

 `int record[26] = {0};`

（手动狗头，也不是所有情况都用map数组效果好。。）

## **15. 三数之和**

一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。

就是所有的方法前提都是要把数组排序，。（把教程里的代码贴上来了，——我自己写的有点稀碎）（甚至用了将近两小时）

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        // 找出a + b + c = 0
        // a = nums[i], b = nums[left], c = nums[right]
        for (int i = 0; i < nums.size(); i++) {
            // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) {
                return result;
            }
            // 错误去重a方法，将会漏掉-1,-1,2 这种情况
            /*
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            */
            // 正确去重a方法
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                // 去重复逻辑如果放在这里，0，0，0 的情况，可能直接导致 right<=left 了，从而漏掉了 0,0,0 这种三元组
                /*
                while (right > left && nums[right] == nums[right - 1]) right--;
                while (right > left && nums[left] == nums[left + 1]) left++;
                */
                if (nums[i] + nums[left] + nums[right] > 0) right--;
                else if (nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;

                    // 找到答案时，双指针同时收缩
                    right--;
                    left++;
                }
            }

        }
        return result;
    }
};
```

## 18. 四数之和

注意出错情况

Line 20: Char 54: runtime error: signed integer overflow: 2000000000 + 1000000000 cannot be represented in type 'value_type' (aka 'int') (solution.cpp)
SUMMARY: UndefinedBehaviorSanitizer: undefined-behavior solution.cpp:20:54

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            // 剪枝处理
            if (nums[k] > target && nums[k] >= 0) {
            	break; // 这里使用break，统一通过最后的return返回
            }
            // 对nums[k]去重
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                // 2级剪枝处理
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) {
                    break;
                }

                // 对nums[i]去重
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if (**(long)** nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if (**(long)** nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 对nums[left]和nums[right]去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }

            }
        }
        return result;
    }
};

```