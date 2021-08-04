

[**c_str**](https://www.cplusplus.com/reference/string/string/c_str/)

Get C string equivalent (public member function )

Returns a pointer to an array that contains a null-terminated sequence of characters (i.e., a C-string) representing the current value of the [string](https://www.cplusplus.com/string) object.

[**data**](https://www.cplusplus.com/reference/string/string/data/)

Get string data (public member function )

Returns a pointer to an array that contains a null-terminated sequence of characters (i.e., a C-string) representing the current value of the string object.

This array includes the same sequence of characters that make up the value of the string object plus an additional terminating null-character ('\0') at the end.

The pointer returned points to the internal array currently used by the string object to store the characters that conform its value.

**Both string::data and string::c_str are synonyms and return the same value.**



[**find**](https://www.cplusplus.com/reference/string/string/find/)

Find content in string (public member function )

Searches the [string](https://www.cplusplus.com/string) for the first occurrence of the sequence specified by its arguments.

When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences that include characters before *pos*.

Return Value

The position of the first character of the first match.
If no matches were found, the function returns [string::npos](https://www.cplusplus.com/string::npos).

```cpp

string (1)	
size_t find (const string& str, size_t pos = 0) const noexcept;
c-string (2)	
size_t find (const char* s, size_t pos = 0) const;
buffer (3)	
size_t find (const char* s, size_t pos, size_type n) const;
character (4)	
size_t find (char c, size_t pos = 0) const noexcept;

// string::find
#include <iostream>       // std::cout
#include <string>         // std::string

int main ()
{
  std::string str ("There are two needles in this haystack with needles.");
  std::string str2 ("needle");

  // different member versions of find in the same order as above:
  std::size_t found = str.find(str2);
  if (found!=std::string::npos)
    std::cout << "first 'needle' found at: " << found << '\n';

  found=str.find("needles are small",found+1,6);
  if (found!=std::string::npos)
    std::cout << "second 'needle' found at: " << found << '\n';

  found=str.find("haystack");
  if (found!=std::string::npos)
    std::cout << "'haystack' also found at: " << found << '\n';

  found=str.find('.');
  if (found!=std::string::npos)
    std::cout << "Period found at: " << found << '\n';

  // let's replace the first needle:
  str.replace(str.find(str2),str2.length(),"preposition");
  std::cout << str << '\n';

  return 0;
}
first 'needle' found at: 14
second 'needle' found at: 44
'haystack' also found at: 30
Period found at: 51
There are two prepositions in this haystack with needles.
```



Searches the [string](https://www.cplusplus.com/string) for the first occurrence of the sequence specified by its arguments.

When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences that include characters before *pos*.

Notice that unlike member [find_first_of](https://www.cplusplus.com/string::find_first_of), whenever more than one character is being searched for, it is not enough that just one of these characters match, but the entire sequence must match.



[**find_first_of**](https://www.cplusplus.com/reference/string/string/find_first_of/)

Find character in string (public member function )

```cpp
string (1)	
size_t find_first_of (const string& str, size_t pos = 0) const noexcept;
c-string (2)	
size_t find_first_of (const char* s, size_t pos = 0) const;
buffer (3)	
size_t find_first_of (const char* s, size_t pos, size_t n) const;
character (4)	
size_t find_first_of (char c, size_t pos = 0) const noexcept;
```

Searches the [string](https://www.cplusplus.com/string) for the first character that matches **any** of the characters specified in its arguments.

When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences before *pos*.

```cpp
// string::find_first_of
#include <iostream>       // std::cout
#include <string>         // std::string
#include <cstddef>        // std::size_t

int main ()
{
  std::string str ("Please, replace the vowels in this sentence by asterisks.");
  std::size_t found = str.find_first_of("aeiou");
  while (found!=std::string::npos)
  {
    str[found]='*';
    found=str.find_first_of("aeiou",found+1);
  }

  std::cout << str << '\n';

  return 0;
}
```



[**substr**](https://www.cplusplus.com/reference/string/string/substr/)

Generate substring (public member function )

```cpp
string substr (size_t pos = 0, size_t len = npos) const;
```

Returns a newly constructed [string](https://www.cplusplus.com/string) object with its value initialized to a copy of a substring of this object.

```cpp
// string::substr
#include <iostream>
#include <string>

int main ()
{
  std::string str="We think in generalities, but we live in details.";
                                           // (quoting Alfred N. Whitehead)

  std::string str2 = str.substr (3,5);     // "think"

  std::size_t pos = str.find("live");      // position of "live" in str

  std::string str3 = str.substr (pos);     // get from "live" to the end

  std::cout << str2 << ' ' << str3 << '\n';

  return 0;
}
```

[**compare**](https://www.cplusplus.com/reference/string/string/compare/)

Compare strings (public member function )

```cpp
string (1)	
int compare (const string& str) const noexcept;
substrings (2)	
int compare (size_t pos, size_t len, const string& str) const;
int compare (size_t pos, size_t len, const string& str,
             size_t subpos, size_t sublen) const;
c-string (3)	
int compare (const char* s) const;
int compare (size_t pos, size_t len, const char* s) const;
buffer (4)	
int compare (size_t pos, size_t len, const char* s, size_t n) const;
```

| value | relation between *compared string* and *comparing string*    |
| ----- | ------------------------------------------------------------ |
| `0`   | They compare equal                                           |
| `<0`  | Either the value of the first character that does not match is lower in the *compared string*, or all compared characters match but the *compared string* is shorter. |
| `>0`  | Either the value of the first character that does not match is greater in the *compared string*, or all compared characters match but the *compared string* is longer. |





 C++、Python 中 ， 我们可以使用 `==` 来比较两个字符串；

C ++中，字符串是可变的。 也就是说，你可以像在数组中那样修改字符串



## 1. 字符串循环移位包含

[编程之美 3.1](https://github.com/CyC2018/CS-Notes/blob/master/notes/Leetcode 题解 - 字符串.md#)

```
s1 = AABCD, s2 = CDAA
Return : true
```

给定两个字符串 s1 和 s2，要求判定 s2 是否能够被 s1 做循环移位得到的字符串包含。

```cpp
bool is_sub(std::string main, std::string sub)
{
    std::string double_main = main + main;
    if(double_main.find(sub) != std::string::npos)
    {
        return true;
    }
    else
    {
        return false;
    }
    
}

int main()
{
    std::string main_string = "hello world";
    std::string sub_string = "dhell";
    if(is_sub(main_string, sub_string))
    {
        std::cout << sub_string << " is  " << main_string << "'s sub string" << std::endl;
    }
    else
    {
        std::cout << "not found sub string " << std::endl;
    }
}

```



## 2. 字符串循环移位

[编程之美 2.17](https://github.com/CyC2018/CS-Notes/blob/master/notes/Leetcode 题解 - 字符串.md#)

```
s = "abcd123" k = 3
Return "123abcd"
```

将字符串向右循环移动 k 位。

将 abcd123 中的 abcd 和 123 单独翻转，得到 dcba321，然后对整个字符串进行翻转，得到 123abcd。

```cpp
//mothed 2: 更加高效；
void move_string2(std::string& str, int k)
{
    k = k % str.size();
    char temp;
    for(int i = 0; i < k; i++)
    {
        int index = str.size() -1 -i;
        temp = str[index];
        while (index - k >= 0)
        {
            str[index] = str[index - k];
            index = index-k;
        }
        str[index] = temp;
    }
}

int main()
{
    std::string main_string = "hello world, hee";
    int k { 2 };
    move_string2(main_string, k);
    std::cout << main_string << std::endl;
}

```



```cpp
std::string move_string(std::string mainstring, int k)
{
    if(k > mainstring.size())
    {
        k = k % mainstring.size();
    }
    std::string sub_string2 = mainstring.substr(mainstring.size() - k);
    std::string sub_string1 = mainstring.substr(0, mainstring.size() - k);
    return sub_string2 + sub_string1;
}
```



## 3. 字符串中单词的翻转

[程序员代码面试指南](https://github.com/CyC2018/CS-Notes/blob/master/notes/Leetcode 题解 - 字符串.md#)

```
s = "I am a student"
Return "student a am I"
```

将每个单词翻转，然后将整个字符串翻转。

```cpp
void inv_single_string(std::string& str, int start, int end)
{
    char temp;
    std::cout << "come in " << std::endl;
    for(int i { 0 }; i < (end - start) / 2; i++)
    {
        std::cout << " i " << i << std::endl;
        temp = str[start + i];
        str[start + i] = str[end - i - 1];
        str[end - i - 1] = temp;
        std::cout << "endl " << std::endl;
    }
    
}

std::string inv_string(std::string main_string)
{
    size_t begin = 0;
    size_t  end = main_string.find(" ");
    while(end != std::string::npos)
    {
       
        inv_single_string(main_string, begin, end);
        begin = end + 1;
         end = main_string.find(" ",begin);
    }
    inv_single_string(main_string,begin,main_string.size());
    inv_single_string(main_string, 0, main_string.size());
    return main_string;
}

```

#### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

难度简单408收藏分享切换为英文接收动态反馈

给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

 

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

 

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母

 

**进阶:** 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？



目标：返回bool， 是否是字母异位词；

条件：两个字符串，词频相同；

特殊情况，字符长度不同，词频一定不同；

**思路1**：遍历两个字符串，并且把每个字母出现的词频记录在hash_map中；

遍历第一个字符串，查找对应的两个hash_map中的值是否相同；

```cpp
//官方解法；
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) {
            return false;
        }
        vector<int> table(26, 0);
        for (auto& ch: s) {
            table[ch - 'a']++;
        }
        for (auto& ch: t) {
            table[ch - 'a']--;
            if (table[ch - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

其实最后判断两个值是否相等的情况，都可以使用这种一增一减的方式，可以减少一次遍历的情况，这对最后的性能会有一定的提升；

#### [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

难度简单314

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
假设字符串的长度不会超过 1010。

**示例 1:**

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```



目标：返回最大长度；

条件：使用给定的字符串中含有的字母，构造；

​		区分大小写；

**要认识到包含字母的hash 映射，hash table 的大小都可以限定在一定的范围内，比如26？**

思路1：

​	构建一个26*2 大小的vector,遍历字符串，并且在26\*2 个字符中存储频次；

​	对每个element, 长度 += 值 / 2 再 \* 2, 如果value % 2 == 1, 标记存在奇数， 最后长度再加1；

```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        std::vector<int> data(26 * 2, 0);
        for(auto x : s)
        {
            if(x <= 'Z')
            {
                data[x - 'A']++;
            }
            else
            {
                data[26 + x - 'a']++;
            }
        }
        int length = 0;
        bool is_odd = false;
        for(auto e : data)
        {
            length += (e / 2 * 2);
            if(e % 2 == 1)
            {
                is_odd = true;
            }
        }
        if(is_odd)
        {
            length++;
        }
        return length;
    }
};
```



#### [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

难度简单374

给定两个字符串 ***s*** 和 ***t\***，判断它们是否是同构的。

如果 ***s*** 中的字符可以按某种映射关系替换得到 ***t\*** ，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

 

**示例 1:**

```
输入：s = "egg", t = "add"
输出：true
```

**示例 2：**

```
输入：s = "foo", t = "bar"
输出：false
```

**示例 3：**

```
输入：s = "paper", t = "title"
输出：true
```



target: 判断是否是重构的，输出布尔值；

条件： 要求是同构的；

需要建立两个哈希表，建立两个方向的映射关系，因为两个方向上的映射关系不是互相的，没有理解清楚题意；要读题三遍；

```cpp
//bugfree , quick;
class Solution {
public:
    bool isIsomorphic(string s, string t) 
    {
        if(s.size() != t.size())
        {
            return false;
        }
        std::unordered_map<char,char> hash_map1(26);
        std::unordered_map<char,char> hash_map2(26);
        for(int i { 0 }; i < s.size(); i++)
        {
            if (!hash_map1.count(s[i]) && !hash_map2.count(t[i]))
            {
                hash_map1[s[i]] = t[i];
                hash_map2[t[i]] = s[i]; 
            }
            else if(hash_map1[s[i]] == t[i])
            {
                continue;
            }
            else
            {
                return false;
            }
        }
        return true;
    }
};
```



#### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

难度中等646

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

**示例 1：**

```
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

 

**提示：**

- 输入的字符串长度不会超过 1000 。



目标： 计算回文字符串的总个数； A ， AA, 也都算回文字符串；

条件：从不同位置开始或者不同的位置结束，就算是不同的回文字符串；



A , AA, ABA；

计算长度： 计算以A 为中心的字符串，含有多少个回文， 计算以AA为中心的字符串含有多少个回文；

初始回文长度为字符串的长度。

计算总长度，计算边界， 遍历每个元素； 对每个元素执行统计操作；

第一种：以A为中心，像两边扩展，如果两边相同，则回文数加1

第二种，如果后一个和前一个相同，即有AA形式，则begin 和end 分别加1，来进行操作；

```cpp
//bug free, quick;
class Solution {
public:
    int countSubstrings(string s) {
        int result = s.size();
        int left = 0;
        int right = 0;
        for(int i { 0 }; i < s.size(); i++)
        {
            left = i - 1;
            right = i + 1;
            while(left >= 0 &&  right < s.size() && s[left] == s[right])
            {
                result++;
                left--;
                right++;
            }
            left = i;
            right = i+1;
            while(left >= 0 &&  right < s.size() && s[left] == s[right])
            {
                result++;
                left--;
                right++;
            }            
        }
        return result;
    }
};
```

> 官方回文思想：
>
> 计算有多少个回文子串的最朴素方法就是枚举出所有的回文子串，而枚举出所有的回文字串又有两种思路，分别是：
>
> 枚举出所有的子串，然后再判断这些子串是否是回文；
> 枚举每一个可能的回文中心，然后用两个指针分别向左右两边拓展，当两个指针指向的元素相同的时候就拓展，否则停止拓展。





#### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

难度简单1571

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，`121` 是回文，而 `123` 不是。

 

**示例 1：**

```
输入：x = 121
输出：true
```

**示例 2：**

```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3：**

```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

**示例 4：**

```
输入：x = -101
输出：false
```

 

**提示：**

- `-231 <= x <= 231 - 1`

 

**进阶：**你能不将整数转为字符串来解决这个问题吗？



targe  : false or ture ;

条件： 正数， 判断是否是回文；

思路1：

​	使用栈和队列； 除余 后 入栈和入队，然后检查出栈和出队元素是否相同；

​	时间复杂度为O(N), 空间复杂度为O(N)

思路2：

​	元素入vector中，然后左右两边向中间聚合检查；

官方思路：

​	把数字从中间对折，如果两个数字相等，则认为是回文数，否则不是回文数，判断是否到中间的条件是，前半部分的数字大于后半部分的数字；

```cpp
//个人解法： bugfree, quick;
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0)
        {
            return false;
        }
        std::vector<int> data;
        while(x != 0)
        {
            data.emplace_back(x % 10);
            x = x / 10;
        }
        int left = 0;
        int right = data.size() - 1;
        while(left < right)
        {
            if(data[left] != data[right])
            {
                return false;
            }
            else
            {
                left++;
                right--;
            }
        }
        return true;
    }
};

//官方思路解法：
//bugfree, quick;
class Solution {
public:
    bool isPalindrome(int x) {
        int left { x };
        int right { 0 };
        if(x < 0 || (x % 10 == 0 && x > 0))
        {
            return false;
        }
        while(left >= right)
        {
            right = right * 10 + left %10;
            if(left == right)
            {
                return true;
            }
            left = left / 10;
            if(left == right)
            {
                return true;
            }
        }
        return false;
    }
};
```



#### [696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

难度简单385

给定一个字符串 `s`，计算具有相同数量 0 和 1 的非空（连续）子字符串的数量，并且这些子字符串中的所有 0 和所有 1 都是连续的。

重复出现的子串要计算它们出现的次数。

 

**示例 1 :**

```
输入: "00110011"
输出: 6
解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。

请注意，一些重复出现的子串要计算它们出现的次数。

另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。
```

**示例 2 :**

```
输入: "10101"
输出: 4
解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。
```

 

**提示：**

- `s.length` 在1到50,000之间。
- `s` 只包含“0”或“1”字符。



target: 目标子字符串的数量

条件： 子字符串：具有相同数量的0 和 1， 且所有的0 聚集，所有的1 聚集；

​			位置不同按找不同的字符循环计数；

​			字符串非空；

​			子字符串在原字符串中连续；

思路1：以每个元素为起点，向后，遇到0 就++， 遇到1 就减减，跳出这两个循环的时候，如果结果为0， 说明是一个子串，有效字符串的数量+1， 遍历1 -> size-1 的元素；（超时了）

看官方解答：

>
>
>思路与算法
>
>我们可以将字符串 ss 按照 00 和 11 的连续段分组，存在 \textit{counts}counts 数组中，例如 s = 00111011s=00111011，可以得到这样的 \textit{counts}counts 数组：\textit{counts} = \{2, 3, 1, 2\}counts={2,3,1,2}。
>
>这里 \textit{counts}counts 数组中两个相邻的数一定代表的是两种不同的字符。假设 \textit{counts}counts 数组中两个相邻的数字为 uu 或者 vv，它们对应着 uu 个 00 和 vv 个 11，或者 uu 个 11 和 vv 个 00。它们能组成的满足条件的子串数目为 \min \{ u, v \}min{u,v}，即一对相邻的数字对答案的贡献。
>
>我们只要遍历所有相邻的数对，求它们的贡献总和，即可得到答案。这个实现的时间复杂度和空间复杂度都是 O(n)*O*(*n*)。
>
>对于某一个位置 ii，其实我们只关心 i - 1i−1 位置的 \textit{counts}counts 值是多少，所以可以用一个 \textit{last}last 变量来维护当前位置的前一个位置，这样可以省去一个 \textit{counts}counts 数组的空间。
>
>- 时间复杂度：O(n)*O*(*n*)。
>- 空间复杂度：O(1)*O*(1)。
>
>

```cpp
//个人按照官方思路写：
//bugfree , quick;
class Solution {
public:
    int countBinarySubstrings(string s) {
        int result { 0 };
        int last_count { 0 };
        int this_count { 0 };
        char last;
        if(s[0] == '0')
        {
            last = '1';
        }
        else
        {
            last = '0';
        }
        for (int i { 0 }; i < s.size();)
        {
            this_count = 0;
            while(i < s.size() && s[i] != last  )
            {
                i++;
                this_count++;
            }
            if(last_count > this_count)
            {
                result += this_count;
            }
            else
            {
                result += last_count;
            }
            last_count = this_count;
            last = s[i-1];
        }
        return result;
    }
};

//官方答案
class Solution {
public:
    int countBinarySubstrings(string s) {
        int ptr = 0, n = s.size(), last = 0, ans = 0;
        while (ptr < n) {
            char c = s[ptr];
            int count = 0;
            while (ptr < n && s[ptr] == c) {
                ++ptr;
                ++count;
            }
            ans += min(count, last);
            last = count;
        }
        return ans;
    }
};

其实并不需要区分是0还是1，只要区分是否连续就行了；
```



















































