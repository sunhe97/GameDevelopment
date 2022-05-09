# 游戏开发学习——碰撞检测理论

# AABB（Axis-aligned Bounding Box）轴对齐碰撞

## 碰撞箱与场景的坐标轴平行
## 三维实现原理：两个三维物体的中心位置向量相减的向量的三轴投影与abs(boxsizeA /2).x + abs(boxsizeB / 2).x,abs(boxsizeA /2).y + abs(boxsizeB / 2).y,abs(boxsizeA /2).z + abs(boxsizeB / 2).z只要一个大于等于两个盒子半长相加则不重叠
# Sphere:球心距离小于两个半径相加，即碰撞
# AABBSphere:一个球，一个AABB，球心位置与AABB盒的位置相减，再clamp(-boxsize,boxsize),AABBCentr - clamp即为P（AABB盒上相距圆心最近的点），小于半径即碰撞。
# OBB(Oriented Bounding Box):能够旋转的AABB包围盒
## 原理：如果能找到一个轴，两个凸形状在该轴上的投影不重叠，则这两个形状不相交。如果这个轴不存在，并且那些形状是凸形的，则可以确定两个形状相交(凹形不适用，比如月牙形状，即使找不到分离轴，两个月牙形也可能不相交)
## SAT：将一束平行光照射到物体上，如果在任何角度的照射下，两个物体的影子都会重合，那么这两个物体一定相交
## SAT实现：在所有物体的所有边做投影，如果其中存在任何一个边上的两个物体的投影不重合，那么这两个物体一定是分离的。相反，如果所有投影轴上的投影都是重合的，那么这两个物体一定是重合的

## 6个box的平行面，每个Ax cross Bx Ax cross By Ax cross Bz, Ay cross Bx Ay cross By Ay cross Bz, Az cross Bx Az cross By Az cross Bz ,看是否有重叠

## box 与 sphere：将圆心位置转化为obb的坐标系获得圆心与obb box的最近点，再与圆心比较

# 射线碰撞检测
## box:ray_pos（射线起始位置），box最大，最小的位置，减去ray_pos，求出临界的模m，在用pos + m * dir,判断是否在box内,x,y,z多长的比例才能让射线与min的X相等。
## 球：圆心与起始位置与direction的投影，pos + 投影长度 * dir，是否在球内

# 碰撞检测与光追加速算法

## BVH：通过分配，叶子节点有元素，其他节点为包围盒

## 八叉树：

# BLIPHONE是着色模型



# 光线追踪

## 光线投射：从相机发出一根射线，打到物体上的点（最近的点），再连接该点与光源,如果该点不在阴影中，即为一条光路，在这条光路的折射以及反射光路到达的点，如果与光源连线与物体没有交集，可以确定一条光路，将这些的所有着色加到这个像素点![image-20220425223049394](Picture\RayTracing.png)

## 交点

## 射线方程带入面的方程

## <font color="red">一个点是否在封闭物体内，一个点发射一条射线，与面有奇数个交点在物体内，偶数个交点在物体外</font>

## plane equation![image-20220425225604161](C:\Users\SunHe\AppData\Roaming\Typora\typora-user-images\image-20220425225604161.png)

## ![image-20220425230106889](Picture\重心坐标.png)

### t >= 0, b1, b2 ,1- b1 - b2均为非负即在平面内

## ray与AABB intersaction，t enter < texit && texit >= 0,![image-20220425231914151](Picture\相交.png)

### 这里的t为时间

## 加速方法

### grid method：均匀划分

### 空间划分：

+ 八叉树：三维切三刀分为八叉树，二维是四叉树，每个节点再切三刀，当有足够少的物体就停下
+ KD-Tree: 每次划一刀，先x 后y，z，交替，二叉树，实际物体只在叶子节点
+ 