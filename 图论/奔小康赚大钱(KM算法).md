# 奔小康赚大钱(KM算法)

## 题目

传说在遥远的地方有一个非常富裕的村落,有一天,村长决定进行制度改革：重新分配房子。
这可是一件大事,关系到人民的住房问题啊。村里共有n间房间,刚好有n家老百姓,考虑到每家都要有房住（如果有老百姓没房子住的话，容易引起不安定因素），每家必须分配到一间房子且只能得到一间房子。
另一方面,村长和另外的村领导希望得到最大的效益,这样村里的机构才会有钱.由于老百姓都比较富裕,他们都能对每一间房子在他们的经济范围内出一定的价格,比如有3间房子,一家老百姓可以对第一间出10万,对第2间出2万,对第3间出20万.(当然是在他们的经济范围内).现在这个问题就是村领导怎样分配房子才能使收入最大.(村民即使有钱购买一间房子但不一定能买到,要看村领导分配的).

- 输入

  输入数据包含多组测试用例，每组数据的第一行输入n,表示房子的数量(也是老百姓家的数量)，接下来有n行,每行n个数表示第i个村名对第j间房出的价格(n<=300)。

- 输出

  输出最大的收入值

## 思路

给你一个带权的二分图，求该二分图的最优匹配权值，左点集代表村名，右点集代表房子，最优匹配 KM 模板题

## 代码

```C++
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<string>
#include<cstring>
#include<cmath>
#include<ctime>
#include<algorithm>
#include<stack>
#include<queue>
#include<vector>
#include<set>
#include<map>
#define PI acos(-1.0)
#define E 1e-6
#define MOD 16007
#define INF 0x3f3f3f3f
#define N 1001
#define LL long long
using namespace std;
int n;
int G[N][N];
int Lx[N],Ly[N];
bool visX[N],visY[N];
int linkX[N],linkY[N];
bool dfs(int x){
    visX[x]=true;
    for(int y=1;y<=n;y++){
        if(!visY[y]){
            int temp=Lx[x]+Ly[y]-G[x][y];
            if(temp==0){
                visY[y]=true;
                if(linkY[y]==-1 || dfs(linkY[y])){
                    linkX[x]=y;
                    linkY[y]=x;
                    return true;
                }
            }
        }
    }
    return false;
}
void update(){
    int minn=INF;
    for(int i=1;i<=n;i++)
        if(visX[i])
            for(int j=1;j<=n;j++)
                if(!visY[j])
                    minn=min(minn,Lx[i]+Ly[j]-G[i][j]);
 
    for(int i=1;i<=n;i++){
        if(visX[i])
            Lx[i]-=minn;
        if(visY[i])
            Ly[i]+=minn;
    }
}
int KM(){
    memset(linkX,-1,sizeof(linkX));
    memset(linkY,-1,sizeof(linkY));
 
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
        ans+=G[linkY[i]][i];
 
    return ans;
}
int main(){
    while(scanf("%d",&n)!=EOF&&(n)){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                scanf("%d",&G[i][j]);
 
        printf("%d\n",KM());
    }
    return 0;
}
```

