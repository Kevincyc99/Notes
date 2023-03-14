# LearnOpenGL

## Part II	Lighting

### 16	Light casters

将光<font color = 'green'>投射(cast)</font>到物体的光源叫<font color = 'green'>透光物(Light Caster)</font>。

#### Directional Light(定向光)

假设光源处于无限远处，其每条光线可以近似于互相平行，不论物体或者观察者的位置，这种光源被称为定向光。因为它的所有光线都有着相同方向，它与光源的位置没有关系。

定向光最典型的例子就是太阳。

我们可以通过定义一个光线方向向量而不是位置向量来模拟一个定向光。

```c++
//片段着色器
struct Light{
    vec3 direction;
    
    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
};
void main()
{
    vec3 lightDir = normalize(-light.direction);//取反，获得一个从片段指向光源的向量
}
```

#### Point Lights(点光源)

点光源是处于世界中某一位置的光源，它会朝着所有方向发光，但光线也会随着距离逐渐衰减。

#### Attenuation

随着光线传播距离降低光的强度通常被称为<font color = 'green'>衰减(attenuation)</font>。
$$
F_{att} = \frac{1.0}{K_c + K_l * d + K_q * d^2}
$$
d表示片段与光源的距离，Kc、Kl、Kq分别为常数项、一次项和二次项。

常数项通常保持为1.0，用来保证分母大于1。