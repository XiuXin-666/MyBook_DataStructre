# 特殊矩阵的压缩存储



## &emsp;&emsp;数组的存储结构

&emsp;&emsp;一个数组中的所有元素在内存中占用一段连续的存储空间。

&emsp;&emsp;对于多维数组，有两种映射方法：按行优先，按列优先。以二维数组为例：按行优先存储的基本思想是：先行后列，先存储行号较小的元素，行号相等先存储列号较小的元素。



## &emsp;&emsp;矩阵的压缩存储

&emsp;&emsp;压缩存储：指为多个值相同的元素只分配一个存储空间，对零元素不分配存储空间。目的是为了节省存储空间。

&emsp;&emsp;特殊矩阵：指具有许多相同矩阵的元素或零元素，并且这些相同矩阵沿元素或零元素的分布有一定规律性的矩阵。常见的特殊矩阵有 对称矩阵，上（下）三角矩阵，对角矩阵。



### &emsp;&emsp;1、对称矩阵

&emsp;&emsp;![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/对称矩阵的压缩存储.png)

&emsp;&emsp;上三角区的所有元素和下三角区的对应元素相同，若仍采用二维数组存储，则会浪费几乎一半的存储空间，所以我们存放在一个一维数组B[n(n+1)/2]中，即元素a(i,j) 存放在b(k)中。

&emsp;&emsp;元素a(i,j)在数组b中的下标k = 1 + 2 + ... + (i - 1) + ( j - 1) = i (i-1) / 2 + j - 1 (数组下标从0 开始)。

&emsp;&emsp;因此，元素下标之间的对应关系为：
$$
k=\left\{\begin{aligned}i(i-1)/2 + j - 1 && i >= j(下三角区和主对角线元素)
					\\j(j-1)/2 + i - 1 && i < j (上三角区元素a~i,j~ = a~j,i~)\end{aligned}\right.
$$

### &emsp;&emsp;2、三角矩阵

![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/上三角矩阵.jpg)

&emsp;&emsp;上三角矩阵中，下三角区的所有元素均为同一常量，一般为0。只需存储主对角线、上三角区上的元素和下三角区的常量一次，可将其压缩存储B[n(n+1)/2 + 1]中。

&emsp;&emsp;因此，元素下标之间的对应关系为：
$$
k=\left\{\begin{aligned}(i-1)(2n-i+2)/2 + j - 1 && i <= j(上三角区和主对角线元素)
					\\n(n+1)/2 && i < j (下三角区元素)\end{aligned}\right.
$$
![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/上三角矩阵压缩存储.jpg)

&emsp;&emsp;下三角矩阵：

![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/下三角矩阵.jpg)

&emsp;&emsp;下三角矩阵的存储思想与对称矩阵类似，不同之处在于存储完下三角区和主对角线上元素之后，紧接着存储对角线上方的常量一次。

&emsp;&emsp;因此，元素下标之间的对应关系为：
$$
k=\left\{\begin{aligned}i(i-1)/2 + j - 1 && i >= j(下三角区和主对角线元素)
					\\n(n+1)/2 && i < j (上三角元素)\end{aligned}\right.
$$


### &emsp;&emsp;3、三对角矩阵

&emsp;&emsp;对角矩阵也称带状矩阵。对于n阶方阵A中的任意元素aij，当|i-j| > 1 时，有aij = 0，则称为三对角矩阵。

&emsp;&emsp;&emsp;&emsp;![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/三对角矩阵.jpg)

&emsp;&emsp;采用压缩存储，将3条对角线上的元素按行优先存放在一维数组B中，且将a0,0 存放在B[0]中，这样就可以计算出三角线上元素 aij（ 0<= i, j <= n, | i - j| <= 1 ) 在一维数组B中存放的下标为 k = 2i + j - 3。



## &emsp;&emsp;稀疏矩阵

&emsp;&emsp;如果矩阵中的非零元素的个数相对整个矩阵的元素个数来说非常少，则将这样的矩阵称为稀疏矩阵。

&emsp;&emsp;存储稀疏矩阵，我们一般构造三元组存储矩阵非零元素。系数矩阵压缩存储后便失去了随机存取的特性。

![](https://xiuxin-1304803037.cos.ap-shanghai.myqcloud.com/稀疏矩阵.png)