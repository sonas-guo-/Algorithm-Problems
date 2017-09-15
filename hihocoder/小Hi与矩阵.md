# 题目链接
小Hi与矩阵[http://hihocoder.com/problemset/problem/1573]

# 思路
距离某个点的曼哈顿距离为k的范围可以看出来是一个旋转45的正方形，这个时候不容易进行操作，那么可以将图形进行旋转，构造一个被放正的正方形，放正之后就可以进行dp了。如
1 2 1
2 1 2
1 2 1
变换之后就变成了
0 0 1 0 0
0 2 0 2 0
1 0 1 0 1
0 2 0 2 0
0 0 1 0 0
变换的公式为(i,j)->(i + j + 1,N - i + j)，从原先(0,N)的坐标系变换到(0,2*N-1]坐标系。这样就可以进行dp操作了，令f[i][j][k]为前i行前j列k的数量，那么进行矩阵分解即可计算。

# 代码
	using namespace std;
	#define MAXN 500
	#define MAXM 25
	int f[MAXN][MAXN][MAXM];
	int matrix[MAXN][MAXN];
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		std::ios::sync_with_stdio(false);
		std::cin.tie(0);
		int M,N,a,b,c;
		memset(f, 0, sizeof(f));
		cin >> N;
		for (int i = 0; i < N; i++)
			for (int j = 0; j < N; j++)
				cin >> matrix[i + j + 1][N - i + j];
		for (int i = 1; i <= 2*N-1; i++)
		{
			for (int j = 1; j <= 2*N-1; j++)
			{
				for (int k = 1; k <= 20; k++)
				{
					f[i][j][k] = f[i - 1][j][k] + f[i][j - 1][k] - f[i - 1][j - 1][k];
				}
				f[i][j][matrix[i][j]]++;
			}
		}
		cin >> M;
		int cx, cy,ans=0,ax,ay,bx,by;
		for (int i = 0; i < M; i++)
		{
			cin >> a >> b >> c;
			cx = a - 1 + b;
			cy = N - a + b;
			ans = 0;
			ax = max(cx - c - 1,0);
			ay = max(cy - c - 1, 0);
			bx = min(cx + c, 2 * N - 1);
			by = min(cy + c, 2 * N - 1);
			for (int k = 1; k <= 20; k++)
			{
				if (c%k==0)
					ans += f[bx][by][k] - f[ax][by][k] - f[bx][ay][k] + f[ax][ay][k];
			}
			cout << ans << endl;
		}
		return 0;
	}