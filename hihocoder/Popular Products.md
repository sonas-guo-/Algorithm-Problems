# 题目链接
Popular Products[http://hihocoder.com/problemset/problem/1351]

# 思路
对所有的purchase list去取交集即可。

# 代码
	using namespace std;
	#define MAXN 300
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		std::ios::sync_with_stdio(false);
		int N,M;
		string a, b, c;
		set<string> ans;
		cin >> N;
		for (size_t i = 0; i < N; i++)
		{
			set<string> S,tmp;
			cin >> M;
			for (size_t j = 0; j < M; j++)
			{
				cin >> a >> b >> c;
				S.insert(a + c);
			}
			if (i)
				set_intersection(S.begin(), S.end(), ans.begin(), ans.end(), inserter(tmp,tmp.begin()));
			else
				tmp = S;
			ans = tmp;
		}
		for (auto i = ans.begin(); i !=ans.end(); i++)
		{
			cout << (*i).substr(0,9) << endl;
		}
		return 0;
	}