# 题目链接
平衡树·Splay2[http://hihocoder.com/problemset/problem/1333]

# 思路
splay平衡树，可以减少区间操作的复杂度，加入懒标记


# 代码
	class Splay
	{
		struct Node
		{
			Node *left, *right, *father;
			int64_t key;
			int64_t val;//保存的值
			int64_t totalVal, num;//以当前结点为根的子树的值的和，以当前节点为根的子树的节点个数
			int64_t lazyValue;
			Node(Node *f = nullptr, int64_t k = 0, int64_t v = 0) :father(f), key(k), val(v) {
				left = right = nullptr;
				totalVal = num = 0;
				lazyValue = 0;
			};
		};
	public:
		Splay()
		{
			root = nullptr;
			insert(INT_MIN);
			insert(INT_MAX);
		};
		void insert(int64_t key, int64_t value = 0)
		{
			if (root)
			{
				Node *node = bstInsert(root, key, value);
				splay(node, nullptr);
			}
			else
				root = new Node(nullptr, key, value);
		}
		bool find(int64_t key)
		{
			Node *node = getNode(key);
			if (node)
				return true;
			else
				return false;
		}
		int findLessEqual(int64_t key)
		{
			Node *node = getNode(key);
			if (node)
				return node->key;
			else
			{
				node = findPrev(key);
				return node->key;
			}
		}
		void remove(int64_t key)
		{
			Node *prev = findPrev(key);
			Node *next = findNext(key);
			splay(prev, NULL);
			splay(next, prev);
			next->left = nullptr;
			update(next);
			update(prev);
		}
		void removeInterval(int64_t a, int64_t b)
		{
			Node *prev = findPrev(a);
			Node *next = findNext(b);
			splay(prev, NULL);
			splay(next, prev);
			next->left = nullptr;
			update(next);
			update(prev);
		}
		int64_t queryIntervalTotal(int64_t a, int64_t b)
		{
			Node *prev = findPrev(a);
			Node *next = findNext(b);
			splay(prev, nullptr);
			splay(next, prev);
			if (!next->left) return 0;
			return next->left->totalVal;
		}
		void addIntervalDelta(int64_t a, int64_t b, int64_t delta)
		{
			Node *prev = findPrev(a);
			Node *next = findNext(b);
			splay(prev, nullptr);
			splay(next, prev);
			marking(next->left, delta);
		}
	private:
		void marking(Node *node, int delta)
		{
			node->lazyValue += delta;
			node->totalVal += node->num*delta;
			node->val += delta;
		}
		void push(Node *node)
		{
			if (node->left)	marking(node->left, node->lazyValue);
			if (node->right) marking(node->right, node->lazyValue);
			node->lazyValue = 0;
			update(node);
		}
		//update函数来更新节点的信息：保证num与totalVal的值正确
		void update(Node *x)
		{
			x->num = 1;
			x->totalVal = x->val;
			if (x->left)
			{
				x->num += x->left->num;
				x->totalVal += x->left->totalVal;
			}
			if (x->right)
			{
				x->num += x->right->num;
				x->totalVal += x->right->totalVal;
			}
		}
		Node* getNode(int64_t key)
		{
			Node *node = bstFind(key);
			if (node) splay(node, nullptr);
			return node;
		}
		//不管key存不存在，都返回小于key的最大值所在的节点
		Node *findPrev(int64_t key)
		{
			Node *node = root;
			while (node)
			{
				if (node->key == key) return _findPrev(node);
				else if (key < node->key)
				{
					if (node->left)
						node = node->left;
					else //要查找的节点不存在，存在prev(key)等价于查找prev(node->val)，因为node->val是大于key的最小的值
						return _findPrev(node);
				}
				else
				{
					if (node->right)
						node = node->right;
					else
						return node;
				}
			}
			return nullptr;
		}
		//针对key存在的情况
		Node *_findPrev(int64_t key)
		{
			splay(getNode(key), nullptr);
			Node *node = root->left;
			while (node->right)	node = node->right;
			return node;
		}
		//针对已经找到Node x
		Node *_findPrev(Node *x)
		{
			splay(x, nullptr);
			Node *node = root->left;
			while (node->right) node = node->right;
			return node;
		}
		//不管key存不存在，都返回大于key的最小值所在的节点
		Node *findNext(int64_t key)
		{
			Node *node = root;
			while (node)
			{
				if (node->key == key) return _findNext(node);
				else if (key < node->key)
				{
					if (node->left)
						node = node->left;
					else
						return node;
				}
				else
				{
					if (node->right)
						node = node->right;
					else
						return _findNext(node);
				}
			}
			return nullptr;
		}
		//针对key存在的情况
		Node *_findNext(int64_t key)
		{
			splay(getNode(key), nullptr);
			Node *node = root->right;
			while (node->left)	node = node->left;
			return node;
		}
		//针对已经找到Node x
		Node *_findNext(Node *x)
		{
			splay(x, nullptr);
			Node *node = x->right;
			while (node->left) node = node->left;
			return node;
		}
		Node *bstInsert(Node *node, int64_t key, int64_t value = 0)
		{
			push(node);
			Node *ret = nullptr;
			if (key < node->key)
			{
				if (node->left)
					ret=bstInsert(node->left, key, value);
				else
				{
					ret = new Node(node, key, value);
					node->left = ret;
				}
			}
			else if (key > node->key)
			{
				if (node->right)
					ret = bstInsert(node->right, key, value);
				else
				{
					ret = new Node(node, key, value);
					node->right = ret;
				}
			}
			else
				node->val = value;
			update(node);
			return ret;
		}
		Node *bstFind(int64_t key)
		{
			Node *current = root;
			//终止条件：current为null或current->val==key
			while (current&&current->key != key)
			{
				if (key < current->key)
					current = current->left;
				else
					current = current->right;
			}
			return current;
		}
		void rightRotate(Node *x)
		{
			Node *p = x->father;
			x->father = p->father;
			push(p);
			push(x);
			if (p->father)
			{
				if (p->father->left == p)
					p->father->left = x;
				else
					p->father->right = x;
			}
			else
				root = x;
			p->left = x->right;
			if (x->right) x->right->father = p;
			x->right = p;
			p->father = x;
			update(p);
			update(x);
		}
		void leftRotate(Node *x)
		{
			Node *p = x->father;
			x->father = p->father;
			push(p);
			push(x);
			if (p->father)
			{
				if (p->father->left == p)
					p->father->left = x;
				else
					p->father->right = x;
			}
			else
				root = x;
			p->right = x->left;
			if (x->left) x->left->father = p;
			x->left = p;
			p->father = x;
			update(p);
			update(x);
		}
		//对于x以及x的祖先y，对x调整，使得x是y的儿子。（需要保证y是x的祖先）
		void splay(Node *x, Node *y)
		{
			while (x->father != y)
			{
				Node *p = x->father;
				if (p->father == y)
				{
					//直接进行zig操作
					if (p->left == x)
						rightRotate(x);
					else
						leftRotate(x);
				}
				else
				{
					Node *g = p->father;
					if (g->left == p)
					{
						if (p->left == x)
						{
							//x，p同为左儿子，zig-zig操作
							rightRotate(p);
							rightRotate(x);
						}
						else
						{
							//p为左儿子，x为右儿子，zig-zag操作
							leftRotate(x);
							rightRotate(x);
						}
					}
					else
					{
						if (p->right == x)
						{
							//x，p同为右儿子，zig-zig操作
							leftRotate(p);
							leftRotate(x);
						}
						else
						{
							//p为右儿子，x为左儿子，zig-zag操作
							rightRotate(x);
							leftRotate(x);
						}
					}
				}
			}
		}
	private:
		Node *root;
	};
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif 
		char q;
		int64_t n, a, b, d;
		Splay tree;
		cin >> n;
		while (n--)
		{
			cin >> q;
			if (q == 'I')
			{
				cin >> a;
				tree.insert(a);
			}
			else if (q == 'Q')
			{
				cin >> a ;
				cout << tree.findLessEqual(a) << endl;
			}
			else if (q == 'D')
			{
				cin >> a >> b;
				tree.removeInterval(a, b);
			}
		}
		return 0;
	}