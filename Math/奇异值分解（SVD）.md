# 奇异值分解（SVD）

Singular Value Decomposition

用处：

- 降维算法中的特征分解

- 用于推荐系统

- 自然语言处理领域





## 1、回顾特征值和特征向量

A x = lambda x

A：n*n的矩阵

x：n维列向量

则lambda为矩阵A的一个特征值，而x为矩阵A的特征值lambda所对应的特征向量



求出**特征值lambda**和**特征向量x**的好处：

可以将矩阵A进行特征分解表示

A = WΣW^-1

其中W是n个特征向量所张成的n*n维矩阵，而Σ是n个特征值为主对角线的n\*n维的矩阵，一般W的这n个特征向量标准化，即满足||Wi||=1，或Wi^T\*Wi = 1，此时W的n个特征向量为标准正交基，满足W^TW = I，即W^T = W^-1，就算是W为酉矩阵。

A = WΣW^T

所有要进行特征分解，矩阵A必须是方阵！



如果不是方阵，行数！=列数时候，使用SVD！



## 2、SVD的定义

SVD也是对矩阵进行分解，SVD不要求为方阵，假设A为m*n的矩阵，那么定义矩阵A的SVD为：

A = UΣV^T

U：m*m矩阵

Σ：m*n矩阵， 除了主对角线上的元素以外全为0，主对角线上每个元素为奇异值

V：n*n矩阵



U和V都是酉矩阵

U^TU = I

V^TV = I

![img](https://pic4.zhimg.com/80/v2-5ee98f8f3426b845bc1c5038ecd29593_720w.jpg)



求解U、Σ、V：



求U：

A^T * A为n*n的矩阵，方阵

A^TA * Vi = lambdai * Vi

得到矩阵A^T*A的n个特征值和n个特征向量，A^T * A的所有特征值向量张成一个n*n的矩阵V，则就SVD中的V。一般V中的每个特征向量为右奇异向量



求V：

A * A^T为m*m的矩阵，方阵

AA^T * Ui = lambdai * Ui

得到矩阵AA^T的m个特征值和m个特征向量，AA^T的所有特征值向量张成一个m*m的矩阵U，则就SVD中的U。一般U中的每个特征向量为左奇异向量

 

求Σ：

Σ需要求出每个奇异值

A = UΣV^T

AV= UΣ

Σ = U^TAV



证明：

A^TA = VΣU^TUΣV^T = V Σ^2 V^T，所有A^TA的W为V，特征值为奇异值的平方

AA^T = U^TΣVV^TΣU = U Σ^2 U^T，所有AA^T的W为U，特征值为奇异值的平方

所以求奇异值可以直接用特征值的根号



## 3、SVD计算

<img src="..\picture\SVD1.png" alt="SVD1" style="zoom:40%;" />



<img src="..\picture\SVD2.png" alt="SVD2" style="zoom:40%;" />





## 4、SVD性质

 对于奇异值和特征值类似，在奇异值矩阵中也是从大到小排列，而且奇异值的减少的特别快，在很多情况下，前10%甚至1%的奇异值的和就占了全部的奇异值之和的99%的以上比例



就是说，我们可以用最大的k个奇异值和对应的左右奇异向量来近似的描述矩阵



A（m*n） = U（m\*m）Σ（m\*n）V（n\*n）^T 约等于 

U（m\*k）Σ（k\*k）V（k\*n）^T  其中k<<n



![img](https://pic3.zhimg.com/80/v2-4437f7678e8479bbc37fd965839259d2_720w.jpg)



所有SVD可以用来进行PCA降维，数据压缩和去噪，推荐算法：

因为奇异值的前10%就可以约为总和的99%



## 5、SVD用于PCA

PCA降维，需要找到样本协方差矩阵X^TX的最大的d个特征向量，然后利用这最大的d个特征向量张成的矩阵来做低维投影。可以看出，在这个过程中需要先求出协方差矩阵X^TX,当样本数多、样本特征数也多的时候，计算量很大



可以利用SVD来帮忙：

SVD可以得到X^TX最大的d个特征向量张成的矩阵，我们可以先不求X^TX，也能求出右奇异矩阵V



左奇异矩阵可以用于**行数的压缩**

右奇异矩阵可以用于**列数（特征维度）的压缩**，就是PCA降维







## 6、几何意义

- 任意的矩阵M可以分解为3个矩阵，U，Σ，V

- V表示原始域的标准正交基

- U表示经过M变换后的新标准正交基

- Σ表示V中向量和U中对应向量的比例关系

- Σ中的奇异值会从大到小排序好，值越大，则该维度越重要

- 提取数据：

  ![[公式]](https://www.zhihu.com/equation?tex=%5Cdisplaystyle+k+%3D+%5Carg+%5Cmin_%7Bk%7D+%5Cfrac%7B%5Csum_%7Bi%3D0%7D%5E%7Bk%7D%5Csigma_i%5E2%7D%7B%5Csum_%7Bi%3D0%7D%5E%7BN%7D%5Csigma_i%5E2%7D+%5Cgeqslant+0.9)





## 7、图片压缩python代码

```python
import numpy as np
import numpy.linalg as la
import matplotlib.pyplot as plt
from sklearn import datasets
from skimage import io


def getImgAsMat(index):
    ds = dataset.fetch_olivetti_faces()
    return np.mat(ds.images[index])

def getImgAsMatFromFile(filename):
	img = io.imread(filename, as_grey=True)
    
def plotImg(imgMat):
    plt.imshow(imgMat,cmap = plt.cm.gray)
    plt.show()
    
def recoverBySVD(imgMat,k):
    U,s,V = la.svd(imgMat)
    Uk = U[:,0:k]
    Sk = np.diag(s[0:k])
    Vk = V[0:k,:]
    imgMat new = Uk*Sk*Vk
    return imgMat_new

A = getImgAsMatFromFile('D:/pic.jpg')
plotImg(A)
A_new = recoverBySVD(A,30)
plotImg(A_new)

```





## 8、SVD应用于求解Ax = b

- SVD：

  ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Singular_value_decomposition_visualisation.svg/220px-Singular_value_decomposition_visualisation.svg.png)

  Σ的形状为m*n，可以有三种形式：

  m=n，m<n，m>n





- 伪逆 pseudo inverse

  

  对于Ax = b这个问题，我们已经知道：A可以被SVD分解为A = UΣV^T

  现在构造一个Σ+ 形状为n*m，可以对应三种形式

  n=m，n<m，n>m

  ![img](https://pic2.zhimg.com/80/v2-926768afc2db7b6194fa67be00c79bcd_720w.jpg)

  

  目的是为了使得Σ+Σ=I

  ![img](https://pic4.zhimg.com/80/v2-7f351b98e5dc8b722b3dbc7793c7714b_720w.jpg)

  对于等式Ax=b

  UΣV^T x = b

  两边同乘以U^T，Σ+，V

  x' = VΣ+U^T b = A+b

  

  A+为伪逆：

  A+ = VΣ+U^T





- 关于解的讨论

  这里x‘和x不一样

  

  

  最小二乘问题（Least-Squares Solution）：

  使得||Ax-b||最小的解

  对于不同形状/不同秩的矩阵，x' = A+b有不同的含义

  

  - 当A是方阵的时候，m=n，方程数和未知数一样

    - 满秩

      A+ = A-1

      x' = A-1b，x’是唯一解，||Ax-b||=0

    - 不满秩

      x‘是不唯一的最小二乘解，||Ax-b||>=0，通常！=0，但是其||x'||最小

  - 当A是细长的时候，m>n，方程数多于未知数

    - 满秩，x’是唯一最小二乘解
    - 不满秩，x‘是不唯一的最小二乘解，||Ax-b||>=0，通常！=0，但是其||x'||最小

    

  - 当A是矮胖的时候，m<n，方程数少于未知数

    - 满秩，x'是不唯一解，||x'||最小
    - 不满秩，不唯一的最小二乘解，||x'||最小

  ![img](https://pic2.zhimg.com/80/v2-ff02fcac90ddaccc9826c2e4fed951b5_720w.jpg)

  















reference：

https://zhuanlan.zhihu.com/p/29846048

https://zhuanlan.zhihu.com/p/36546367

https://zhuanlan.zhihu.com/p/131097680