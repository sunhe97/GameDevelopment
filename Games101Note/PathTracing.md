![PathTracing](..\Picture\PathTracing.png)

SPP:4,TIME:3min

![PathTracing512](..\Picture\PathTracing512.png)

![time](..\Picture\time.png)

# Path Tracing

```c++
Vector3f Scene::castRay(const Ray &ray, int depth) const
{
    // TO DO Implement Path Tracing Algorithm here
    float ELISION = 0.01f;
    
    Intersection interp = intersect(ray);
    //射出的光线没有碰撞直接返回0
    if(!interp.happened)
    {
        return Vector3f(0.0f);
    }
    //射出的光线如果过depth == 0 且是发光物体直接返回物体的发射的颜色
    if(interp.m->hasEmission())
    {
        //frome pixel light
        if(depth == 0) 
        {
            return interp.m->getEmission();
        }
        //如果过是多次弹射的光碰到光源，直接返回0
        else return Vector3f(0.0f);
    }
    //直接光照
    Vector3f L_dir = Vector3f(0.0f);
    float pdf_light = 0.0f;
    Intersection interx;
    sampleLight(interx, pdf_light);
    Vector3f wo = ray.direction;
    Vector3f x = interx.coords;
    Vector3f ws = normalize(interx.coords - interp.coords);
    Vector3f N = normalize(interp.normal);
    Vector3f NN = normalize(interx.normal);
    Vector3f emit = interx.emit;
    float normvalue = (x - interp.coords).norm();
    Intersection intertoLight = intersect(Ray(interp.coords, ws));
    //如果发射的光线与光源发生碰撞且中间无遮挡，对光源采样进行着色，采用蒙特卡洛方法
    if(intertoLight.happened && (intertoLight.coords - x).norm() < ELISION)
    {
        L_dir = emit * interp.m->eval(wo, ws, N) 
        * dotProduct(ws, N) 
        * dotProduct(-ws, NN) 
        / std::pow(normvalue, 2) / pdf_light; 
    }
    //使用俄罗斯罗盘赌结束递归
    float ksi = get_random_float();
    if(ksi > RussianRoulette) return L_dir;
	//间接光照，从第一次射到的点继续发射
    Vector3f L_indir = Vector3f(0.0f);
    Vector3f wi = normalize(interp.m->sample(wo, N));
    Intersection interIndir = intersect(Ray(interp.coords, wi));
    //如果碰撞发生且碰撞的不是发光物体进行间接光照计算
    if(interIndir.happened && !interIndir.m->hasEmission())
    {
        //其他物体射来的光 castRay(Ray(interp.coords, wi), depth + 1)计算渲染方程
        L_indir = castRay(Ray(interp.coords, wi), depth + 1) * interp.m->eval(wo, wi, N) * dotProduct(wi, N) / interp.m->pdf(wo, wi, N) / RussianRoulette;
    }
    return L_dir + L_indir;


}


```

