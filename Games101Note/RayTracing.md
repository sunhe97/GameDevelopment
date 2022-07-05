![Whitted-Styled-RayTracing](..\Picture\WhittedStyleRayTracing.png)

## 求像点的世界坐标，像素坐标系: x:0 - 800, y:0 - 700转换到NDC[-1, 1]，再从正方型变为aspectratio，通过fov以及imageplane固定在<font color="red">相机坐标系的-1</font>(cameraspace),之后将cameraspace转换到worldspace的矩阵乘这个向量即可，在世界坐标系求交，如果过camera在世界坐标原点，-z同向不用乘。

## NDC是左手，framebuffer是左上角为0，0坐标，NDC是右手坐标系的x，y所以y要（1 - ）才能变换到NDC，之后切换到近平面大小比例，再变换到相机坐标系，获得相机坐标系的坐标，之后（乘camera到world，逆lookat矩阵）转换到世界坐标进行求交