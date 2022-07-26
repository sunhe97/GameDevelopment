# Light Propagation Volumes(LPV)

## LPV将场景分为3D的网格，发射Radiance

### step1 检测场景中的的直接被照亮的，通过RSM可以直接找到直接照亮的像素，获得虚拟的光源

### step2 注入，将场景分为格子后，找到包围虚拟光源的各自，将各个方向的radiance加起来，并用二阶SH表示每个格子四面八方的初始值，在格子中所有的radiance都一致

### step3传播，那个格子向周围六个面进行传播，之后再相加再传播，迭代4-5次，次级光源也和RSM一样不考虑visibility

### step4 get the incident radience in the  grid cell (from all directions),着色

## 问题会导致lightleaking，作为一个格子中物体背面与被直接照射的正面同时作为次级光源，发光，会导致lightleaking

# VXGI（Voxel Gobal IIIumination）,two-pass algorithm

## 第一步：将场景全部分为三维的小格子（hierarchical）有小有大，物体表面做小格子，上面进行分级。

## 第二步：从camera出发，射到小块后向周围锥形发散，分八个圆锥，从每个shadingpoint 的出射方向发一个圆锥，找到reflect light的物体的radiance求和。

# Screen Space Ambient Occlusion(SSAO)

## 增强相对位置的感觉

## 假设任何一个方向来的radiance是<font color="red">常量</font>，但是不一定全接收到，<font color="red">不同的visibility，全是diffuse物体</font>要考虑来自不同方向的可见性在该着色点，其实下面公式表示就是间接光照

## ![AO原理](..\Picture\AO原理.png)

![AO2](..\Picture\AO2.png)

## AO：在一定范围内判断是否被遮挡

## SSAO：using z-buffer 在一个球中进行采样，采样点与其所在像素的depthbuffer进行比较 ，<font color="red">从着色点能否看到点当被挡住的采样点占总采样点个数过半，才考虑SSAO</font>，只要超过就遮挡，camera到采样点的距离，距离大于像素的depth，即被挡住，用该像素的颜色去计算间接光照。

## HBAO：采样法线的半球，求cosa的加权，在法线的半球（确定半径内部的点才比较depth，超过半径距离，不要），<font color="red">考虑depth的差距。</font>

## SSDO（Screen Space Directional Occlusion）：

![SSDO1](..\Picture\SSDO1.png)

### SSAO假设所有的间接光照来源非常远的地方，SSDO假设所有的间接光照来源于一个小范围内，能不能被挡住从选的像素xy的depth比随机出来的depth比较，看能否看见，然后通过normal看贡献，看成一个小片去看贡献

![SSDO2](..\Picture\SSDO2.png)

## Screan Space Reflection：屏幕空间的光线追踪

### 对depth进行mipmap，存四个像素中的最小深度值，在level++上的图走一个格

![SSR1](..\Picture\SSR1.png)

### SSR将deph做minz的金字塔，level0为最精细的，level1为四个归为一个，检测到有比depth大的点去相应的下一级mipmap去找，还是假设次级光源diffuse，因为不知道次级光源反射的光去哪个方向，次级光源也想RSM一样用depth buffer中的着色结果去射radiance。

![SSR2](..\Picture\SSR2.png)
