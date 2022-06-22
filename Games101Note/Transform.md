# 基于homework 1 加强transform的理解

### 使用齐次坐标目的是为了讲位移矩阵也可以用 Ax = B这种进行线性表示

## ortho-projection matrix推导![orthoproj](..\Picture\Mortho-project.jpg)

+ z为变量，znear zfar为常量

## ortho matrix的推导，将坐标变换为[-1， 1]^ 3![ortho](..\Picture\ortho.jpg)

## 结果

![result](..\Picture\result.png)

## <font color="red">提高部分</font>，绕任意过原点轴旋转

### 坐标轴先绕x轴旋转beta角度

### 由于向量长出不变，绕x旋转x坐标不变，在xoz平面y = 0，长度为

$$
\sqrt {a^2 + b^2 + c^2}
$$

### 求出在xoz上坐标再转alpha到z轴在旋转seta角度，之后再乘逆即可获得坐标

![绕任意轴旋转](..\Picture\旋转.jpg)

![旋转10度](..\Picture\旋转10度.png)

### <font color="red">绕(0, 1, 0)旋转逆时针旋转10度。</font>

# 对坐标系统的理解

- 局部空间(Local Space，或者称为物体空间(Object Space))
- 世界空间(World Space)
- 观察空间(View Space，或者称为视觉空间(Eye Space))
- 裁剪空间(Clip Space)
- 屏幕空间(Screen Space)

## 物体坐标系通过平移旋转，转换到世界坐标，之后从世界坐标转换到相机坐标维原点，观察空间（右手坐标系），再之后projection转换为clipspace，裁剪空间（将锥体外的物体删除），已经完成透视投影，并将坐标系转换为NDC（左手坐标系），用scale + 平移完成，然后平移x + 1, y + 1,再用scale扩大到约定的分辨率，<font color="red">将转换到裁剪空间中的坐标，在NDC中坐标绝对值大于1的剔除，或者在变为NDC之前的x，y，z绝对值大于w的剔除</font>
