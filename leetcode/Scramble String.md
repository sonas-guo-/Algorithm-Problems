# 题目链接
Scramble String[https://leetcode.com/problems/scramble-string/description/]

# 思路
动态规划思想，每次枚举分割点，递归判断子串，但是需要优化剪枝，当遇到子串不可能匹配的时候提前退出。

# 代码
	bool check(string a,string b)
	{
	    vector<int> counts(26,0);
	    for (size_t i=0;i<a.length();i++)
	    {
	        counts[a[i]-'a']++;
	    }
	    for (size_t i=0;i<b.length();i++)
	    {
	        counts[b[i]-'a']--;
	        if (counts[b[i]-'a']<0)
	            return false;
	    }
	    return true;
	}
	class Solution {
	public:
		bool isScramble(string s1, string s2) {
			if (s1 == s2)
				return true;
	        if (!check(s1,s2))
	            return false;
			for (size_t i = 1; i < s1.length(); i++)
			{
				int l = i, r = s1.length() - i;
				if (isScramble(s1.substr(0, l), s2.substr(0, l)) && isScramble(s1.substr(l, r), s2.substr(l, r)))
					return true;
				if (isScramble(s1.substr(0, l), s2.substr(r, l)) && isScramble(s1.substr(l, r), s2.substr(0, r)))
					return true;
			}
			return false;
		}
	};