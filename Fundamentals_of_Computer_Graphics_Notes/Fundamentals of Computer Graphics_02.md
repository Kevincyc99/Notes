# Fundamentals of Computer Graphics, Fourth Edition

## 02 Miscellaneous Math



### 2.4 Vectors（已读完）

Vectors can represent offsets.

#### 2.4.1 Vector Operations

#### 2.4.2 Cartesian Coordinates of a Vector

#### 2.4.3 Dot Product

The most common use of the dot product in graphics programs is to compute the cosine of the angle between two vectors.（求夹角的cos）

The dot product can also be used to find the projection of one vector onto another.（求投影）

#### 2.4.4 Cross Product

The magnitude ∥**a**×**b**∥ is equal to <u>the area of the parallelogram formed by vectors **a** and **b**</u>.

#### 2.4.5 Orthonormal Bases and Coordinate Frames

global coordinate system 基坐标和原点(origin)不会显示(explicit)存储

local coordinate system	基坐标和原点需要显示存储在全局坐标系中的坐标

#### 2.4.6 Constructing a Basis from a Single Vector

通过一个向量**a**构建一组基，先得到**w**，再取一个不与**w**共线的**t**，构建**u**


$$
\vec{w} = \frac{\vec{a}}{||\vec{a}||}
\\
\vec{u} = \frac{\vec{t}\times\vec{w}}{||\vec{t}\times\vec{w}||}
\\
\vec{v} = \vec{w}\times\vec{u}
$$

#### 2.4.7 Constructing a Basis from Two Vectors

通过不平行的向量**a**、**b**构建一组基
$$
\vec{w} = \frac{\vec{a}}{||\vec{a}||}
\\
\vec{u} = \frac{\vec{b}\times\vec{w}}{||\vec{b}\times\vec{w}||}
\\
\vec{v} = \vec{w}\times\vec{u}
$$

#### 2.4.8 Squaring Up a Basis

