#第一章 计量经济学：探究变量之间的因果关系

# 第二章 数学回顾

##2.1 微积分

### 1. 导数

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061927538.png" alt="image-20230606192703463" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061925872.png" alt="image-20230606192520532" style="zoom: 67%;" />

<img src="C:\Users\12723\AppData\Roaming\Typora\typora-user-images\image-20230606192908171.png" alt="image-20230606192908171" style="zoom: 33%;" />



#### 2. 一元最优化

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061932603.png" alt="image-20230606193223392" style="zoom:33%;" />

一元最大化问题：找到一个x使得f(x)最大

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061934795.png" alt="image-20230606193413710" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061934573.png" alt="image-20230606193431520" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061937632.png" alt="image-20230606193731536" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061940442.png" alt="image-20230606194053377" style="zoom:33%;" />

# 第三章 一元线性回归

## 3.1 一元线性回归模型

一元：一个解释变量

​                                                             <img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062227761.png" style="zoom: 33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062231989.png" alt="image-20230606223151600" style="zoom:33%;" />
$$
式子推导：\\
∵f(x)=y=ax+b\\
f'(x)=y'=dy/dx\\
dlnx=dx*(1/x)=dx/x\\
∴f(s)=lnw=α+βs\\
f'(s)=dlnw/ds=β\\
f'(s)=dlnw/ds=dw*(1/w)=dw/w/ds≈△w/w/△s
$$
<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062251987.png" alt="image-20230606225131885" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062253560.png" alt="image-20230606225324455" style="zoom:33%;" />



明瑟模型推断工资对数与教育年限为线性关系，此预言是否与现实数据相符？使用数据集grilic.dta来考察，此数据集包括758位美国年轻男子的教育投资回报率数据。

先看此数据集的变量s与lnw的前10个观测值

```stata
//list代表列，1/10表示1至10
use grilic.dta,clear  
list s lnw in 1/10
```

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062319764.png" alt="image-20230606231940695" style="zoom:33%;" />

为了考察工资对数与教育年限的关系，画二者的散点图，并在图上画出离这些样本点最近的“回归直线”

```stata
//twoway二维图；scatter散点图；||分隔符，为了后面继续画图；lfit表示linear fit线性拟合
twoway scatter lnw s || lfit lnw s
```

<img src="C:\Users\12723\AppData\Roaming\Typora\typora-user-images\image-20230606232349203.png" alt="image-20230606232349203" style="zoom: 50%;" />



工资对数与教育年限正相关，似乎存在线性关系，这是平均规律，但图中还存在一些不在斜线上的点