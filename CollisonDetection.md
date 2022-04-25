# AABB（Axis-aligned Bounding Box）轴对齐碰撞
## 碰撞箱与场景的坐标轴平行
## 三维实现原理：两个三维物体的中心位置向量相减的向量的三轴投影与abs(boxsizeA /2).x + abs(boxsizeB / 2).x,abs(boxsizeA /2).y + abs(boxsizeB / 2).y,abs(boxsizeA /2).z + abs(boxsizeB / 2).z只要一个大于等于则不重叠
# Sphere:球心距离小于两个半径相加，即碰撞
# AABBSphere:一个球，一个AABB，球心位置与AABB盒的位置相减，再clamp(-boxsize,boxsize),AABBCentr - clamp即为P（AABB盒上相距圆心最近的点），小于半径即碰撞。
# OBB(Oriented Bounding Box):能够旋转的AABB包围盒
## 原理：如果能找到一个轴，两个凸形状在该轴上的投影不重叠，则这两个形状不相交。如果这个轴不存在，并且那些形状是凸形的，则可以确定两个形状相交(凹形不适用，比如月牙形状，即使找不到分离轴，两个月牙形也可能不相交)
## SAT：将一束平行光照射到物体上，如果在任何角度的照射下，两个物体的影子都会重合，那么这两个物体一定相交
## SAT实现：在所有物体的所有边做投影，如果其中存在任何一个边上的两个物体的投影不重合，那么这两个物体一定是分离的。相反，如果所有投影轴上的投影都是重合的，那么这两个物体一定是重合的