# 题目链接
Constraint Checker[http://hihocoder.com/problemset/problem/1341]

# 思路
将原先每个Constraint都转换成若干个二元关系，这里用正则表达式提取这些关系，然后把读到的值代入一一验证即可。

# 代码
	int n,T;
	struct Relation
	{
		string var1;
		string var2;
		string symbol;
		Relation(string a, string b, string c) :var1(a), var2(c), symbol(b) {};
	};
	vector<Relation> rels;
	set<string> variables;
	map<string, int> var2value;
	void checkVariable(string s)
	{
		regex pat("[A-Z]");
		if (regex_match(s, pat))
			variables.insert(s);
	}
	void parseFineGrained(string str,int p)
	{
		string a, b, c;
		if (str[p + 1] == '=')
		{
			a = str.substr(0, p);
			b = str.substr(p, 2);
			c = str.substr(p + 2);
		}
		else
		{
			a = str.substr(0, p);
			b = str.substr(p, 1);
			c = str.substr(p + 1);
		}
		checkVariable(a);
		checkVariable(c);
		rels.push_back(Relation(a,b,c));
	}
	void parseCoarseGrained(string str)
	{
		regex pat("[A-Z0-9]+(<|<=)[A-Z0-9]+");
		smatch match;
		while (regex_search(str,match,pat))
		{
			parseFineGrained(match.str(),match.position(1));
			if (str[match.position(1) + 1] == '=')
				str = str.substr(match.position(1) + 2);
			else
				str = str.substr(match.position(1) + 1);
		}
	}
	int getValue(string s)
	{
		if (variables.find(s) == variables.end())
			return atoi(s.c_str());
		else
			return var2value[s];
	}
	bool check()
	{
		for (size_t i = 0; i < rels.size(); i++)
		{
			int a, b;
			a = getValue(rels[i].var1);
			b = getValue(rels[i].var2);
			if (rels[i].symbol == "<")
			{
				if (a >= b)
					return false;
			}
			else
			{
				if (a > b)
					return false;
			}
		}
		return true;
	}
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		std::ios::sync_with_stdio(false);
		cin >> n;
		string expr;
		for (size_t i = 0; i < n; i++)
		{
			cin >> expr;
			parseCoarseGrained(expr);
		}
		cin >> T;
		string var;
		int value;
		for (size_t t = 0; t < T; t++)
		{
			var2value.clear();
			for (size_t i = 0; i < variables.size(); i++)
			{
				cin >> var >> value;
				var2value[var] = value;
			}
			if (check())
				cout << "Yes" << endl;
			else
				cout << "No" << endl;
		}
		return 0;
	}