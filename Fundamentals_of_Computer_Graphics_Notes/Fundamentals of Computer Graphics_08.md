# Fundamentals of Computer Graphics, Fourth Edition

## 08	The Graphics Pipeline

### 8.2	Operations Before and After Rasterization

#### 8.2.3	Using a z-Buffer for Hidden Surfaces (读完)

The z-buffer algorithm is implemented in the fragment blending phase (片元混合).

The z buffer is initialized to the maximum depth.

Z buffer通常用正整数存储
$$
\Delta z = (f - n) / B
$$
B = 2<sup>b</sup>，b为bits，

为了让z小，1）拉近f和n的距离

​					 2）增加b