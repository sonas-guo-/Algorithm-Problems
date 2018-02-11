# 题目链接
小p的强力值[http://hihocoder.com/contest/hiho189/problem/1]

# 思路
对题目中给的公式取log，转换为求和的形式，发现属性的收益是独立的，与其他属性无关，而且同一属性的收益随着点数增加会衰减，所以可以用贪心策略来每次选加点收益最大的属性。

# 代码
	#define MAXN 100005
	#define MAXK 15
	int A[MAXK], B[MAXK];
	int N, K;
	int main()
	{
	#ifdef DEBUG
	    ifstream cin("in.txt");
	    ofstream cout("out.txt");
	    /*freopen("in.txt", "r", stdin);
	    freopen("out.txt", "w", stdout);*/
	#endif
	    cin >> N >> K;
	    for (size_t i = 0; i < K; i++)
	        cin >> A[i];
	    for (size_t i = 0; i < K; i++)
	        cin >> B[i];
	    for (size_t i = 0; i < N; i++)
	    {
	        double delta = 0,tdelta;
	        int index=0;
	        for (size_t j = 0; j < K; j++)
	        {
	            tdelta = 1.0 / B[j] * (log(A[j]+ 1) - log(A[j]));
	            if (tdelta>delta)
	            {
	                index = j;
	                delta = tdelta;
	            }
	        }
	        A[index]++;
	    }
	    double ans = 1;
	    for (size_t i = 0; i < K; i++)
	    {
	        ans *= pow(A[i], 1.0 / B[i]);
	    }
	    cout <<fixed<<setprecision(10)<< ans<<endl;
	    return 0;
	}