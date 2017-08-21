# 题目链接
H国的身份证号码II[http://hihocoder.com/problemset/problem/1560]

# 思路
动态规划，f[i][j]表示从前一位的i转移到当前位的j所需要的方案数，由于N的范围很大，不能直接开数组dp，观察转移矩阵发现除了初始状态之外，其他从第i-1位到第i位的转移矩阵是一样的，所以可以用快速幂将后面N-1次的转移矩阵先乘起来，最后再跟初始状态相乘。

# 代码
	#define MOD 1000000007
	#define MAXN 100005
	#define MAXK 10
	struct Matrix
	{
		Matrix()
		{
			memset(m_data, 0, sizeof(m_data));
			for (size_t i = 0; i < MAXK; i++)
				m_data[i][i] = 1;
		};
		void clear() { memset(m_data, 0, sizeof(m_data)); };
		int64_t * const operator[](const int i)
		{
			return m_data[i];
		}
		friend Matrix operator *(Matrix &, Matrix &);
		int64_t m_data[MAXK][MAXK];
	};
	Matrix operator *(Matrix &a, Matrix &b)
	{
		Matrix res;
		res.clear();
		for (size_t i = 0; i <MAXK ; i++)
		{
			for (size_t j = 0; j < MAXK; j++)
			{
				for (size_t k = 0; k < MAXK; k++)
				{
					res[i][j] = (res[i][j] + a[i][k] * b[k][j] % MOD) % MOD;
				}
			}
		}
		return res;
	}
	Matrix power(Matrix a, int64_t b)
	{
		Matrix res;
		while (b)
		{
			if (b & 1) res = res*a;
			a = a*a;
			b >>= 1;
		}
		return res;
	}
	int main() {

	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);	
	#endif
		int64_t N,K;
		string s;
		cin >> N >> K;
		Matrix start,trans;
		for (size_t i = 0; i <= 9; i++)
		{
			for (size_t j = 0; j <=9; j++)
			{
				start[i][j] = 0;
				if (i <= K&&j <= K&&i*j <= K)
					trans[i][j] = 1;
				else
					trans[i][j] = 0;
			}
			if (i>0&&i<=K) start[0][i] = 1;
		}
		trans = power(trans, N - 1);
		trans = start*trans;
		int64_t ans = 0;
		for (size_t i = 0; i <=9; i++)
		{
			ans = (ans + trans[0][i]) % MOD;
		}
		cout << ans << endl;
		return 0;
	}