# 题目链接
小Ho的防护盾[http://hihocoder.com/problemset/problem/1357]

# 思路
因为T越大防护盾越不容易被攻破，T越小越容易，所以是个明显的二分问题，直接模拟即可。另外题目没说清楚炮弹的顺序是否可以调换，是个小bug。

# 代码
	#define MAXN 100005
	int64_t A[MAXN];
	int64_t N, M, K;
	bool check(int64_t T)
	{
	    int64_t m = M, k = K;
	    for (size_t i = 0; i < N; i++)
	    {
	        m = min(m + T, M);
	        if (A[i] >= m)
	        {
	            k--;
	            m = M;
	            if (!k)
	                return true;
	            continue;
	        }
	        else
	        {
	            m -= A[i];
	        }
	    }
	    return false;
	}
	int main()
	{
	#ifdef DEBUG
	    ifstream cin("in.txt");
	    ofstream cout("out.txt");
	    /*freopen("in.txt", "r", stdin);
	    freopen("out.txt", "w", stdout);*/
	#endif
	    cin >> N >> M >> K;
	    for (size_t i = 0; i < N; i++)
	        cin >> A[i];
	    int64_t l = 1, r = M, m;
	    while (l<r)
	    {
	        m = l + (r - l) / 2;
	        if (check(m))
	            l = m + 1;
	        else
	            r = m;
	    }
	    if (check(l))
	        cout << -1 << endl;
	    else
	        cout << l << endl;
	    return 0;
	}