# 题目链接
Matrix Sum[http://hihocoder.com/problemset/problem/1336]

# 思路
二维树状数组，一维的扩展。参考[http://hihocoder.com/discuss/question/4956]

# 代码
	using namespace std;
	#define MAXN 1005
	#define MOD 1000000007
	int64_t SZ[MAXN][MAXN];
	int N, M;
	int lowbit(int x)
	{
		return x&(-x);
	}
	void add(int x, int y, int v)
	{
		for (int i = x; i <=N; i+=lowbit(i))
			for (int j = y; j <= N; j += lowbit(j))
			{
				SZ[i][j] += v;
			}
	}
	int64_t sum(int x, int y)
	{
		int64_t res = 0;
		for (int i = x; i > 0; i -= lowbit(i))
			for (int j = y; j > 0; j -= lowbit(j))
				res += SZ[i][j];
		return res;
	}
	int64_t search(int x1, int y1, int x2, int y2)
	{
		return sum(x2, y2) + sum(x1 - 1, y1 - 1) - sum(x2, y1 - 1) - sum(x1 - 1, y2);
	}
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		int x, y, x1, y1, v;
		memset(SZ, 0, sizeof(SZ));
		string cmd;
		std::ios::sync_with_stdio(false);
		cin >> N >> M;
		for (size_t i = 0; i < M; i++)
		{
			cin >> cmd;
			if (cmd == "Add")
			{
				cin >> x >> y >> v;
				add(x+1, y+1, v);
			}
			if (cmd == "Sum")
			{
				cin >> x >> y >> x1 >> y1;
				cout << (search(x+1,y+1,x1+1,y1+1)+MOD)%MOD << endl;
			}
		}
		return 0;
	}