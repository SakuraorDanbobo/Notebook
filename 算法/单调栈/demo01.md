```c++
描述：给出一组数字，求一区间，使得区间元素和乘以区间最小值最大，结果要求给出这个最大值和区间的左右端点
输入：3 1 6 4 5 2
输出：60
       3 5
解释：将3到5（6+4+5）这段区间相加，将和与区间内最小元素相乘获得最大数字60
思路：使用暴力解法求出所有区间，再求出区间的最小值相乘跟新数据，并不是一种很好的算法
y
1.设置一个单调递减的栈（栈内0~n为单调递增）
2.当遇到小于栈顶元素的值，我们开始更新数据，因为当前遇到的值一定是当前序列最小的

int GetMaxSequence(vector<int>& v)
{
	stack<int> st;
	vector<int> vs(v.size()+1);
	vs[0] = 0;
	for (int i = 1; i < vs.size(); i++)
	{
			vs[i] = vs[i - 1] + v[i-1];
	}
	v.push_back(-1);
	int top, start, end, ret = 0;
	for (int i = 0; i < v.size(); i++)
	{
		if (st.empty() || v[st.top()] <= v[i])
		{
			st.push(i);
		}
		else
		{
			while (!st.empty() && v[st.top()] > v[i])
			{
				top = st.top();
				st.pop();
				int tmp = vs[i] - vs[top];
				tmp = tmp * v[top];
				if (tmp > ret)
				{
					ret = tmp;
					start = top+1;
					end = i;
				}
			}
			st.push(top);
			v[top] = v[i];//与第二题相同的道理，将当前数据的更改最左的top下标，防止出现比当前数据更小的数据
			//这句在这道题里真的超级难理解，但是只要你有耐心相信你可以理解的
		}
	}
	return ret
}
```

