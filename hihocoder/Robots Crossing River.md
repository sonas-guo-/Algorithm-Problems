# 题目链接
Robots Crossing River[http://hihocoder.com/problemset/problem/1340]

# 思路
首先需要看出整个流程的瓶颈完全在B-C这一段，换句话说我们只需求出所有机器人从B到C的最少时间，再加上2小时就是答案。事实上这个时间恰好等于把所有机器人直接从B运到C最少需要的船次x6。设最小船次为times，那么最终的结果就是(times-1)\*6+4+2=times\*6
如果没有“一船超过15个机器人则每种机器人不能超过半数”的限制，我们只需要20/船运走即可，最少船次是ceil((X+Y+Z)/20)。
由于有上面的限制，我们需要仔细讨论一下XYZ的相对大小。不妨设X >= Y >= Z，同时我们称三种机器人也为X、Y、Z类。
1、如果X <= Y + Z，那么我们仍然可以20/船运走，同时所有船都没有机器人超过半数。
这种情况最少船次仍然是是ceil((X+Y+Z)/20)。这种情况是每次都装满20人，然后三种机器人按XYZ的比例上船。这样每种比例都不会超过一半，因为最多的X都不超过一半。
2、 如果X > Y + Z，这时我们没办法使所有船都载20机器人。但是我们当然希望能尽量派出载20机器人的船。于是有如下贪心策略：
1) 首先尽量10个X类和10个非X类组成一船，派出若干船直到非X类不足10个机器人。
2) 余下若干(不足10个)非X类机器人，配合尽可能多X类机器人组成一船。这里需要讨论余下的非X类机器人有多少个，不妨设为K。如果K不足8个，那么最多配合15-K个X类机器人，组成一船15个机器人；否则可以配合K个X类机器人，组成一船2K个机器人。
3) 最后只剩下若干X类机器人，这些机器人只能15/船派出。 

# 代码
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		std::ios::sync_with_stdio(false);
		int X, Y, Z;
		cin >> X >> Y >> Z;
		auto maximum = [](int a, int b, int c) { return max(a, max(b, c)); };
		auto minimum = [](int a, int b, int c) { return min(a, min(b, c)); };
		auto medium = [&](int a, int b, int c) {return a + b + c - maximum(a, b, c) - minimum(a, b, c); };
		int boats = 0,tmpX,tmpY,tmpZ,times=0;
		while (X!=0||Y!=0||Z!=0)
		{
			tmpX = maximum(X, Y, Z);
			tmpY = medium(X, Y, Z);
			tmpZ = minimum(X, Y, Z);
			X = tmpX;
			Y = tmpY;
			Z = tmpZ;
			if (X <= Y + Z)
			{
				boats = ceil((X + Y + Z) / 20.0);
				X = 0;
				Y = 0;
				Z = 0;
				times+=boats;
			}
			else
			{
				if (Y + Z >= 10)
				{
					boats = (Y + Z) / 10;
					tmpX = boats*10;
					tmpY = boats*10*Y/(Y+Z);
					tmpZ = boats*20-tmpX-tmpY;
					X -= tmpX;
					Y -= tmpY;
					Z -= tmpZ;
					times+=boats;
				}
				else
				{
					if (Y + Z != 0)
					{
						if (Y + Z < 8)
						{
							tmpX = 15 - Y - Z;
							tmpY = Y;
							tmpZ = Z;
							X -= tmpX;
							Y -= tmpY;
							Z -= tmpZ;
							times++;
						}
						else
						{
							tmpX = (Y + Z);
							tmpY = Y;
							tmpZ = Z;
							X -= tmpX;
							Y -= tmpY;
							Z -= tmpZ;
							times++;
						}
					}
					else
					{
						boats = ceil(X / 15.0);
						X =0;
						times+=boats;
					}
				}
			}
		}
		cout << times*6<<endl;
		return 0;
	}
