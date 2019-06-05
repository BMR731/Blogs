这道题我先搞明白学习一种思路吧。。

在统计学上，中位数被用作将一个集合划分为两个等长的子集，一个子集总是比另一个子集要更大，这里有两个数组，我们把它们划分为左、右两个部分。

        // left_part                |        right_part
        // A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
        // B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
我们需要找到i，j，他们满足

- A[i-1] < B[j]  &&  B[j-1] <A[i]                   ===>Median=(max(left_part)+min(right_part))/2.
- len(left_part) == len(right_part)            ===> i + j == m-i+n-j => i + j = (m+n) / 2 

```c++
//32ms,9.8MB
class Solution {
public:
	double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
		int m = int(nums1.size());
		int n = int(nums2.size());
		//for all ,we assume m <= n
		if (m > n) return findMedianSortedArrays(nums2, nums1);
		int index_min = 0;
		int index_max = m;
		int left_part_max, right_part_min;
		int i = (index_min + index_max) / 2;
		int j = (m + n + 1) / 2 - i;
		//binary search i
		while (index_min <= index_max) {
			i = (index_max + index_min) / 2;
			j = (m + n + 1) / 2 - i;
			if (i > 0 && nums1[i - 1] > nums2[j]) {
				//i is too large
				index_max = i - 1;
			}
			else if (i < m && nums2[j - 1] > nums1[i]) {
				//i is too small
				index_min = i + 1;
			}
			else break;//i is perfect
		}
		if (i == 0) left_part_max = nums2[j - 1];
		else if (j == 0) left_part_max = nums1[i - 1];
		else left_part_max = max(nums1[i - 1], nums2[j - 1]);
        if ((m + n) % 2 == 1) return left_part_max;

		if (i == m) right_part_min = nums2[j];
		else if (j == n) right_part_min = nums1[i];
		else right_part_min = min(nums2[j], nums1[i]);

		return (left_part_max + right_part_min) / 2.0;
	}
};
```

所以这种解法的思路就是理解中位数的意义，然后列出等式，通过对i进行二分查找，只要i一定，根据等式j就定了，那么中位数就可以通过性质找到，只能说，我没有对中位数理解的那么深刻。。。这道题需要多看几次，这可以看成一道二分查找的题，只不过找的是一个下标而已。

