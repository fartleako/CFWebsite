---
title: "Transformations"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 10
math: true
---

#### This chapter covers very basic mathematical concepts. If you know how to work with vectors and matrices, you can skip this chapter.

## Vectors and Points

To first understand how computer graphics work we need to understand how we can use our **3D-Space**. \
Imagine you have a point in the space and you want to get to another. Your points always have three coordinates - \\(x,y,z\\). So you want to get from \\((x_1,y_1,z_1)\\) to \\((x_2,y_2,z_2)\\). 
To get to another point we use the difference of the points itself.

{{< math >}}
$$
\vec{v} =

\begin{pmatrix}
  x_2 \\
  y_2 \\
  z_2 
\end{pmatrix}

- 

\begin{pmatrix}
  x_1 \\
  y_1 \\
  z_1 
\end{pmatrix}

= 

\begin{pmatrix}
  x_2 - x_1 \\
  y_2 - y_1 \\
  z_2 - z_1 
\end{pmatrix}
$$
{{< /math >}}

This is more or less a vector in the **3D-Space**. There are also vectors for the 2D space (just with \\(x, y\\) or for any other space).
So a vector is defined as the difference of the components.  

You may have noticed the \\(\vec{v}\\) before the two points. This \\(v\\) with a little arrow indicates a vector. If you dive deeper you will also see quite often that there is no arrow and the \\(v\\) is just bold like this: \\(\bold{v}\\). So don't get confused. Both things mean the same thing. 

Now we defined a vector. To get to another point via a vector we just add this specific vector to the position vector. What is a **position vector**? Well very short that is the vector from the point of origin \\((0,0,0)\\) to the point itself. We use this vector because in mathematics there is no addition or subtraction of points itself. The origin is often marked as \\(O\\). So the position vector of \\(P\\) is then \\(\vec{OP}\\).

Let \\(Q\\) be a new point which can be build by adding \\(\vec{v}\\) to the position vector of \\(P\\). 
{{< math >}}
$$
Q = 
\begin{pmatrix}
  P_x \\
  P_y \\
  P_z 
\end{pmatrix}

+

\begin{pmatrix}
  v_x \\
  v_y \\
  v_z 
\end{pmatrix}

= 

\begin{pmatrix}
  P_x + v_x \\
  P_y + v_y \\
  P_z + v_z 
\end{pmatrix}
$$
{{< /math >}}

 In short: \\(Q = \vec{v} + \vec{OP}\\). So in 2D it could look a bit like this:

 !["https://www.geogebra.org/m/bAnquQpH"](/example_Vector.JPG?height=200px)

#### Multiplication

After we defined addition, we can talk about multiplication. 
Scalar multiplication is defined for vectors. So you can multiply the vector with a scalar (i.e. a factor) \\(f \cdot \vec{v}\\), (\\(f \in R\\)).

If we had a vector and we want to multiply it by the factor of two - it would look like this:
{{< math >}}
$$
2

\cdot 

\begin{pmatrix}
  3 \\
  1 \\
  2 
\end{pmatrix}

= 

\begin{pmatrix}
  2 \cdot 3 \\
  2 \cdot 1 \\
  2 \cdot 2 
\end{pmatrix}

= 

\begin{pmatrix}
  6 \\
  2 \\
  4 
\end{pmatrix}


$$
{{< /math >}}

#### Scalar product or Dot product

There is another product of vectors - it is called scalar or dot product. 

{{< math >}}
$$
\vec{v} \cdot \vec{w} = \langle \vec{v},\vec{w} \rangle = v_x \cdot w_x + v_y \cdot w_y + v_z \cdot w_z
$$
{{< /math >}}

Often it is also written like: 

{{< math >}}
$$
\begin{pmatrix}
  v_x \\
  v_y \\
  v_z 
\end{pmatrix}^T

\cdot 

\begin{pmatrix}
  w_x \\
  w_y \\
  w_z 
\end{pmatrix}
$$
{{< /math >}}

The \\(T\\) stand here for transposed vector of \\(\vec{v}\\) which is the same as:

{{< math >}}
$$
\begin{pmatrix}
  v_x &
  v_y &
  v_z 
\end{pmatrix}

\cdot 

\begin{pmatrix}
  w_x \\
  w_y \\
  w_z 
\end{pmatrix}
$$
{{< /math >}}

So we can conclude that the dot product is the sum of the componentwise multiplication of two vectors. 

#### Length of vectors

Often it is necessary to get the length of a vector. In 2D we used the Pythagorean theorem. We can use the same in 3D just with one coordinate more. Let \\(d\\) be the length of a given vector \\(\vec{v}\\) which is defined through:

{{< math >}}
$$
||\vec{v}|| = \sqrt{v_x^2+v_y^2+v_z^2}
$$
{{< /math >}}

Note that you can not form the length of a point - you can form the length of the position vector or the vector between two points (here \\(Q\\) and \\(P\\)): 

{{< math >}}
$$
\overline{PQ} = \sqrt{(Q_x-P_x)^2+(Q_y-P_y)^2+(Q_z-P_z)^2}
$$
{{< /math >}}

Above we stated that the dot product is the sum of componentwise multiplication. So another way to form the length equation would be the square root of the dot product of a vector with itself: 

{{< math >}}
$$
||\vec{v}|| = \sqrt{\langle \vec{v},\vec{v} \rangle}
$$
{{< /math >}}

### Coordinate system

We have not yet talked about coordinate systems. There is always an origin \\(O\\) and \\(n\\) indepented vectors which are our axes. These are aligned in a 90° angle and if they are normalized it is called a Cartesian coordinate system.

### Affine transformation 

Maybe you have noticed that it does not matter whether you add a vector \\(\vec{v}\\) to a position vector and then a vector \\(\vec{w}\\) or if you add the \\(\vec{w}\\) first and afterwards \\(\vec{v}\\) (\\(\vec{OP} + \vec{v} + \vec{w} = \vec{OP} + \vec{w} + \vec{v}\\)). This property is called affine transformation.

A transformation \\(F\\) is affine if it is interchangeable with further affine transformation: \\(F(\sum_i
 \lambda_i P_i) = \sum_i \lambda_i F (P_i)\\). \
That means that the attributes of every affine transformation are: 
* Are three points collinear, then are the mapped points also collinear
* Every pair parallels stays parallel
* Three collinear points are mapped in this way that the partial ratios stay the same

Every affine transformation can be computed by back-to-back execution of translations, rotations, scales and shears.

## Matrices

A matrix is a rectangular array of elements - you can imagine it like a table. Furthermore a matrix can be described via rows and columns. So a 2x3 matrix has 2 rows and 3 columns. An element of a matrix can be indexed by two numbers - \\((i,j)\\), where \\(i\\) is standing for the row and \\(j\\) stands for the column. \
So a 2x3 matrix would look like this: 

{{< math >}}
$$
\begin{bmatrix}
    2 & 1 & 5 \\
    3 & 0 & 2 
  \end{bmatrix}
$$
{{< /math >}}

So matrices have \\(m \cdot n\\) elements and it is often represented as a uppercase letter - like 'A' but the elements in a matrix are written as lowercase. So the elements in a matrix can be displayed or written like this: 

{{< math >}}
$$
\begin{bmatrix}
    a_{11} & a_{12} & a_{13} \\
    a_{21} & a_{22} & a_{23} \\
    a_{31} & a_{32} & a_{33}
  \end{bmatrix}
$$
{{< /math >}}


### Addition and subtraction

Addition or subtraction is only possible if both matrices have the same number of rows and columns. Then the addition (or subtraction) is performed element wise (\\(c_{ij} = a_{ij} + b_{ij}\\)).

{{< math >}}
$$
\begin{bmatrix}
    0 & 1 \\
    2 & 3  
\end{bmatrix}

+ 
\begin{bmatrix}
    4 & 5 \\
    6 & 7  
\end{bmatrix}

= 
\begin{bmatrix}
    0+4 & 1+5 \\
    2+6 & 3+7  
\end{bmatrix}
=
\begin{bmatrix}
    4 & 6 \\
    8 & 10  
\end{bmatrix}
$$
{{< /math >}}

### Scalar multiplication

We can also multiply a matrix with a number (a scalar value). Then every element of the matrix is multiplied with this value. 

{{< math >}}
$$

\textcolor{blue}{4}

\cdot 

\begin{bmatrix}
    2 & 4 \\
    8 & 16
\end{bmatrix}

= 

\begin{bmatrix}
    \textcolor{blue}{4} \cdot 2 & \textcolor{blue}{4} \cdot 4 \\
    \textcolor{blue}{4} \cdot 8 & \textcolor{blue}{4}\cdot 16
\end{bmatrix}

= 

\begin{bmatrix}
    8 & 16\\
    32 & 64
\end{bmatrix}


$$
{{< /math >}}

### Matrix multiplication

It is possible to multiply two matrices together, the result is also a matrix. But this multiplication is just possible if the matrices are "compatible". That means that the number of columns of the first matrix is the same as the number of rows of the second matrix. 
(\\(A \cdot B \Rarr \textcolor{blue}{m}\\) x \\(\textcolor{red}{n} \cdot \textcolor{red}{n}\\) x \\(\textcolor{blue}{p}\\)). So \\(\textcolor{red}{n}\\) needs to be the same. But you can also get the dimensions of the new matrix from this trick. The new matrix has the dimensions of \\(\textcolor{blue}{m}\\) x \\(\textcolor{blue}{p}\\). 

**But how do we multiply two matrices together?**
The new matrix will be obtained by multiplying each row vector of A with the according column vector of B. \
Let's use the example from above but this time with matrix multiplication. 

{{< math >}}
$$
\begin{bmatrix}
    \textcolor{blue}{0} & \textcolor{blue}{1} \\
    \textcolor{red}{2} & \textcolor{red}{3} 
\end{bmatrix}
\cdot 
\begin{bmatrix}
    \textcolor{964B00}{4} & \textcolor{green}{5} \\
    \textcolor{964B00}{6} & \textcolor{green}{7}  
\end{bmatrix}

= 
\begin{bmatrix}
    \textcolor{blue}{0} \cdot \textcolor{964B00}{4} + \textcolor{blue}{1} \cdot \textcolor{964B00}{6} & \textcolor{blue}{0} \cdot \textcolor{green}{5} + \textcolor{blue}{1} \cdot \textcolor{green}{7} \\
    \textcolor{red}{2} \cdot \textcolor{964B00}{4} + \textcolor{red}{3} \cdot \textcolor{964B00}{6} & \textcolor{red}{2} \cdot \textcolor{green}{5} + \textcolor{red}{3} \cdot \textcolor{green}{7}
\end{bmatrix}
=
\begin{bmatrix}
    6 & 7 \\
    26 & 31  
\end{bmatrix}
$$
{{< /math >}}

As you have seen even this small example takes a bit of effort to calculate. If you need more examples or you need further explanation you can do that [here](https://www.cuemath.com/algebra/solve-matrices/).

Here is a last example you can use to check you newly gained knowledge.

{{< math >}}
$$
\begin{bmatrix}
    2 & 1 & 5\\
    3 & -2 & 1
\end{bmatrix}

\cdot 

\begin{bmatrix}
    -3 & 2 \\
    4 & 8 \\
    2 & 2 
\end{bmatrix}

= ? 
$$
{{< /math >}}

{{%expand "The result is:" %}}
\\(8\\)   \\(22\\) \
\\(-15\\) \\(-8\\)
{{% /expand%}}


## Matrix-Vector multiplication

Due to the fact that a vector is nothing else than a Nx1 matrix (where N is the number of components) we can multiply it with a matrix. This matrix needs to have the dimensons of MxN in order to multiply it with our Nx1 vector. \
Matrices can be used for **transformations** like scaling, translation and rotation of vectors. We just multiply the according matrix with a vector to get the desired result.

### Translation

To easily move a vector we can use a translation matrix. The points are going to be moved by 

{{< math >}}
$$
\vec{v} = \begin{pmatrix} x_0 \\ y_0 \\ z_0 \\ 1\end{pmatrix}

\to 
T = 

\begin{bmatrix}
    1 & 0 & 0 & x_0 \\
    0 & 1 & 0 & y_0 \\
    0 & 0 & 1 & z_0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}
$$
{{< /math >}}

#### Homogeneous coordinates

You may have noticed the matrix is \\(\in \Reals^4\\) (i.e. a 4x4 matrix). But in fact we are still in \\(\Reals^3\\). In the lower most right cell there is a 1. In the lecture before we just needed a single vector (\\(\Reals^3\\)). To use translations with matrices we lift the matrices one dimension higher. The new component in the lower most right corner is called w component. This component is most of the time \\(1\\). If it is not \\(1\\) we can just divide \\(x, y, z, w\\) by \\(w\\) itself. Note that \\(w\\) can not be \\(0\\)!

### Scaling

In order to scale a vector we need to set the values on the main diagonal.

{{< math >}}
$$  
\begin{pmatrix}
   s_x \cdot v_x \\
   s_y \cdot v_y \\
   s_z \cdot v_z \\
   1
\end{pmatrix}

=

  \begin{bmatrix}
    s_x & 0 & 0 & 0 \\
    0 & s_y & 0 & 0 \\
    0 & 0 & s_z & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

\cdot 


\begin{pmatrix}
   v_x \\
   v_y \\
   v_z \\
   1
\end{pmatrix}

$$
{{< /math >}}

With \\(s_x\\) for example we scale the x-component. \
If we set it to 2 - all the x values would have been doubled:

{{< math >}}
$$  
\begin{pmatrix}
   \bold{2} \cdot 3 \\
   1  \\
   1  \\
   1
\end{pmatrix}

=

  \begin{bmatrix}
    \bold{2} & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

\cdot 


\begin{pmatrix}
   3 \\
   1 \\
   1 \\
   1
\end{pmatrix}

$$
{{< /math >}}

### Rotation

To rotate a vector around an axis we need to use rotation matrices. We are in \\(R^3\\) so we need three matrices - one for each axis (x,y,z). We have to specify an angle \\( \varphi \\)

For the x-axis the first row stays the same.

{{< math >}}
$$ R_x = 
  \begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & cos \varphi & - sin \varphi & 0 \\
    0 & sin \varphi & cos \varphi & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}
$$
{{< /math >}}

The sign is flipped with \\(R_y \\) because of the right-hand rule. Thumb is the rotational axis while the index finger is rotated to the middle finger. 

{{< math >}}
$$ R_y = 
  \begin{bmatrix}
    cos \varphi & 0 & sin \varphi & 0 \\
    0 & 1 & 0 & 0 \\
    - sin \varphi & 0  & cos \varphi & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}
$$
{{< /math >}}

So if we would rotate the vector \\( \vec{v} \\) along the z-axis it would look like this: 

{{< math >}}
$$ 
\begin{pmatrix}
   cos \varphi \cdot v_x - sin \varphi \cdot v_y \\
   sin \varphi \cdot v_x + cos \varphi \cdot v_y \\
   z \\
   1
\end{pmatrix}

=

  \begin{bmatrix}
    cos \varphi & - sin \varphi & 0 & 0 \\
    sin \varphi &  cos \varphi & 0& 0 \\
    0 & 0  & 1 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

\begin{pmatrix}
   v_x \\
   v_y \\
   v_z \\
   1
\end{pmatrix}
$$
{{< /math >}}

It is also possible to calculate a rotation in x, y and z with just one matrix. This matrix is very verbose and it is often easier just to calculate one rotation at a time.

If we now want, to calculate a series of transformations we just multiply the matrices in order to combine them.


For example, we have the point \\((3,2,1)\\) and we want to scale it by the factor of 2 and then rotate it 180° along the y-axis, it woul look like this:

{{< math >}}
$$  
  \begin{bmatrix}
    cos (180°) & 0 & sin (180°) & 0 \\
    0 & 1 & 0 & 0 \\
    - sin (180°) & 0  & cos (180°) & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{bmatrix}
    2 & 0 & 0 & 0 \\
    0 & 2 & 0 & 0 \\
    0 & 0 & 2 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{pmatrix}
   3 \\
   2 \\
   1 \\
   1
\end{pmatrix}
$$
{{< /math >}}

which equals to:

{{< math >}}
$$  
  \begin{bmatrix}
    -1 & 0 & 0& 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0  & -1 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{bmatrix}
    2 & 0 & 0 & 0 \\
    0 & 2 & 0 & 0 \\
    0 & 0 & 2 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{pmatrix}
   3 \\
   2 \\
   1 \\
   1
\end{pmatrix}

\hat{=}

\begin{bmatrix}
    -1 & 0 & 0& 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0  & -1 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

\begin{pmatrix}
   6 \\
   4 \\
   2 \\
   1
\end{pmatrix}

\hat{=}

\begin{pmatrix}
   -6 \\
   4 \\
   -2 \\
   1
\end{pmatrix}

$$
{{< /math >}}

Note that the first transformation (i.e. here the scale) is always the outer most right matrix. Matrix multiplicands are not commutative! A good guideline is to read or write the matrices from right to left to get the according transformation. 

In the example below you can see that indeed matrix multiplicans are not commutative.

{{< math >}}
$$ 

  \begin{bmatrix}
    2 & 0 & 0 & 1 \\
    0 & 2 & 0 & 1 \\
    0 & 0 & 2 & 1 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \hat{=}

  \begin{bmatrix}
    1 & 0 & 0 & 1 \\
    0 & 1 & 0 & 1 \\
    0 & 0 & 1 & 1 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{bmatrix}
    2 & 0 & 0 & 0 \\
    0 & 2 & 0 & 0 \\
    0 & 0 & 2 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \neq

  \begin{bmatrix}
    2 & 0 & 0 & 0 \\
    0 & 2 & 0 & 0 \\
    0 & 0 & 2 & 0 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \cdot 

  \begin{bmatrix}
    1 & 0 & 0 & 1 \\
    0 & 1 & 0 & 1 \\
    0 & 0 & 1 & 1 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

  \hat{=}

  \begin{bmatrix}
    2 & 0 & 0 & 2 \\
    0 & 2 & 0 & 2 \\
    0 & 0 & 2 & 2 \\
    0 & 0 & 0 & 1
  \end{bmatrix}

$$
{{< /math >}}

On the left side we scale first and then translate but on the other side we do the opposite.