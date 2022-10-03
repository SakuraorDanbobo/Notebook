```c++
void getnext(string str,int next[])
{
	next[0]=0;
	//前缀尾部位置,另外代表有j个长度的前后缀相同的字符个数
	int j=0;  
	
	int len=str.length();
	for(int i=1;i<len;i++) //后缀尾部位置 
	{
//		cout<<i<<" "<<j<<"-----\n"; 
		//若当前匹配字母不同，找到之前最长前缀和当前匹配的后缀 
		while(j>0 && str[j]!=str[i])j=next[j-1];
		//前后字符串相同, j++
		if(str[j]==str[i])  
		{
			j++; 	
		} 
//		cout<<i<<" "<<j<<"\n"; 
		next[i]=j; //标记若匹配失败，需要回退的位置 
	} 
}
```