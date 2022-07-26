![BVHAccelerating](..\Picture\BVHAccelerating.png)

## BVH 相较于八叉树（空间切两刀），KD-Tree(每次在空间中选择一个方向交替的在分割线对场景进行分割)，BVH采用物体作为分割形成包围盒，避免了一个物体在两个包围盒中，在不同叶子节点中有多个相同物体

## BVH建立的过程

```c++
BVHBuildNode* recursiveBuild(std::vector<Object*> objects)
{
    BVHBuildNode* node = new BVHBuildNode();
    if(object.size == 1)
    {
        node->bounds = object[0]->bounds;
        node->object = object[0];
        node->left = nullptr;
        node->right = nullptr;
        return node;
    }
    else if(object.size == 2)
    {

        node->left = recurseBuild(objects);//递归调用构造函数，因为只有两个节点不用分x,y,z三个轴去选择，直接分
        node->right = recurseBuild(objects);
        node->bounds = Union(node->left->bounds, node->right->bounds);
        return node;
    }
	else
    {
        Bounds3 centroidBounds;
        for(int  i = 0; i < objects.size(); ++i)
            centroidBounds = Union(centroidBounds, objects[i]->getBounds.Centroid());
        int dim = centroidBounds.MaxExtent();//找这个总包围盒中最长的轴
        switch(dim)//按最长的轴对物体进行排序
        {
            case 0:
                std::sort(objects.begin(), objects.end(), [](auto f1, auto f2){
                    return f1->getBounds().Centroid().x <
                        	f2->getBounds().Centroid().x;
                });
                break;
            case 1:
                std::sort(objects.begin(), objects.end(), [](auto f1, auto f2){
                    return f1->getBounds().Centroid().y <
                        	f2->getBounds().Centroid().y;
                });
                break;
            case 2:
                std::sort(objects.begin(), objects.end(), [](auto f1, auto f2){
                    return f1->getBounds().Centroid().z <
                        	f2->getBounds().Centroid().z;
                });
                break;
        }
		auto beginning = objects.begin();
        auto middling = objects.begin() + objects.size() / 2;
        auto endding = objects.end();
        auto leftshapes = std::vector<Object*>(beginning, middling);
        auto rightshapes = std::vector<Object*>(middling, endding);
        node->left = recurseBuild(leftshapes);
        node->right = recurseBuild(rightshapes);
        node->bounds = Union(node->left->bounds, node->right->bounds)
        
        
    }
    return node;
}

```

## 与BVH的求交也是递归一直到叶子节点返回，并返回左右节点距离最近的元素



# SAH

## 排完序后不直接使用中间作为分割，而是去遍历整个size，找（表面积/总面积）* 物体个数（左） + （表面积/总面积）* 物体个数（右）最小的index 

```c++
 if(SAH){
            // 递归分离节点
            auto size = objects.size();
            int proper_cut = 0;
            double mintime = 0x3f3f3f;
            for(int index=0; index<size; index++){
                middling = objects.begin() + index;
                auto leftshapes = std::vector<Object*>(beginning, middling);
                auto rightshapes = std::vector<Object*>(middling, ending);
 
                assert(objects.size() == (leftshapes.size() + rightshapes.size()));
 
                Bounds3 leftBounds,rightBounds;
                //     time = S_1面积 /S_0面积 *S_1空间物体数 * t_obj    
                //              + S_2面积 /S_0面积 *S_2空间物体数 * t_obj 
                for (int i = 0; i < leftshapes.size(); ++i)
                    leftBounds =
                        Union(leftBounds, leftshapes[i]->getBounds().Centroid());
                for (int i = 0; i < rightshapes.size(); ++i)
                    rightBounds =
                        Union(rightBounds, rightshapes[i]->getBounds().Centroid());
                
                auto leftS = leftBounds.SurfaceArea();
                auto rightS = rightBounds.SurfaceArea();
                auto S = leftS + rightS;
                auto time = leftS / S * leftshapes.size() + rightS / S * rightshapes.size();
                if(time<mintime){
                    mintime = time;
                    proper_cut = index;
                }
            }
            middling = objects.begin() + proper_cut;
        }

```

