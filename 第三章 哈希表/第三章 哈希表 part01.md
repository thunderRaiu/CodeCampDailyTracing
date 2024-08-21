# 第三章 哈希表 part01

Date: August 20, 2024
Tags: 哈希表, 知识总结

## 242.有效的字母异位词

用一个26位的数组记录

初始化用的语句：`int record[26]={0};`

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
            if(s.size()!=t.size()){
                return false;
            }
            int record[26]={0};
            for(int i=0; i<s.size();i++){
                record[s[i]-'a']++;
            }
            for(int i=0; i<s.size();i++){
                record[t[i]-'a']--;
            }
            for(int i=0; i<26;i++){
                if(record[i]!=0){
                    return false;
                }
            }
            return true;
    }
};
```

## 349. 两个数组的交集

数组中的数值的大小不像字母表一样有限，所以哈希表的设置不能单纯用上题的那种自定义的`record[26]`

set:

| **set** | 底层实现 | 是否有序 | 数值可否重复 | 能否更改数值 | 查询效率 | 增删效率 |
| --- | --- | --- | --- | --- | --- | --- |
| std::set | 红黑树 | 有序 | 否 | 否 | O(log n) | O(log n) |
| std::multiset | 红黑树 | 有序 | 是 | 否 | O(logn) | O(logn) |
| std::unordered_set | 哈希表 | 无序 | 否 | 否 | O(1) | O(1) |

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        std::unordered_set<int> result;
        std::unordered_set<int> nums1_set(nums1.begin(),nums1.end());
        for(int num : nums2){
            if(nums1_set.count(num)>0){
                result.insert(num);
            }
        }
        return vector<int>(result.begin(),result.end());
    }

};
```

### 后续

力扣改了 题目描述 和 后台测试数据，增添了 数值范围：

- 1 <= nums1.length, nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 1000

所以就可以 使用数组来做哈希表了， 因为数组都是 1000以内的。

对应C++代码如下：

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果，之所以用set是为了给结果集去重
        int hash[1005] = {0}; // 默认数值为0
        for (int num : nums1) { // nums1中出现的字母在hash数组中做记录
            hash[num] = 1;
        }
        for (int num : nums2) { // nums2中出现话，result记录
            if (hash[num] == 1) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};

```

- 时间复杂度: $O(m + n)$
- 空间复杂度: $O(n)$

### related question **350. 两个数组的交集 II**

给你两个整数数组 `nums1` 和 `nums2` ，请你以数组形式返回两数组的交集。**返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致**（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]

```

**示例 2:**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        int set[1001] = {0};
        std::multiset<int> result_set;
        for(int num : nums1){
            set[num] ++;
        }
        for(int num :nums2){
            if(set[num]>0){
                result_set.insert(num);
                set[num]--;
            }
        }
        return vector<int> (result_set.begin(),result_set.end());
    }
};
```

## **202. 快乐数**

[**力扣题目链接(opens new window)**](https://leetcode.cn/problems/happy-number/)

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

如果 n 是快乐数就返回 True ；不是，则返回 False 。

**示例：**

输入：19

输出：true

解释：

1^2 + 9^2 = 82

8^2 + 2^2 = 68

6^2 + 8^2 = 100

1^2 + 0^2 + 0^2 = 1

思路：无限循环则会不是快乐数，所以可以想到使用哈希表

```cpp
class Solution {
public:
    int getsum(int n){
        int sum = 0;
        while(n){
            sum += (n%10)*(n%10);
            n/=10;
        }
        return sum;
    }
    bool isHappy(int n) {
        std::unordered_set<int> sumset;
        while(1){
            int sum = getsum(n);
            if(sum == 1){
                return true;
            }
            if(sumset.count(sum)>0){
                return false;
            }else{
                sumset.insert(sum);
            }
            n=sum;
        }
        
    }
};
```

## **1. 两数之和**

查找一个数组里两个元素之和等于target

若暴力求解的话会有$O(n^2)$的复杂度，为了降低复杂度，

通过题目可以很快的计算出目标数值，所以可以用一个集合放已然遍历过的元素，因为哈希表map的查找复杂度是O(1)，所以大大降低复杂度

| **map** | 底层实现 | 是否有序 | 数值可否重复 | 能否更改数值 | 查询效率 | 增删效率 |
| --- | --- | --- | --- | --- | --- | --- |
| std::map | 红黑树 | key有序 | key不可重复 | key不可修改 | O(logn) | O(logn) |
| std::multimap | 红黑树 | key有序 | key可重复 | key不可修改 | O(log n) | O(log n) |
| std::unordered_map | 哈希表 | key无 序 | key不可重复 | key不可修改 | O(1) | O(1) |

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int,int> set;

        for(int i=0; i<nums.size(); i++){
            auto it = set.find(target-nums[i]);
            if(it!=set.end()){
                return {it->second,i};
            }else{
                set.insert(pair<int,int>(nums[i],i));
            }
        }
        return {};
    }
};
```

注意几个map的用法

**查找元素：**

- 使用 `find` 方法查找元素，返回一个**迭代器**，如果未找到元素，迭代器会等于 `end()`。

```cpp
auto it = myMap.find(2);
if (it != myMap.end()) {
    std::cout << "Key 2 found with value: " << **it->second** << std::endl;
} else {
    std::cout << "Key 2 not found." << std::endl;
}
```

插入元素：

```cpp
std::unordered_map<int, std::string> myMap;
myMap[1] = "one";  // 如果键1不存在，则插入；如果存在，则更新值
myMap.insert({2, "two"});  // 使用 insert 插入新元素
**myMap.insert(pair<int,string>(i,string[i])); //使用pair把数组对加进去**
```