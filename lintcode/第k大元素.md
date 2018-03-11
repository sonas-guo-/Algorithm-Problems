# 题目链接
第k大元素[http://www.lintcode.com/zh-cn/problem/kth-largest-element/#]

# 思路
用快速排序的思想，先根据一个pivot把数组进行划分，然后比较pivot的下标，如果就是第k个话，则直接返回，若p - left + 1< k，也就是说第k大的数在pivot右边，则在右边递归搜索，否则在左边递归搜素。

# 代码
	class Solution {
	public:
		/*
		* @param n: An integer
		* @param nums: An array
		* @return: the Kth largest element
		*/
		int partition(vector<int> &nums, int left, int right)
		{
			int l = left, r = right, key = nums[left];
			while (l<r)
			{
				while (nums[r] <= key && l<r) r--;
				swap(nums[l], nums[r]);
				while (nums[l] >= key && l<r) l++;
				swap(nums[l], nums[r]);
			}
			return l;
		}
		int findKthLargestElement(vector<int> &nums, int k, int left, int right)
		{
			int p = partition(nums, left, right);
			if (p - left + 1 == k)
				return nums[p];
			else if (p - left + 1<k)
				return findKthLargestElement(nums, k - p + left-1, p+1, right);
			else
				return findKthLargestElement(nums, k, left, p-1);
		}
		int kthLargestElement(int n, vector<int> &nums) {
			// write your code here
			return findKthLargestElement(nums, n, 0, nums.size() - 1);
		}
	};