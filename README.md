# LeeCode_example

---
### reference
- [1](http://www.cnblogs.com/grandyang/p/4606334.html)
- [2](http://uploadfiles.nowcoder.com/pdf/leetcode-cpp.pdf)
- [c++](http://www.cplusplus.com/reference/)
###  1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

**O(n^2):**
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> output;
        for(int i=0; i<nums.size(); i++)
            for(int j=i+1; j<nums.size(); j++)
            {
              if(nums[i]+nums[j]==target)
              {
                  output.push_back(i);
                  output.push_back(j);
                  return output;
              }   
            }
    }
};
```
**O(n):**
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> m;
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            m[nums[i]] = i;
        }
        for (int i = 0; i < nums.size(); ++i) {
            int t = target - nums[i];
            if (m.count(t) && m[t] != i) {
                res.push_back(i);
                res.push_back(m[t]);
                break;
            }
        }
        return res;
    }
};
```

<br/>

###  2. Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        /*
        struct ListNode *head, *res, *cur;
        head = NULL;
        int carry = 0;
        while (l1 || l2) 
        {
            int n1 = l1 ? l1->val : 0;
            int n2 = l2 ? l2->val : 0;
            int sum = n1 + n2 + carry;
            carry = sum / 10;
            cur = (struct ListNode*)malloc(sizeof(struct ListNode));
            cur->val = sum % 10;
            cur->next=NULL;
            if(head==NULL) head=cur;
            else res->next=cur;
            res=cur;
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }
      
        if (carry){ 
        cur = (struct ListNode*)malloc(sizeof(struct ListNode));
        cur->val = 1;
        cur->next=NULL;
        res->next=cur;
        res=cur;
        }
        return head;
        */
        
        struct ListNode *res, *cur;
        cur = (struct ListNode*)malloc(sizeof(struct ListNode));
        res=cur;
        int carry = 0;
        while (l1 || l2) 
        {
            int n1 = l1 ? l1->val : 0;
            int n2 = l2 ? l2->val : 0;
            int sum = n1 + n2 + carry;
            carry = sum / 10;
            cur->next = (struct ListNode*)malloc(sizeof(struct ListNode));
            cur->next->val = sum % 10;
            cur=cur->next;
            cur->next=NULL;
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }
      
        if (carry){ 
        cur->next = (struct ListNode*)malloc(sizeof(struct ListNode));
        cur->next->val = 1;
        cur->next->next=NULL;
        }
        return res->next;       
    }   
};
```
<br/>

------

## Reverse

### 344. Reverse String
Write a function that takes a string as input and returns the string reversed.

Example:
Given s = "hello", return "olleh".<br/>

hints:直接從兩頭往中間走，同時交換兩邊的字符即可:
```C++
class Solution {
public:
    string reverseString(string s) {
        int left = 0, right = s.size() - 1;
        while (left < right) {
            char t = s[left];
            s[left] = s[right];
            s[right] = t;
            left++;
            right--;
        }
        return s;
    }
};
```
利用swap()
```C++
class Solution {
public:
    string reverseString(string s) {
        int left = 0, right = s.size() - 1;
        while (left < right) {
            swap(s[left++], s[right--]);
        }
        return s;
    }
};
```

利用std::reverse()
```C++
class Solution {
public:
    string reverseString(string s) {
        reverse(s.begin(),s.end());
        return s;
    }
};
```
<br/>

### 541. Reverse String II
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.
Example:
```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```
Restrictions:
The string consists of lower English letters only.
Length of the given string and k will in the range [1, 10000]

hints: 字串每隔2k間隔翻轉前k個字元，如範例為每個4(2xk個)字元翻轉前2(k個)個字元。<br/>

```C++
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += 2 * k) {
            reverse(s.begin() + i, min(s.begin() + i + k, s.end()));
        }
        return s;
    }
};
```
<br/>

std::reverse 使用範例如下:
```C++
#include <vector>     // vector
#include <iostream>   // cout
#include <iterator>   // begin, end
#include <algorithm>  // reverse
int main()
{
    std::vector<int> v({1,2,3});
    std::reverse(std::begin(v), std::end(v));
    std::cout << v[0] << v[1] << v[2] << '\n';
 
    int a[] = {4, 5, 6, 7};
    std::reverse(std::begin(a), std::end(a));
    std::cout << a[0] << a[1] << a[2] << a[3] << '\n';
}
```
<br/>

 
### 151. Reverse Words in a String
Given an input string, reverse the string word by word.<br/>

For example,<br/>
```
Given s = "the sky is blue",
return "blue is sky the".
```
Update (2015-02-12):<br/>
For C programmers: Try to solve it in-place in O(1) space.<br/>

hints:需考慮輸入字串頭尾是否有空白、輸入的字串是否為連續空白，以及每個分段字串之間是否為連續空白而非單個空白分段。<br/>
```C++
class Solution {
public:
    void reverseWords(string &s) {
        stringstream is;
        string tmp, s_out;
        is << s;
        is >> s_out;      
        while(is >> tmp)
        {
            s_out = tmp + " " + s_out;
        }
        //if(!s_out.empty() && s_out[0] == ' ') 
        //{
        //    s_out = "";
        //}
        s=s_out;
    }
    
};
```
含有非空格字符，那麼每次 **>>** 操作就會提取連在一起的非空格字符，若有空格字符則自動分段非空格字符串輸出。<br/>
若streamstring已無任何分段字符串，則輸出為空("") <br/>
<br/>

**StringStream 是一個專門用來處理讀取或寫入到String的類別。<br/>**
**最常見的使用方法是拿StringStream來做字串的分割（空白切割）或int與String類別的之間的轉換。<br/>**
- [streamstring](https://dotblogs.com.tw/v6610688/2013/11/08/cplusplus_stringstream_int_and_string_convert_and_clear)
<br/>

### 186. Reverse Words in a String II 

Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters. <br/>
The input string does not contain leading or trailing spaces and the words are always separated by a single space. <br/>
For example, <br/>
```
Given s = "the sky is blue", 
return "blue is sky the". 
```
Could you do it in-place without allocating extra space? <br/>

hints:此題與151. Reverse Words in a String 解法相同，且此題不必考慮頭尾空白及雙空白分段之特殊情況。 <br/>
<br/>

### 557. Reverse Words in a String III
Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.<br/>

Example 1:<br/>
```
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"
```
Note: In the string, each word is separated by single space and there will not be any extra space in the string.<br/>

```C++
class Solution {
public:
    string reverseWords(string s) {
    /*
    int num=0;
	for (int i = 0; i<s.size(); i++)
	{
		if (s[i] == ' '|| i==s.size()-1)
		{	
			if (i == s.size() - 1)
			{
				reverse(s.begin() + num, s.end());
			}
			else
			{
				reverse(s.begin() + num, s.begin() + i);
			}
			num = i+1;
		}
	}      
    return s;
    
    */
    stringstream ss;
    ss<<s;
    string s1,s2;
    while(ss>>s1)
    {
        reverse(s1.begin(),s1.end());
        s2=s2+s1+" ";        
    }
    s2.pop_back();
    return s2;    
    }
};
```



