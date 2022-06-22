![Whitted-Styled-RayTracing](..\Picture\WhittedStyleRayTracing.png)

## 求像点的世界坐标，像素坐标系: x:0 - 800, y:0 - 700转换到NDC[-1, 1]，再从正方型变为aspectratio，通过fov以及imageplane固定在<font color="red">相机坐标系的-1</font>(cameraspace),之后将cameraspace转换到worldspace的矩阵乘这个向量即可，在世界坐标系求交，如果过camera在世界坐标原点，-z同向不用乘。