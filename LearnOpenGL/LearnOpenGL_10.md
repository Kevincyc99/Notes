# LearnOpenGL

## Part II	Lighting

### 13	Basic Lighting

<font color = 'green'>冯氏光照模型(Phong lighting model)</font>主要由三个部分组成：环境(ambient)、漫反射(diffuse)、镜面(specular)光照。

<font color = 'green'>环境光照(Ambient Lighting)</font>：它将永远给物体一些颜色。

<font color = 'green'>漫反射光照(Diffuse Lighting)</font>：模拟光源对物体的方向性影响(Directional Impact)。物体面向光源的越多，该光照越亮。

<font color = 'green'>镜面光照(Specular Lighting)</font>：模拟高光。通常更偏向光的颜色。

#### Ambient lighting

若考虑光的反射和散射，称为<font color = 'green'>全局光照(Global Illumination，GI)</font>。

为了实现环境光照，我们只需要用光的颜色乘上一个常量的环境因子，再乘以物体的颜色。

```c++
void main()
{
    float ambientStrength = 0.1f;	//环境因子
    vec3 ambient = ambientStrength * lightColor;
    
    vec3 result = ambient * objectColor;
    FragColor = vec4(result, 1.0f);
}
```

#### Diffuse lighting

我们通常用<font color = 'green'>法向量(Normal Vector)</font>来测量光线与片段之间的角度。

通过光线向量和法向量的点乘(需要是两个单位向量)，得到的标量代表了光线对于片段的影响



![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/12_Diffuse_Lighting.png)



因此，为了计算漫反射光照，需要：

​	法向量：垂直于顶点表面的向量。

​	定向的光线，为了计算该光线，需要光源的位置和片段的位置向量。

1.为了添加法向量，需要向顶点数据中传递数据，并在光照的顶点着色器中添加数据。由于所有的光照都是在片段着色器中进行,因此需要把法向量由顶点着色器传入片段着色器中；

2.为了计算漫反射光照，需要将光源的位置传入片段着色器中，并在渲染循环中更新uniform。

3.还需要得到片段的位置，可以通过顶点属性乘以模型矩阵，得到世界坐标。最后在片段着色器中添加该输入。

4.通过光源的位置减片段的位置，得到光的方向(<font color = 'blue'>注：该方向并非是光照射的朝向，为了计算角度θ，用的是实际光的反向</font>)

这样，我们就可以得到漫反射的光照，它会随着光源与物体表面法向量角度的改变而改变物体表面的颜色。



由于片段着色器中的计算都是在世界空间下进行的，我们需要把法向量也转到世界空间里，但是由于法向量只是一个方向向量，没有齐次坐标，因此，如果用法向量乘以一个模型矩阵，只选模型矩阵左上角的3x3矩阵。

为了解决不等比缩放的问题，用一个专门针对法向量的<font color = 'green'>法线矩阵(Normal Matrix)</font>。其定义为模型矩阵左上角(3x3)的逆矩阵的转置矩阵。

```c++
Normal = mat3(transpose(inverse(model))) * aNormal;
```

<font color = 'red'>矩阵求逆对于着色器开销很大,因此应该尽量避免。最好在绘制之前，在CPU中计算好逆矩阵，再通过uniform把值传给着色器。</font>

#### Specular Lighting

<font color = 'green'>镜面光照(Specular Lighting)</font>是基于光的反射特性。

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/13_Specular_Lighting.png)

当视角与光源方向的反射向量角度越小(θ越小)，镜面高光就越大。

我们选择在世界空间进行光照计算，但是大多数人选择在观察空间进行光照计算，因为这样可以直接获取观察者的位置(0, 0, 0)，但是在世界空间中计算光照更符合直觉。

这样，我们还需要得到观察者的世界空间坐标。

```c++
//在片段着色器中添加uniform
uniform vec3 viewPos;
------------------------------------
//在渲染循环中传值
lightingShader.setVec3("viewPos", camera.Position);
```

```c++
float specularStrength = 0.8f;	//镜面强度
vec3 viewDir = normalize(viewPos - FragPos);	//视线方向向量
vec3 reflectDir = reflect(-lightDir, norm);		//reflect函数第一个参数是从光源指向片段，因此lightDir取反，第二个参数是法向量
float spec = pow(max(dot(viewDir, reflectDir), 0.0), 32);
//先计算视线与反射方向的点乘，并确保不是负值，再取32次幂，该数值为反光度
vec3 specular = specularStrength * spec * lightColor;
```

高光的<font color = 'green'>反光度(Shininess)</font>是指一个物体的反光度越高，反射光的能力越强，散射越少，高光点也会越小。

最终将环境光、漫反射以及镜面反射叠加，得到冯氏光照的完整效果。

```c++
vec3 result = (ambient + diffuse + specular) * objectColor;
FragColor = vec4(result, 1.0f);
```



在早期，开发者曾经在顶点着色器中实现冯氏光照模型，其优点是顶点少，效率高；

但是其问题是计算得到的颜色仅仅是顶点的颜色，而片段的颜色由插值得到，结果光照会显得不太真实。

该着色采用的是Gouraud着色，与之相比，冯氏着色能产生更加平滑的光照效果。