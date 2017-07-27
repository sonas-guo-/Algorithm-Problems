# 题目链接
MSFT[http://hihocoder.com/problemset/problem/1535]
# 思路
将S串通过交换两个字符转换为T串，因为MSFT四个字符最多出现两次，那么范围很小，可以考虑直接搜索。对于T串上的每个位置，都有一个最终的字符，那么对于S串上响应位置的字符看是否符合要求，不符合要求的话则从后面的字符串中挑选指定的字符进行枚举。
# 代码
	string a, b;
	void dfs(int p, int step, int &ans)
	{
		if (p >= a.length())
		{
			ans = min(ans, step);
		}
		else
		{
			if (a[p] == b[p])
				dfs(p + 1, step, ans);
			else
			{
				for (int i = p + 1; i < a.length(); i++)
				{
					if (a[i] == b[p])
					{
						swap(a[i], a[p]);
						dfs(p + 1, step + 1, ans);
						swap(a[i], a[p]);
					}
				}
			}
		}

	}
	int main() {

	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		int T;
		
		cin >> T;
		for (size_t t = 0; t < T; t++)
		{
			cin >> a >> b;
			int ans = INT_MAX;
			dfs(0, 0,ans);
			cout << ans << endl;
		}
		return 0;
	}