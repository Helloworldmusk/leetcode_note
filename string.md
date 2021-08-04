

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









































































