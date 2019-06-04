开始先用暴力法跑通

## 暴力法

```c++
//超时
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int length_max=0;
		for (int i = 0; i < s.length();i++) {
			for (int j = i+1; j <= s.length(); j++) {
				if (isStringUnique(s.substr(i, j - i))) {
					if (j - i > length_max) length_max = j - i;
				}
			}
		}
		return length_max;		
	}

	bool isStringUnique(string s) {
		set<char> bag;
		for (int i = 0; i < s.length; i++) {
			if (bag.count(s[i]) == 0) bag.insert(s[i]);
			else return false;
		}
		return true;
	}
};
```

暴力法的时间复杂度O(n3),然后学习Solution中滑动窗口的用法...

## 滑动窗口法

因为本题要找一个满足特定性质的连续子串，所以我们可以维护一个满足性质的滑动窗口，（对于本题而言，该性质就是子串内不能有相同的字符）。所以如果我们可以选择合适的数据结构（set），并且能够通过简单的操作维持这种性质（左边界右移时删除，有边界右移时添加），我们就可以解决问题。了解滑动窗口什么时候用，怎么用，尤其在字符串和数组问题中，可以多学习。

```c++
//44ms,15.7MB
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int len = s.length();
		set<char> bag;
		int i = 0, j = 0, length_max=0;
		while (i < len && j < len) {
			if (bag.count(s[j]) == 0) {
				bag.insert(s[j]);
				j++;
				if (length_max < j - i) length_max = j - i;
			}
			else {
				bag.erase(s[i]);
				i++;
			}
		}
		return length_max;
	}
};
```

做好时间复杂度的分析有时候很关键，给我们提供了进一步优化的思路，在上述滑动窗口版本中，每个元素至多可以被访问两次，如'bbbbb'这样的字符串，复杂度O(2n),对于这样的最坏情况，有没有思路可以改进呢？又学到了改进的思路。

## 优化的滑动窗口

未优化之前，左边界只能一步一步的向右侧移动，如果右边界在遇到重复的字符后，有一种数据结构能够直接告诉我们这个字符在哪里，那么左边界就可以迈出一个大步，具体的原因在于如果 是s[j]在[i,j)内又一个重复下标为j',那么我们可以直接将下次的滑动窗口变为[j'+1,j+1)，但更细致的注意，其实新的左边界L= max(i,j'+1)，否则在遇到 'baab'中第二个b时，会有一些小错误。就这样从而算法得到进一步的优化。这种映射的数据结构，就是map。

```c++
//16ms,10.8MB
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int len = s.length();
		unordered_map<char, int> slide_window;
		int i = 0, j = 0, length_max=0;
		while (i < len && j < len) {
			if (slide_window.find(s[j]) != slide_window.end()) {
				i = std::max(slide_window[s[j]] + 1,i);
			}
			slide_window[s[j]] = j;
			length_max = std::max(length_max, j - i + 1);
			j++;	
		}
		return length_max;
	}
};
```

事实上，存储映射不需要用map类的数据结构，简单的vector就可以了，学习了下最快的代码

## 性能优异的滑动窗口

```C++
//4ms,10.7MB
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int border_left = 0;
		int length_max = 0;
		vector<int> slide_window(256, -1);
		for (int i = 0; i < s.size(); i++) {
			border_left = max(border_left, slide_window[s[i]] + 1);
			slide_window[s[i]] = i;
			length_max = std::max(length_max, i - border_left + 1);
		}
		return length_max;
	}
};
```

换用vector后，将判断的逻辑转化为了查表，可以看出性能一下子快了不少，最后过一下python版本的代码

## python版本

```python
#56ms,13.3MB
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dicts = {}
        lenght_max  = border_left = 0
        for i,value in enumerate(s):
            if value in dicts:
                border_left = max(border_left, dicts[value]+1)
            lenght_max = max(lenght_max, i-border_left+1)
            dicts[value] = i
        return lenght_max
```

## 总结

本题从暴力的超时,44ms,16ms,4ms一步步优化过来，不但有算法上的分析和改进，也考虑了性能的优化和提升。

我认为这是一道比较好的滑动窗口的例题，也比较了滑动窗口思想结合不同数据结构下的实现，好题！