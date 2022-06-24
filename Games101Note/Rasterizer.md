![with MSAA](..\Picture\with MSAA.png)![Rasterizer](..\Picture\no MSAA.png)

## 抗锯齿

+ MSAA，通过采样的方法
+ TAA，通过复用上一帧的结果与当前做混合
+ FXAA，通过后处理的方法，将锯齿一次性去掉

## 深度值的矫正，深度值在经过MVP变换后会增大，所以，要将screen space的深度值转换到view space，不知depth，纹理的插值等都需要矫正

$$
{b_0}/{z_0} = alpha * {b_1}/{z_1} + beta * {b_2}/{z_2} + gamma * {b_3}/{z_3}
$$

### b0：正确的插值结果，z0正确的depth结果，alpha、beta、gamma在屏幕空间的重心坐标，b1，b2，b3三角形顶点的属性，z1，z2，z3三角形在view space的深度值（真正的）

$$
1 / z0 = alpha * 1 / z1 + beta * 1 / z2 + gamma * 1 / z3 
$$



