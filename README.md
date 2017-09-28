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


## Linkedlist

###  2. Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```C++
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```
Hint: use val = sum % 10 , sum = sum/10
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

###  445. Add Two Numbers II
You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Follow up:
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

Example:
```C++
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```
Hint: use STL stack,  val = sum % 10 , sum = sum/10
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
        ListNode* l3, * head;
        l3=(struct ListNode*)malloc(sizeof(struct ListNode));
        l3->next=NULL;
        std::stack<int> m1, m2;
        int tmp1=0, tmp2=0, tmp3=0, sum=0;        
        while(l1){
            m1.push(l1->val);
            l1=l1->next;
        }
        while(l2){
            m2.push(l2->val);
            l2=l2->next;
        }
        while(!m2.empty() || !m1.empty())
        {    
            tmp2=0;
            tmp1=0;
            if(!m1.empty())
            {
                tmp1=m1.top();
                m1.pop();
            }
            if(!m2.empty())
            {
                tmp2=m2.top();
                m2.pop();
            }
            tmp3=tmp1+tmp2;
            sum=sum+tmp3;
            l3->val = sum % 10;
            head=(struct ListNode*)malloc(sizeof(struct ListNode));
            head->next=l3;
            l3=head;     
            sum=sum/10;
        }
        if(sum!=0)
        {
            l3->val = sum % 10;
            head=(struct ListNode*)malloc(sizeof(struct ListNode));
            head->next=l3;
            l3=head;
        }
        return l3->next;
    }
};
```
<br/>

###  328. Odd Even Linked List
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

Example:
Given 1->2->3->4->5->NULL,
return 1->3->5->2->4->NULL.

Note:
The relative order inside both the even and odd groups should remain as it was in the input. 
The first node is considered odd, the second node even and so on ...

Credits:
Special thanks to @DjangoUnchained for adding this problem and creating all test cases.

Hint: connect odd end with even head
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
    ListNode* oddEvenList(ListNode* head) {
        if (!head || !head->next)
        {
            return head;
        }
        ListNode *odd, *even, *even_head;
        odd = head;
        even = head->next;
        even_head = even;

        while (even && even->next) 
        {
            odd->next = even->next;
            odd = odd->next;

            even->next = odd->next;
            even = even->next;
        }
        odd->next = even_head;
        return head;
    }
};
```
<br/>

### 237. Delete Node in a Linked List
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

Hint: change the now Node and the next Node val
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
    void deleteNode(ListNode* node) {
        ListNode * tmp = node->next;
        node->val = node->next->val;
        node->next = node->next->next;
        delete tmp;    
    }
};
```
<br/>

### 203. Remove Linked List Elements
Remove all elements from a linked list of integers that have value val.

Example
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5
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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *first, * tmp, * ptr;
        first = (ListNode*)malloc(sizeof(ListNode));
        ptr = first;
        first->next=head;
        while(ptr->next)
        {
            if(ptr->next->val==val)   
            {   
                tmp = ptr-> next;
                ptr->next = ptr->next->next;
                delete tmp;
            }
            else
            {
                ptr = ptr->next;
            }
        }        
        return first->next; 
    }
};
```
<br/>

### 141. Linked List Cycle
Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

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
    bool hasCycle(ListNode *head) {
        ListNode *slow, *fast ;
        slow=head;
        fast=head;
        while(fast && fast->next){
            slow=slow->next;
            fast=fast->next->next;
            if(slow==fast) return true;  
        }
        return false;
    }
};
```

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
<br/>

### 206. Reverse Linked List

Reverse a singly linked list.
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
    ListNode* reverseList(ListNode* head) {
        struct ListNode * prev, *current, *next;   
        prev   = NULL;
        current = head;
        while (current ) //current != NULL
        {
            next  = current->next;  
            current->next = prev;   
            prev = current;
            current = next;
        }
        return prev;  

    }
};

```
















