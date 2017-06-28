# 题目链接
Browser Caching[http://hihocoder.com/problemset/problem/1086]
# 思路
用list实现最近最少使用的缓存，每次满的时候把list的头部去掉即可，然后用一个set来记录在缓存中的url的迭代器，这样当遇到已经在缓存中的url，可以在O(lgn)的时间内查到，将它移动到尾部。
# 代码
	struct Node
	{
		string url;
		list<string>::iterator iter;
		Node(string u, list<string>::iterator i) :url(u),iter(i){};
		Node(string u) :url(u){};
		bool operator<(const Node &a) const
		{
			return url < a.url;
		}
	};
	list<string> cache;
	set<Node> S;
	int main() {
	#ifdef DEBUG
	    /*ifstream cin("in.txt");
		ofstream cout("out.txt");*/
		freopen("in.txt", "r", stdin);
		freopen("out.txt", "w", stdout);
	#endif
		int N, M;
		string url;
		char curl[200];
		scanf("%d%d\n", &N, &M);
		//cin >> N >> M;
		for (size_t i = 0; i < N; i++)
		{
			//cin >> url;
			scanf("%s", curl);
			url = curl;
			auto iter = S.find(Node(url));
			if (iter == S.end())
			{
				printf("Internet\n");
				//cout << "Internet" << endl;
				if (cache.size() == M)
				{
					string del_url = cache.front();
					S.erase(Node(del_url));
					cache.pop_front();
				}
				cache.push_back(url);
				auto iter1 = cache.end();
				iter1--;
				S.insert(Node(url,iter1));
			}
			else
			{
				printf("Cache\n");
				//cout << "Cache" << endl;
				auto iter1 = (*iter).iter;
				cache.erase(iter1);
				cache.push_back(url);
				iter1 = cache.end();
				iter1--;
				S.insert(Node(url, iter1));
			}
		}
		return 0;
	}