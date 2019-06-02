首先想到的是
## 使用双指针的思路
```c++
//4ms,9.6MB
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        //思路一:排序后双指针扫描，O(nlogn)
        vector<int> res;
        vector<int> nums_copy = nums;
        sort(nums.begin(),nums.end());
        for(int i=0,j=nums.size()-1;i<=j;){
            if(nums[i]+nums[j]==target){
                for(int k=0;k<nums.size();k++){
                    if(nums_copy[k]==nums[i] || nums_copy[k]==nums[j]){
                        res.push_back(k);                   
                    }
                }
                break;
            }else if(nums[i]+nums[j]<target){ i++; }
            else{ j--;}
        }
        return res;
    }
};
```
## 然后想到使用Hashmap的思路
```c++
//8ms,10MB
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        unordered_map<int,int> hash;
        for(int i=0;i<nums.size();i++){
            int complement = target-nums[i];
            if(hash.find(complement) != hash.end()){
                res.push_back(i);
                res.push_back(hash[complement]);
                break;
            }
            hash[nums[i]] = i;
        }
        
        return res;
    }
};
```



这里发现hashmap比two-pointer的要慢，于是学习到了

## 更快的Hashmap版本

```c++
//0ms,10.5MB
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> index;
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            index[nums[i]] = i;
        }
        
        for (int i = 0; i < nums.size(); ++i) {
            if (index.find(target - nums[i]) != index.end() 
               && index[target - nums[i]] != i) {
                res.push_back(i);
                res.push_back(index[target-nums[i]]);
                break;
            }
        }
        return res;
    }
};
```

为什么快了呢，与我写的不同，奇怪。。。。没搞懂还

## Python版本

```python
//28ms,14.1MB 
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
    	hash = {}
    	for i,num in enumerate(nums):
    		if target - num in hash：
    			return hash[target - num], i
    		hash[num] = i
```



