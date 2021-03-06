# f.判断点到线段的最近点

## 思路

1.公式
设直线方程为ax+by+c=0,点坐标为(m,n) 
则垂足为`((b*b*m-a*b*n-a*c)/(a*a+b*b),(a*a*n-a*b*m-b*c)/(a*a+b*b)) `
2.计算点到线段的最近点

- 如果该线段平行于X轴（Y轴），则过点point作该线段所在直线的垂线，垂足很容 易求得，然后计算出垂足，如果垂足在线段上则返回垂足，否则返回离垂足近的端点；

- 如果该线段不平行于X轴也不平行于Y轴，则斜率存在且不为0。设线段的两端点为 pt1和pt2，斜率为：

   `k = ( pt2.y - pt1. y ) / (pt2.x - pt1.x )`; 
  该直线方程为： 
  `y = k* ( x - pt1.x) + pt1.y `
  其垂线的斜率为 - 1 / k， 
  垂线方程为： 
  `y = (-1/k) * (x - point.x) + point.y` 
  联立两直线方程解得： 
  `x = ( k^2*pt1.x + k*(point.y - pt1.y) + point.x)/( k^2 + 1) `
  `y = k * ( x - pt1.x) + pt1.y`;

- 然后再判断垂足是否在线段上，如果在线段上则返回垂足；如果不在则计算两端点 到垂足的距离，选择距离垂足较近的端点返回。

## 代码
```C
 List<Integer> edgeList=road.getEdgeList();
float k,x,y,x1,y1,x2,y2,px,py;
for(int edgeId:edgeList){
Edge edge=TopologyMap.edgeMap.get(edgeId);
Node fromNode=TopologyMap.nodeMap.get(edge.getFromNodeId());
Node toNode=TopologyMap.nodeMap.get(edge.getToNodeId());
x1=fromNode.getLat();y1=fromNode.getLong();
x2=toNode.getLat();y2=toNode.getLong();
px=GPoint.getLat();py=GPoint.getLong();
//求垂足
if(x1==x2){
x=x1;
y=GPoint.getLong();
}else if(y1==y2){
y=y1;
x=GPoint.getLat();
}else{   //存在斜率k
k=(y2-y1)/(x2-x1);
x= (k*k*x1 + k*(py-y1) + px)/(k*k+1) ;
y=k*(x-x1) + y1; 
}
//判断垂足是否在edge上
float edgeLength=(float)Math.sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));
float dist1=(float)Math.sqrt((x-x1)*(x-x1)+(y-y1)*(y-y1));
float dist2=(float)Math.sqrt((x-x2)*(x-x2)+(y-y2)*(y-y2));
float compare=dist1+dist2;
if(compare-edgeLength>0.0004){//不在线段上，则取最近点为端点
 if(dist1<dist2){
 x=x1;y=y1;
 }else{
 x=x2;y=y2;
 }
}
Node pedal=new Node(x,y,edgeId,roadId);

