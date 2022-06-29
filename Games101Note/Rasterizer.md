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



## 光栅化过程，对于每个图形的Bounding 区域，在屏幕空间，i + 0.5， j+ 0.5，转换到是否在屏幕空间三角形顶点的内部，叉积判断，如果再并且如果当前像素对应的depth深度比depthbuffer中的小，更新depthbuffer和framebuffer中的颜色。

## 因为三角形的面是线性的，通过重心坐标根据三个顶点的z插值出每个像素对应的depth（在相机坐标中的depth），所以将屏幕空间的z进行变换，再用相机空间的depth进行插值。

## MSAA即为在一个像素多次采样，获得颜色，进行抗锯齿。

## TAA，在时间轴上，复用上一帧的结果，且每一帧的采样点变换

## FXAA，检测边缘，后处理技术处理锯齿
