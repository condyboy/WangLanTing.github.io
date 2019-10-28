---
title: djistra算法+bellman算法+floyd算法
date: 2019-10-28 18:54:10
tags:
categories: 算法
---
# djistra算法+bellman算法+floyd算法
## djistra算法

算法原理：
贪心策略
1、将开始节点加入S集合
2、在刚加入的节点相连的边中更新暂存函数dist数组
3、选择dist数组中最小的，并且还未加入S集合的加入，将最小值保存到result数组中
缺点：不能计算边中含有负权重的值，否则会计算错误
```cpp
#include <iostream>
#include <stack>
#include <cstring>

using namespace std;

int main() {
    int n;
    cin >> n;
    int tag[100]; //标识其加入了节点集
    int path[100];  //标识最短路径
    int num[100][100]; //存储节点边权重
    int dist[100]; //暂存最短路径
    int result[100]; //最后选取的最短路径的值
    memset(tag, 0, sizeof(tag));
    memset(num,0x3f3f3f3f,sizeof(num));  //用0x3f3f3f3f，保证相加时不会溢出的最大值
    memset(result, 0x3f3f3f3f, sizeof(result));
    memset(dist, 0x3f3f3f3f, sizeof(dist));
    memset(path, -1, sizeof(path));
    path[0]=0;
    tag[0] = 1;
    result[0]=0;
    for (int i = 0; i < n; i++) {
        int x,y,w;
        cin >> x >> y >> w;
        num[x][y]=w;
    }
    int k=0;  //标识当前新加进来的节点
    for (int i = 0; i < n; i++) {
        for (int j = 1; j < n; j++) {
            if (tag[j]!=1&&(result[k]+num[k][j])<dist[j]){  //加过的点不进入判断
                dist[j]=result[k]+num[k][j];
            }
        }
        int min=0x3f3f3f3f,flag=0;
        for(int j = 1 ; j < n ;j++){
            if(dist[j]<min&&tag[j]!=1)    ////加过的点不进入判断
            {
                min=dist[j];
                flag=j;
            }
        }
        path[flag]=k;  //先更新路径值，再更新k即当前新加入的节点
        tag[flag]=1;  //将点加入集合，下回不进行判断
        result[flag]=min; //加入最后的result集合
        k=flag;   //标识当前新加入的节点
    }
    for(int i = 1;i < n ;i++){
        cout<<result[i]<<endl;
    }
    for(int i = 1;i < n ;i++){
        int x=i;
        stack<int> s;
        while(path[x]!=0&&path[x]!=-1){
            s.push(path[x]);
            x=path[x];
        }
        cout<<0<<"->";
        while (!s.empty())
        {
            cout<<s.top()<<"->";
            s.pop();
        }
        cout<<i<<endl;
    }

    return 0;
}
/**
 * 输入数据：
 *  8
    0 5 100
    0 4 30
    0 2 10
    4 5 60
    3 5 10
    1 2 5
    2 3 50
    0 2 10
 * 输出数据：
 *  1061109567
    10
    60
    30
    70
    1061109567
    1061109567
    0->1
    0->2
    0->2->4->3
    0->2->4
    0->2->4->3->5
    0->6
    0->7
*/
```
## bellman算法
算法原理：
参考[bellman](https://www.cnblogs.com/fzl194/p/8728529.html)
遍历边，每次找到据初始节点走过K条边的值，并进行松弛操作，更新dist数组，注意方向，这里dist数组为到初始点的距离。
优点：能够解决负权重问题，能够发现图中包含负环
缺点：不能够存在负环，否则值会为负无穷
```cpp
#include <iostream>
#include <cstring>
#include <stack>
using namespace std;

typedef struct  node
{
    int x,y,w;
}Node;


int main(){
    int n,m;
    cin>>n>>m;
    Node edge[m];
    int dist[n+1];
    int path[100];
    memset(dist,0x3f3f3f3f,sizeof(dist));
    dist[0]=0;
    for(int i = 0 ; i < m ;i++){
        int x,y,w;
        cin>>x>>y>>w;
        edge[i].x=x;
        edge[i].y=y;
        edge[i].w=w;
    }
    for(int i = 0 ; i < n ;i++){
        int tag=0;
        for(int j = 0; j < m;j++){
            if(dist[edge[j].x]+edge[j].w<dist[edge[j].y])
            {
                dist[edge[j].y]=dist[edge[j].x]+edge[j].w;
                path[edge[j].y]=edge[j].x;
                tag=1;
            }
        }
        if(tag&&i==n-1){
            cout<<"图中出现负环"<<endl;
        }
        if(!tag)    //当上一轮没有更新时，边无必要进行下一轮
            break;
    }
    for(int i = 1 ; i < n ;i++){
        cout<<dist[i]<<endl;
        int x=i;
        stack<int> s;
        while(path[x]!=0){
            s.push(path[x]);
            x=path[x];
        }
        cout<<0<<"->";
        while (!s.empty())
        {
            cout<<s.top()<<"->";
            s.pop();
        }
        cout<<i<<endl;
    }
    return 0;
}

/**
 * 测试数据：
 *  7 10
    0 1 6
    0 2 5
    0 3 5
    1 4 -1
    2 1 -2
    2 4 1
    3 2 -2
    3 5 -1
    4 6 3
    5 6 3
    输出样例：
    1
    0->3->2->1
    3
    0->3->2
    5
    0->3
    0
    0->3->2->1->4
    4
    0->3->5
    3
    0->3->2->1->4->6
*/

```
## floyd算法

每次选取中间点，并进行更新result数组，直到所有的情况全包括
优点：算法思想简单，能够计算任意两点间的最短路径
缺点：时间复杂度高

```cpp
#include <iostream>

#include <cstring>
using namespace std;

int main() {
    int n;
    cin >> n;
    int tag[100]; //标识其加入了节点集
    int path[100][100];  //标识最短路径
    int result[100][100]; //最后选取的最短路径的值
    memset(result, 0x3f3f3f3f, sizeof(result));  //没有路，或不是对角线的均为无穷大
    for (int i = 0; i < n; i++) {
        int x,y,w;
        cin >> x >> y >> w;
        result[x][y]=w;  //
        result[i][i]=0;  //自己到自己的路径值为0
    }
    for(int i = 0 ; i < n ;i++){
        for(int j = 0 ; j < n ;j++){
            path[i][j]=j;
        }
    }
    for(int i = 0 ; i < n ;i++){
        for(int j = 0; j < n ; j++){
            for (int k = 0; k < n; k++)
            {
                if((result[i][k]+result[k][j])<result[i][j]){
                    result[i][j]=result[i][k]+result[k][j];
                    path[i][j]=path[i][k];  //表示通过中间点k
                } 
            }
        }
    }
    for(int i=0;i<n;i++){
        cout<<result[0][i]<<endl;
        cout<<"路径为："<<0;
        int temp=temp=path[0][path[0][i]];
        int x=0,y=i;
        while (temp!=y)
        {
            temp=path[x][path[x][y]];
            x=path[x][y];
            if(temp!=y)
            cout<<"->"<<temp;
        }
        cout<<"->"<<i;
        cout<<endl;
        
    }
    return 0;
}
/**
 * 测试数据：
 *  8
    0 5 100
    0 4 30
    0 2 10
    4 5 60
    3 5 10
    1 2 5
    2 3 50
    0 2 10
    输出样例：
    0
    路径为：0->0
    1061109567
    路径为：0->1
    10
    路径为：0->2
    60
    路径为：0->2->3
    30
    路径为：0->4
    70
    路径为：0->2->3->5
    1061109567
    路径为：0->6
    1061109567
    路径为：0->7
*/
```
