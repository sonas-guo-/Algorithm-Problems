# 题目链接
Email Merge[http://hihocoder.com/problemset/problem/1335]

# 思路
并查集，对于每个用户名，找到它的邮箱对应的之前的用户名，然后用并查集合并， 最后扫描遍并查集把结果输出来就行。

# 代码
	using namespace std;
	#define MAXN 10005
	string names[MAXN];
	class UnionFindSet
	{
	public:
		UnionFindSet(int size) {
			sets.resize(size);
			for (size_t i = 0; i < size; i++) sets[i] = i;
		};
		size_t size() { return sets.size(); };
		void setUnion(int a, int b)//合并集合
		{
			int s1 = getSet(a), s2 = getSet(b);
			if (s1 < s2)
				sets[s2] = s1;
			else
				sets[s1] = s2;
		};
		int getSet(int a) //查询集合，在查询的过程修改每个元素的集合为最终集合
		{
			int father;
			if (a != sets[a])
			{
				father = getSet(sets[a]);
				sets[a] = father;
				return father;
			}
			return a;
		};
	private:
		vector<int> sets;
	};
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		int N,M;
		string username, email;
		cin >> N;
		UnionFindSet ufset(N);
		unordered_map<string, set<int>> mail2names;
		for (size_t i = 0; i < N; i++)
		{
			cin >> names[i];
			cin >> M;
			int target;
			for (size_t j = 0; j < M; j++)
			{
				cin >> email;
				for (auto k = mail2names[email].begin(); k != mail2names[email].end(); k++)
					ufset.setUnion(i, *k);			
				mail2names[email].insert(i);
			}
		}
		unordered_map<int, set<int>> res;
		for (size_t i = 0; i < N; i++)
		{
			int s = ufset.getSet(i);
			res[s].insert(i);
		}
		for (auto i = res.begin(); i != res.end(); i++)
		{
			for (auto j = (*i).second.begin(); j != (*i).second.end(); j++)
				j == (*i).second.begin() ? cout << names[*j] : cout <<' '<< names[*j];
			cout << endl;
		}
		return 0;
	}
