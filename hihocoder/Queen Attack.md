# 题目链接
Queen Attack[http://hihocoder.com/problemset/problem/1497]
# 思路
这道题的重点是找到每行每列每个对角线上的queen数量，那么统计数量，对于对角线，可以用row+col和row-col唯一确定一条对角线，所以直接统计数量即可。
# 代码
	int N;
	int main() {
	#ifdef DEBUG
	    ifstream cin("in.txt");
		ofstream cout("out.txt");
		/*freopen("in.txt", "r", stdin);
		freopen("out.txt", "w", stdout);*/
	#endif
		int64_t row, col;
		map<int64_t, int64_t> rows, cols, diags, idiags;
		cin >> N;
		for (size_t i = 0; i < N; i++)
		{
			cin >> row >> col;
			rows[row] += 1;
			cols[col] += 1;
			diags[row + col] += 1;
			idiags[col - row] += 1;
		}
		int64_t ans = 0;
		auto f = [&](const pair<int, int> &p){
			ans += p.second*(p.second - 1) / 2;
		};
		for_each(rows.begin(),rows.end(),f);
		for_each(cols.begin(), cols.end(), f);
		for_each(diags.begin(), diags.end(), f);
		for_each(idiags.begin(), idiags.end(), f);
		cout << ans << endl;
		return 0;
	}