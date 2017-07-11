# 题目链接
Shortest Proper Prefix[http://hihocoder.com/problemset/problem/1107]
# 思路
Trie树，遍历每个节点，找出来以该节点为前缀的字符串的个数，那么对于某个符合条件的节点来说，它的父节点代表的前缀属于的字符串的数量一定大于5，它本身小于等于5。遍历这个trie树，统计数量。
# 代码
	#define MAXSIZE 2000005
	#define CHAR_RANGE 26
	struct TrieNode
	{
		bool flag;//是否是单词的结束
		int cntWord,f;
		int count;//以当前路径为前缀的词典中词的数量
		TrieNode *childs[CHAR_RANGE];
		TrieNode *parent;
		TrieNode *fail;
		TrieNode() :parent(NULL), flag(false), count(0), fail(NULL) {
			cntWord = f = 0;
			for (int i = 0; i < CHAR_RANGE; i++)
				childs[i] = NULL;
		}
	};
	void insertWord(TrieNode *p, char* word)
	{
		if ((*word) == '\0')
		{
			p->flag = true;
			++p->cntWord;
			return;
		}
		int index = word[0] - 'a';
		if (p->childs[index] == NULL)
			p->childs[index] = new TrieNode();
		if (p->childs[index]->parent == NULL)
			p->childs[index]->parent = p;
		p->childs[index]->count++;
		insertWord(p->childs[index], word + 1);
	}
	char str[MAXSIZE];
	int ans = 0;
	int dfs(TrieNode *node)
	{
		for (size_t i = 0; i < CHAR_RANGE; i++)
		{
			if (node->childs[i] != NULL)
				node->f+=dfs(node->childs[i]);
		}
		node->f += node->cntWord;
		for (size_t i = 0; i < CHAR_RANGE; i++)
		{
			if (node->childs[i] != NULL)
			{
				if (node->childs[i]->f <= 5 && node->f > 5)
					ans++;
			}
		}
		return node->f;
	}
	int main() {
	#ifdef DEBUG
		//ifstream cin("in.txt");
		//ofstream cout("out.txt");
		freopen("in.txt", "r", stdin);
		freopen("out.txt", "w", stdout);
	#endif

		int n;
		TrieNode *root=new TrieNode;
		scanf("%d", &n);
		for (size_t i = 0; i < n; i++)
		{
			scanf("%s", str);
			insertWord(root, str);
		}
		dfs(root);
		printf("%d", ans);
		return 0;
	}