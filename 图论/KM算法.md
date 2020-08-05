# KM算法

## 原理

**KM 算法是用于求带权二分图的最优匹配的算法**，其时间复杂度为 O(N^3)。

1.首先选择顶点数较少的为 X 部（左点集），初始时对 X 部的每一个顶点设置顶标，顶标的值为该点关联的最大边的权值，Y 部（右点集）的顶点顶标为 0。

2.对于 X 部中的每个顶点，在相等子图中利用匈牙利算法找一条增广路径，如果没有找到，则修改顶标，扩大相等子图，继续找增广路径。

3.当 X 部的每个点都找到增广路径时，此时意味着每个点都在匹配中，即找到了该二分图的完全匹配。该完全匹配即为二分图的最优匹配。

## 相关概念

1）相等子图：由于每个顶点有一个顶标，如果选择边权等于两端点的顶标之和的边，它们组成的图称为相等子图。

2）顶标：每个点的顶标为该点关联的最大边的权值。

## 顶标的修改

如果从 X 部中的某个点 Xi 出发在相等子图中没有找到增广路径，则需要修改顶标。

如果没有找到增广路径，则一定找到了许多条从 Xi 出发并结束于 X 部的匹配边与未匹配边交替出现的路径，即交错路。

将交错路中 X 部的顶点顶标减去一个值 d，交错路中属于 Y 部的顶点顶标加上一个值 d，那么会发现：

- 两端都在交错路中的边（i,j），其顶标和没有变化，即：其原属于相等子图，现仍属于相等子图。
- 两端都不在交错路中的边（i,j），其顶标也没有变化，即：其原来属于（或不属于）相等子图，现仍属于（或不属于）相等子图。
- X 端不在交错路中，Y 端在交错路中的边（i,j），其顶标和会增大，即：其原来不属于相等子图，现仍不属于相等子图。
- X 端在交错路中，Y 端不在交错路中的边（i,j），其顶标和会减小，即：其原来不属于相等子图，现可能进入相等子图，从而使相等子图得到扩大。

修改顶标的目的就是要扩大相等子图，为保证至少有一条边进入相等子图，可以在交错路的边中寻找顶标和与边权之差最小的边，也即前述的 d 值。

将交错路中属于 X 部的顶点减去 d，交错路中属于 Y 部的顶点加上 d，则可以保证至少有一条边扩充进入相等子图。

## 相等子图的性质

1）任意时刻，相等子图的 **最大权匹配 ≤ 相等子图的顶标和**

2）任意时刻，相等子图的 **顶标和=所有顶点的顶标和**

3）扩充相等子图后，相等子图的顶标和会减小

4）相等子图的 最大匹配=原图的完全匹配 时，匹配边的权值和=所有顶点的顶标和，此匹配即为最优匹配

## 代码

**最优匹配**

```C++
#include<cstdio>
#include<cstring>
#include<cmath>
#define INF 0x3f3f3f3f
#define N 1001
int n,m;//x、y中结点个数，下标从1开始
int G[N][N];//边权值矩阵
int Lx[N],Ly[N];//x、y中每个点的期望值
bool visX[N],visY[N];//标记左右点集是否已被访问过
int linkX[N],linkY[N];//linkX[i]表示与X部中点i匹配的点,linkY[i]表示与Y部中点i匹配的点,-1时表示无匹配
bool dfs(int x){
    visX[x]=true;
    for(int y=1;y<=m;y++){
        if(!visY[y]){
            int temp=Lx[x]+Ly[y]-G[x][y];
            if(temp==0){//不在交替路中
                visY[y]=true;//放入交替路
                if(linkY[y]==-1 || dfs(linkY[y])){//如果是未匹配点,说明交替路是增广路
                    linkX[x]=y;//交换路径
                    linkY[y]=x;
                    return true;//返回成功
                }
            }
        }
    }
    return false;//不存在增广路
}
void update(){
    int minn=INF;
    for(int i=1;i<=n;i++){//找出边权与顶标和的最小的差值
        if(visX[i]){
            for(int j=1;j<=m;j++){
                if(!visY[j]){
                    minn=min(minn,Lx[i]+Ly[j]-G[i][j]);
                }
            }
        }
    }
 
    for(int j=1;j<=n;j++)//将交错路中X部的点的顶标减去minn
        if(visX[j])
            Lx[j]-=minn;
    for(int j=1;j<=m;j++)//将交错路中Y部的点的顶标加上minn
        if(visY[j])
            Ly[j]+=minn;
}
int KM(){//更新理想值,纳入更多的边
    memset(linkX,-1,sizeof(linkX));
    memset(linkY,-1,sizeof(linkY));
    memset(Lx,0,sizeof(Lx));
    memset(Ly,0,sizeof(Ly));
    
    for(int i=1;i<=n;i++)//更新理想值
        for(int j=1;j<=m;j++)
            Lx[i]=max(Lx[i],G[i][j]);
 
    for(int i=1;i<=n;i++){
        while(true){
            memset(visX,false,sizeof(visX));
            memset(visY,false,sizeof(visY));
 
            if(dfs(i))
                break;
            else
                update();
        }
    }
 
    int ans=0;
    for(int i=1;i<=n;i++)
        if(linkY[i]!=-1)//若存在边
            ans+=G[linkY[i]][i];//统计边权和
 
    return ans;
}
 
int main(){
    while(scanf("%d%d",&n,&m)!=EOF&&(n+m)){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
                scanf("%d",&G[i][j]);
 
        printf("%d\n",KM());
    }
    return 0;
}
```

