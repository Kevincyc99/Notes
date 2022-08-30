# LearnOpenGL

## Part II	Lighting

### 14	Materials

由于不同材质的物体往往会有不同的光照效果，通过建立材质结构体，我们可以批量化地定义出不同的效果。

```c++
//Fragment Shader
#version 330 core
struct Material {
    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
    float shininess;
}; 

uniform Material material;
```

![](https://github.com/Kevincyc99/Images-Store/raw/main/LearnOpenGL/Notes/14_Materials.png)



| 材质名称          | 环境参数 |          |          | 漫反射参数 |            |            | 镜面强度   |            |            | 反光度     |
| ----------------- | -------- | -------- | -------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| 绿宝石(emerald)   | 0.0215   | 0.1745   | 0.0215   | 0.07568    | 0.61424    | 0.07568    | 0.633      | 0.727811   | 0.633      | 0.6        |
| jade              | 0.135    | 0.2225   | 0.1575   | 0.54       | 0.89       | 0.63       | 0.316228   | 0.316228   | 0.316228   | 0.1        |
| obsidian          | 0.05375  | 0.05     | 0.06625  | 0.18275    | 0.17       | 0.22525    | 0.332741   | 0.328634   | 0.346435   | 0.3        |
| pearl             | 0.25     | 0.20725  | 0.20725  | 1          | 0.829      | 0.829      | 0.296648   | 0.296648   | 0.296648   | 0.088      |
| 红宝石(ruby)      | 0.1745   | 0.01175  | 0.01175  | 0.61424    | 0.04136    | 0.04136    | 0.727811   | 0.626959   | 0.626959   | 0.6        |
| 绿松石(turquoise) | 0.1      | 0.18725  | 0.1745   | 0.396      | 0.74151    | 0.69102    | 0.297254   | 0.30829    | 0.306678   | 0.1        |
| brass             | 0.329412 | 0.223529 | 0.027451 | 0.780392   | 0.568627   | 0.113725   | 0.992157   | 0.941176   | 0.807843   | 0.21794872 |
| bronze            | 0.2125   | 0.1275   | 0.054    | 0.714      | 0.4284     | 0.18144    | 0.393548   | 0.271906   | 0.166721   | 0.2        |
| 铬(chrome)        | 0.25     | 0.25     | 0.25     | 0.4        | 0.4        | 0.4        | 0.774597   | 0.774597   | 0.774597   | 0.6        |
| copper            | 0.19125  | 0.0735   | 0.0225   | 0.7038     | 0.27048    | 0.0828     | 0.256777   | 0.137622   | 0.086014   | 0.1        |
| gold              | 0.24725  | 0.1995   | 0.0745   | 0.75164    | 0.60648    | 0.22648    | 0.628281   | 0.555802   | 0.366065   | 0.4        |
| silver            | 0.19225  | 0.19225  | 0.19225  | 0.50754    | 0.50754    | 0.50754    | 0.508273   | 0.508273   | 0.508273   | 0.4        |
| black plastic     | 0.0      | 0.0      | 0.0      | 0.01       | 0.01       | 0.01       | 0.50       | 0.50       | 0.50       | .25        |
| cyan plastic      | 0.0      | 0.1      | 0.06     | 0.0        | 0.50980392 | 0.50980392 | 0.50196078 | 0.50196078 | 0.50196078 | .25        |
| green plastic     | 0.0      | 0.0      | 0.0      | 0.1        | 0.35       | 0.1        | 0.45       | 0.55       | 0.45       | .25        |
| red plastic       | 0.0      | 0.0      | 0.0      | 0.5        | 0.0        | 0.0        | 0.7        | 0.6        | 0.6        | .25        |
| white plastic     | 0.0      | 0.0      | 0.0      | 0.55       | 0.55       | 0.55       | 0.70       | 0.70       | 0.70       | .25        |
| yellow plastic    | 0.0      | 0.0      | 0.0      | 0.5        | 0.5        | 0.0        | 0.60       | 0.60       | 0.50       | .25        |
| black rubber      | 0.02     | 0.02     | 0.02     | 0.01       | 0.01       | 0.01       | 0.4        | 0.4        | 0.4        | .078125    |
| cyan rubber       | 0.0      | 0.05     | 0.05     | 0.4        | 0.5        | 0.5        | 0.04       | 0.7        | 0.7        | .078125    |
| green rubber      | 0.0      | 0.05     | 0.0      | 0.4        | 0.5        | 0.4        | 0.04       | 0.7        | 0.04       | .078125    |
| red rubber        | 0.05     | 0.0      | 0.0      | 0.5        | 0.4        | 0.4        | 0.7        | 0.04       | 0.04       | .078125    |
| white rubber      | 0.05     | 0.05     | 0.05     | 0.5        | 0.5        | 0.5        | 0.7        | 0.7        | 0.7        | .078125    |
| yellow rubber     | 0.05     | 0.05     | 0.0      | 0.5        | 0.5        | 0.4        | 0.7        | 0.7        | 0.04       | .078125    |

为了达到更加准确的材质效果，我们还需要用更复杂的形状对立方体进行替换。

#### Setting materials

```c++
//片段着色器中
void main()
{    
    // 环境光
    vec3 ambient = lightColor * material.ambient;

    // 漫反射 
    vec3 norm = normalize(Normal);
    vec3 lightDir = normalize(lightPos - FragPos);
    float diff = max(dot(norm, lightDir), 0.0);
    vec3 diffuse = lightColor * (diff * material.diffuse);

    // 镜面光
    vec3 viewDir = normalize(viewPos - FragPos);
    vec3 reflectDir = reflect(-lightDir, norm);  
    float spec = pow(max(dot(viewDir, reflectDir), 0.0), material.shininess);
    vec3 specular = lightColor * (spec * material.specular);  

    vec3 result = ambient + diffuse + specular;
    FragColor = vec4(result, 1.0);
}
//渲染循环中
lightingShader.setVec3("material.ambient",  1.0f, 0.5f, 0.31f);
lightingShader.setVec3("material.diffuse",  1.0f, 0.5f, 0.31f);
lightingShader.setVec3("material.specular", 0.5f, 0.5f, 0.5f);
lightingShader.setFloat("material.shininess", 32.0f);
```

除了材质，我们还需要对光的属性进行调整

```c++
//片段着色器中
struct Light {
    vec3 position;

    vec3 ambient;
    vec3 diffuse;
    vec3 specular;
};

uniform Light light;
void main()
{
	vec3 ambient  = light.ambient * material.ambient;
	vec3 diffuse  = light.diffuse * (diff * material.diffuse);
	vec3 specular = light.specular * (spec * material.specular);
    ...
}
//渲染循环中
lightingShader.setVec3("light.ambient",  0.2f, 0.2f, 0.2f);
lightingShader.setVec3("light.diffuse",  0.5f, 0.5f, 0.5f); // 将光照调暗了一些以搭配场景
lightingShader.setVec3("light.specular", 1.0f, 1.0f, 1.0f); 
```

