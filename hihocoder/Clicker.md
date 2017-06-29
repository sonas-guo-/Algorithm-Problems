# 题目链接
Clicker[http://hihocoder.com/problemset/problem/1091]
# 思路
完全背包问题[http://hihocoder.com/problemset/problem/1043]，动态转移的过程中可以省略一维，对于这道题目来说round函数也是一个坑，因为每升一级都会做一个下取整，对于初始值为1的话，每升一级消耗均为1，那么最高可能为20000级。
# 代码
	#define MAXN 35
	#define MAXL 150
	#define MAXM 20005
	int Ai[MAXN], Bi[MAXN];
	int sum[MAXN][MAXM];
	int C[MAXN][MAXM];
	int f[MAXN][MAXM];
	int main() {
	#ifdef DEBUG
	    ifstream cin("in.txt");
		ofstream cout("out.txt");
		/*freopen("in.txt", "r", stdin);
		freopen("out.txt", "w", stdout);*/
	#endif
		int N, M;
		cin >> N >> M;
		for (size_t i = 1; i <= N; i++)
		{
			cin >> Ai[i] >> Bi[i];
		}

		memset(f, 0, sizeof(f));
		memset(C, 0, sizeof(C));
		memset(sum, 0, sizeof(sum));
		for (int i = 1; i <= N; i++)
		{
			for (int j = 1; j <= M; j++)
			{
				if (j == 1)
				{
					C[i][j] = Bi[i];
				}
				else
				{
					C[i][j] = floor(C[i][j - 1] *1.07) ;
				}
				sum[i][j] = sum[i][j - 1] +C[i][j];
			}
		}
		int ans = 0;
		for (int i = 1; i <= N; i++)
		{
			for (int j = 1; j <= M; j++)
			{
				for (int k = 0; k <= M; k++)
				{
					if (j >= sum[i][k])
						f[i][j] = max(f[i][j], f[i - 1][j - sum[i][k]] + Ai[i] * k);
					else
					{
						f[i][j] = max(f[i][j], f[i - 1][j]);
						break;
					}
				}
				ans = max(ans, f[i][j]);
			}
		}
		cout << ans << endl;
		return 0;
	}