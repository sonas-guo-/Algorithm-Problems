# 题目链接
Array Partition[http://hihocoder.com/problemset/problem/1534]
# 思路
将数组按要求分隔成三段，首先想到的是枚举两个分割点，但是枚举的话复杂度是O(n^2)，所以考虑怎么能优化一维。因为|S1-S2|<=1，所以考虑在这个地方1做处理，令S1+S2=A，那么|2S1-A|<=1，可以解出来S1>=(A-1)/2且S1<=(A+1)/2，那么当知道A的值的时候，就可以算出来S1的取值范围，S1最多会有两个值，而A恰好是第二个分割点，所以只需要枚举一个分割点即可。
# 代码
	#define MAXN 100005
	int64_t Ai[MAXN],sum[MAXN];
	int main() {
		#ifdef DEBUG
			ifstream cin("in.txt");
			ofstream cout("out.txt");
			//freopen("in.txt", "r", stdin);
			//freopen("out.txt", "w", stdout);
		#endif
			int N;
			cin >> N;
			for (size_t i = 0; i < N; i++)
			{
				cin >> Ai[i];
				if (i)
					sum[i] = sum[i - 1] + Ai[i];
				else
					sum[i] = Ai[i];
			}
			map<int64_t, int64_t> arised;
			int64_t S = sum[N - 1],S3,S2,S1;
			int64_t ans = 0;

			for (size_t i = 0; i < N-1; i++)
			{
				S3 = S - sum[i];
				for (S1 = ceil((sum[i] - 1) / 2); S1 <= floor((sum[i] + 1) / 2); S1++)
				{
					S2 = sum[i] - S1;
					if (abs(S1 - S2) <= 1)
					{				
						if (arised.find(S1) != arised.end())
						{
							if (abs(S3 - S2) <= 1 && abs(S3 - S1) <= 1)
							{
								ans += arised[S1];
							}
						}
					}
				}
				arised[sum[i]]++;
			}
			cout << ans << endl;
			return 0;
	}