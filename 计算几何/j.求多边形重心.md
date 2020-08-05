# j.求多边形重心

## 思路

三角形的重心：A(x1,y1)，B(x2,y2)，C(x3,y3)  

重心 `G( (x1+x2+x3) / 3,(y1+y2+y3) / 3）`

三角形面积 `S=1/2*AB*AC*sinθ = (x2-x1)*(y3-y2)-(x3-x1)*(y2-y1)` 

(向量AB和向量AC叉乘的行列式 / 2)

n边形可以以A为点分成n-2个三角形

**多边形重心**

` x = (∑ x[i]*s[i]) / ∑s[i] / 3;  y = (∑ y[i]*s[i] ) / ∑s[i] / 3`

其中（x[i], y[i], s[i]）分别是所划分的第i个三角形的重心坐标和面积

## 代码

```C++
#include<bits/stdc++.h>
using namespace std;
double x1,yl,x2,y2,x3,y3;
double sum_x,sum_y,sum_s,s;//∑x[i]*s[i]、∑y[i]*s[i]、∑s[i] 
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        int n;
        cin>>n;
        sum_x=sum_y=sum_s=0;
        cin>>x1>>yl>>x2>>y2;
        for(int i=1;i<=n-2;i++)
        {
            cin>>x3>>y3;
            s=((x2-x1)*(y3-yl)-(x3-x1)*(y2-yl))/2.0;
            sum_x+=(x1+x2+x3)*s;
            sum_y+=(yl+y2+y3)*s;
            sum_s+=s;
 
            x2=x3;
            y2=y3;
        }
        printf("%.2lf %.2lf\n",sum_x/sum_s/3,sum_y/3*sum_s/3);
    }
}
```

