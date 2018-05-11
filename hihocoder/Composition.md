# 题目链接
Composition[http://hihocoder.com/problemset/problem/1400]

# 思路
dp问题，原先是求最少删除的字符，可以求最多剩多少个字符。然后f[i]表示以第i个字符结尾构成的合法字符串最多会剩多少个字符，f[i]=max(f[j]+1,f[i])，j为枚举i之前最近的a-z的字符的位置。

# 代码
	#define MAXN 100005
	bool adjacent[26][26];
	int f[MAXN];
	int pos[26];
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif 
		int n;
		string str;
		char a, b,ci;
		cin >> n;
		cin >> str;
		cin >> n;
		fill(adjacent[0], adjacent[0] + 26 * 26, true);
		fill(f, f + MAXN, 0);
		fill(pos, pos + 26, -1);
		for (size_t i = 0; i < n; i++)
		{
			cin >> a >> b;
			adjacent[a - 'a'][b - 'a'] = false;
			adjacent[b - 'a'][a - 'a'] = false;
		}
		int ans = 0;
		for (int i = 0; i < str.length(); i++)
		{
			ci = str[i] - 'a';
			f[i] = 1;
			for (char c = 0; c < 26; c++)
			{
				if (i)
				{
					if (adjacent[c][ci])
					{
						if (pos[c]>=0)
							f[i] = max(f[i], f[pos[c]] + 1);
					}
				}
			}
			ans = max(ans, f[i]);
			pos[ci] = max(pos[ci], i);
		}
		cout << str.length()-ans << endl;
		return 0;
	}
