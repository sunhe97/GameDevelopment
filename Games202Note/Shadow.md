![硬阴影](..\Picture\硬阴影.png)

# 硬阴影

## 在着色点到光源的距离 - elsion （不减会出现自遮挡）要小于shadowmap上的值visibility = 1，

## Lootat矩阵

### 由1，0，0，x, y, z转到right，up，front的矩阵的逆即为旋转矩阵，位移为负的相机世界坐标位置，rotaion * translation  = view matrix

# PCF软阴影

![PCF](..\Picture\PCF.png)

## 强制转换float(), 不能有.f

## 着色点距光的距离与采样点在shadowmap上的depth比，大于shadowmap为1

# PCSS

![PCSS](..\Picture\PCSS.png)

## define name 10（不能加分号）

# VSSM

## 在第一步通过设置zuocc = t(shading point depth)，size内的均值方差

$$
N_1/N * Z_{UNOCC} + N_2 / N * Z_{OCC} = Z_{AVG}
$$

$$
N_1 / N 为切比雪夫不等式，P(x > t) 通过SAT求depth^2的和，depth的和求均值和方差，N_2 / N为1- P，求出Z_{AVG}
$$

## 第三步切比雪夫不等式，通过size求均值和方差