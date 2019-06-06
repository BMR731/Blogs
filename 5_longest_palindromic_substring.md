先用暴力法跑通

## 暴力法

```c++
//超时
class Solution {
public:
	string longestPalindrome(string s) {
		int len = s.length();
		int length_max = 0;
		int target_str_begin=0;
		for (int i = 0; i < len ; i++) {
			for (int j = i; j < len; j++) {
				if (isPalindromic(s,i,j)) {
					if (length_max < j - i + 1) {
						length_max = j - i + 1;
						target_str_begin = i;
					}
				}
			}
		}
		return s.substr(target_str_begin,length_max);
	}
private:
	bool isPalindromic(string s,int left,int right) {
		while (left <= right) {
			if (s[left] == s[right]) { left++; right--; }
			else { return false; }
		}
		return true;
	}
};
```

显然，时间复杂度O(n3),尝试下一步优化思路，判断是否是回文子串的这一功能，好像不需要每次花费O(n)，每次似乎都做了一些 重复的工作....于是学到了用动态规划的优化方法

## 动态规划

```c++
class Solution {
public:
	string longestPalindrome(string s) {
		int len = s.length();
		int length_max = 0;
		int target_str_begin=0;
		//init for dp
		vector<vector<bool>> dp(len, vector<bool>(len, false));
		for (int i = 0; i < len; i++) dp[i][i] = true;
		for (int i = 0; i < len - 1; i++) dp[i][i + 1] = (s[i] == s[i + 1]);

		for (int substr_length = 1; substr_length <= len; substr_length++) {
			for (int i = 0; i < len - substr_length + 1; i++) {
				if (isPalindromic(s, i, i + substr_length - 1, dp) && 
					length_max < substr_length) {
					length_max = substr_length;
					target_str_begin = i;
				}
			}
		}
		return s.substr(target_str_begin,length_max);
	}
private:
	bool isPalindromic(string s,int left,int right,vector<vector<bool> >& dp) {
		if (left == right || left + 1 == right) return dp[left][right];
		dp[left][right] = dp[left + 1][right - 1] && s[left] == s[right];
		return dp[left][right];
	}
};

```

动态规划法占用O(n2)的空间，```dp[i][j]```表示s[i,j]的子串是否是回文串，但是现在的时间复杂度降到了O(n2)，注意在使用时，我们先对dp进行了初始化，在循环结构上也有一些改动，利用子串长度作为外层循环，为什么要进行这样的改动呢，因为对dp的递推公式可知，每次要使用```dp[i+1][j-1]```的数据，为了确保数据在使用前已经被计算过，需要留意下遍历的方式，使之每次遍历正好可以用到需要的数据。同时也要注意限制，如```i<len -substr_lenght +1```，确保右边界不会出界等。

前几个月是acc了的，1492ms，但是这次程序在运行时超了内存限制，没有acc...，而且空间复杂度是有点大，有没有稍微省点空间的动态规划思想方法呢，该题还有一个中心扩展法和一个Manachers's Algorithm，都很难抽取出通用的思想，选择中心扩展法学习一下

## 中心扩展法

```c++
//36ms
class Solution {
public:
	string longestPalindrome(string s) {
		int len_str = s.length();
		int target_str_begin = 0, target_str_end = 0;
		for (int i = 0; i < len_str; i++) {
			int len_expand1 = expand_around_center(s, i, i);
			int len_expand2 = expand_around_center(s, i, i + 1);
			int len_expand_max = std::max(len_expand1, len_expand2);
			if (len_expand_max > target_str_end - target_str_begin) {
				target_str_begin = i - (len_expand_max - 1) / 2;
				target_str_end = i + len_expand_max / 2;
			}
		}
		return s.substr(target_str_begin,target_str_end-target_str_begin+1);
	}
private:
	int expand_around_center(string s, int left, int right) {
		int l = left, r = right;
		while (l >= 0 && r < s.length() && s[l] == s[r]) {
			l--; r++;
		}
		return r - l - 2 + 1;
	}
};
```

这种方法思路就比较清晰，但是过过眼瘾就好。。感觉一下子很难想到，时间O(n2),空间O(1)。

最后学一个pythonic

## Python版本

```python
def longestPalindrome(self, s):
    res = ""
    for i in xrange(len(s)):
        # odd case, like "aba"
        tmp = self.helper(s, i, i)
        if len(tmp) > len(res):
            res = tmp
        # even case, like "abba"
        tmp = self.helper(s, i, i+1)
        if len(tmp) > len(res):
            res = tmp
    return res
 
# get the longest palindrome, l, r are the middle indexes   
# from inner to outer
def helper(self, s, l, r):
    while l >= 0 and r < len(s) and s[l] == s[r]:
        l -= 1; r += 1
    return s[l+1:r]
```

这道题主要对于我而言，学到的是简单的动态规划怎么找，怎么写。还是很好的题。

