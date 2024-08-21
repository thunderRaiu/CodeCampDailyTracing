# 第四章 字符串 part01

Date: August 21, 2024
Tags: 字符串, 知识总结

## 344. 反转字符串

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for(int i=0; i<s.size()/2; i++){
            char tmp = s[i];
            s[i]=s[s.size()-i-1];
            s[s.size()-i-1] = tmp;
         
        }
//      for (int i = 0, j = s.size() - 1; i < s.size()/2; i++, j--) {
//      swap(s[i],s[j]);
    }
};
```

## **541. 反转字符串II**

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

**———当需要固定规律一段一段去处理字符串的时候，要想想在在for循环的表达式上做做文章**

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i=0; i<s.size();i+=2*k){
            if(i+k<=s.size()){
                for(int j=0; j<k/2; j++){
                    swap(s[i+j],s[i+k-j-1]);
                }
            }else{
                for(int j=0; j<s.size()%k/2; j++){
                    swap(s[i+j],s[i+s.size()%k-j-1]);
                } 
            }
        }
        return s;
    }
};
```

---

**（参考答案好简单的写法ww，而我框框地提交错了两次最后用时和内存也耗费很大。）**

善用string的成员函数。。我像个原始人。。

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += (2 * k)) {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k );
            } else {
                // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
};
```

用pos标记要反转的字符的起点

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.size(),pos = 0;
        while(pos < n){
            //剩余字符串大于等于k的情况
            if(pos + k < n) reverse(s.begin() + pos, s.begin() + pos + k);
            //剩余字符串不足k的情况
            else reverse(s.begin() + pos,s.end());
            pos += 2 * k;
        }
        return s;
    }
};
```

## 54. 替换数字（第八期模拟笔试）

```cpp
#include <iostream>
using namespace std;
int main(){
    string s;
    int count = 0;
    while(cin>>s){

        for(int i=0;i<s.size();i++){
            if(s[i]>='0'&&s[i]<='9'){
                count++;
            }
        }
    }        
    int oldtail=s.size()-1;
    s.resize(s.size()+count*5);
    int tail = s.size()-1;
    for(int i=oldtail; i>=0; i--){
        if(s[i]>='0'&&s[i]<='9'){
            s[tail--]='r';
            s[tail--]='e';
            s[tail--]='b';
            s[tail--]='m';
            s[tail--]='u';
            s[tail--]='n';
        }else s[tail--]=s[i];
    }
    cout<<s;
}
```