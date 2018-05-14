# 题目链接
平衡树·Splay[http://hihocoder.com/problemset/problem/1329]

# 思路
splay平衡树，可以减少区间操作的复杂度。


# 代码
	class Splay
	{
		struct Node
		{
			Node *left, *right, *father;
			int key;
			Node(Node *f = nullptr, int k = 0) :father(f), key(k) { left = right = nullptr; };
		};
	public:
		Splay() 
		{
			root = nullptr; 
			insert(INT_MIN);
			insert(INT_MAX);
		};
		void insert(int key)
		{
			Node *node = bstInsert(key);
			splay(node, nullptr);
		}
		bool find(int key)
		{
			Node *node = getNode(key);
			if (node)
				return true;
			else
				return false;
		}
		int findLessEqual(int key)
		{
			Node *node = getNode(key);
			if (node)
				return node->key;
			else
			{
				insert(key);
				node = findPre(key);
				remove(key);
				return node->key;
			}		
		}
		void remove(int key)
		{
			Node *prev = findPre(key);
			Node *next = findNext(key);
			if (prev&&next)
			{
				splay(prev, NULL);
				splay(next, prev);
				next->left = nullptr;
			}
		}
		void removeInterval(int a, int b)
		{
			insert(a);
			insert(b);
			Node *prev = findPre(a);
			Node *next = findNext(b);
			if (prev&&next)
			{
				splay(prev, NULL);
				splay(next, prev);
				next->left = nullptr;
			}
			remove(a);
			remove(b);
		}
	private:
		Node* getNode(int key)
		{
			Node *node = bstFind(key);
			if (node) splay(node, nullptr);
			return node;
		}
		Node *findPre(int key)
		{
			Node *node = getNode(key);
			if (node)
				splay(node, nullptr);
			else
				return nullptr;
			node = root->left;
			while (node->right)
				node = node->right;
			return node;
		}
		Node *findNext(int key)
		{
			Node *node = getNode(key);
			if (node)
				splay(node, nullptr);
			else
				return nullptr;
			node = root->right;
			while (node->left)
				node = node->left;
			return node;
		}
		Node *bstInsert(int key)
		{
			if (!root)
			{
				root = new Node(nullptr, key);
				return root;
			}
			else
			{
				Node *current = root, *father = root;
				bool isLeft = true;
				while (current)
				{
					father = current;
					if (key < current->key)
					{
						current = current->left;
						isLeft = true;
					}
					else if (key > current->key)
					{
						current = current->right;
						isLeft = false;
					}
					else //已经存在值
						return current;
				}
				current =new Node(father, key);
				if (isLeft)
					father->left = current;
				else
					father->right = current;
				return current;
			}
		}
		Node *bstFind(int key)
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
		}
		void leftRotate(Node *x)
		{
			Node *p = x->father;
			x->father = p->father;
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
			if(x->left) x->left->father = p;
			x->left = p;
			p->father = x;
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
		int n,a,b;
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
				cin >> a;
				cout << tree.findLessEqual(a)<<endl;
			}
			else if (q == 'D')
			{
				cin >> a >> b;
				tree.removeInterval(a,b);
			}
		}
		return 0;
	}