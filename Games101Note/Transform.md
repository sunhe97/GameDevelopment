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

