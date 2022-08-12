# Fundamentals of Computer Graphics, Fourth Edition

## 04	Ray Tracing

3D-rendering:	taking a scene, or model, composed of many geometric objects arranged in 3D space and producing a 2D image that shows the objects as <u>viewed from a particular viewpoint.</u>

<u>rendering is a process that takes its input **a set of objects** and produces as its output **an array of pixels**.</u>

### 4.1	The Basic Ray-Tracing Algorithm

A ray tracer has three parts:

1	ray generation:	compute the origin and direction of each pixel's viewing ray 

2	ray intersection:	find the closest object intersecting the viewing ray

3	shading:	computes the pixel color

The structure of basic ray tracing program:

for each pixel do

​	compute viewing ray

​	find first object hit by ray and its surface normal n

​	set pixel color to value computed from hit point, light and n

### 4.2	Perspective

linear perspective

### 4.3	Computing Viewing Rays(该看)