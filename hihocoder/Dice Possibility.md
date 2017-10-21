# 题目链接
Dice Possibility[http://hihocoder.com/problemset/problem/1339]

# 思路
动态规划。f[i][j]表示i个骰子和为j的概率，f[i][j]=\sum f[i-1][j-k]\*1/6, f[0][0]=1。

# 代码
	double f[MAXN][MAXN];
	int N, M;
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		memset(f, 0, sizeof(f));
		std::ios::sync_with_stdio(false);
		cin >> N >> M;
		double p = 1.0 / 6;
		f[0][0] = 1;
		for (int i = 1; i <= N; i++)
		{
			for (int j = 0; j <= M; j++)
			{
				for (int k = 1; k <= 6; k++)
				{
					if (j-k>=0)
						f[i][j]+= f[i - 1][j - k] * p;
				}
			}
		}
		cout << fixed<<setprecision(2)<< f[N][M]*100 << endl;
		return 0;
	}