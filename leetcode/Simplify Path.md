# 题目链接
Simplify Path[https://leetcode.com/problems/simplify-path/description/]

# 思路
用栈来进行模拟，遇到"."不变，遇到".."则弹出栈顶，否则压栈。

# 代码
	using namespace std;
	class Solution {
	public:
	    string simplifyPath(string path) {
	        stringstream ss(path);
	        string tmp;
	        stack<string> st;
	        while(getline(ss,tmp,'/'))
	        {
	            if (tmp=="."||tmp=="") continue;
	            else if (tmp=="..") 
	            {
	                if (!st.empty())
	                    st.pop();
	            }
	            else
	                st.push(tmp);
	        }
	        string ans;
	        while(!st.empty())
	        {
	            ans="/"+st.top()+ans;
	            st.pop();
	        }
	        if (ans=="") ans="/";
	        return ans;
	    }
	};