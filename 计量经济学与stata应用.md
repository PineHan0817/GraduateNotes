#第一章 计量经济学

## 1.1 什么是计量经济学

“计量经济学”(econometrics)，是运用概率统计方法对经济变量之间的(因果)关系进行定量分析的科学。

由于实验数据的缺乏，计量经济学常常不足以确定经济变量之间的因果关系。但大多数实证分析的目的恰恰正是要确定变量之间的因果关系( 即 *X* 是否导致*Y* )，而非相关关系。

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071157911.png" alt="image-20230607115708800" style="zoom: 80%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071157893.png" alt="image-20230607115725833" style="zoom: 80%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071206648.png" alt="image-20230607120600608" style="zoom: 80%;" />

## 1.2 经济数据的特点与类型

***特点***：
经济数据是自然发生的观测数据(observational data)；比如，统计局收集的数据。

由于个人行为的随机性，所有经济变量原则上都是随机变量。在有些本科计量教材中，为了简单起见，有时假设解释变量是

非随机的、固定的(fixed regressors) 。

这只是教学法上的权宜之计，给深入的理论探讨带来不便。如果解释变量为非随机，则无法考虑其与扰动项的相关性。

在本教程中，所有变量都是随机的(即使非随机的常数，也可视为退化的随机变量) 。

---

***类型***：

1.**横截面数据**(cross-sectional data，简称截面数据)：指的是多个经济个体的变量在同一时点上的取值。

2.**时间序列数据**(time series data)：指的是某个经济个体的变量在不同时点上的取值。

3.**面板数据**(panel data)：指的是多个经济个体的变量在不同时点上的取值。（既包括横截面数据，又包括时间序列数据）

# 第二章 Stata命令

**1.查看数据集中的便令名称、标签等，可简写为d**

```stata
describe
```

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071706029.png" alt="image-20230607170609989" style="zoom: 67%;" />

**2.查看数据**

**在屏幕底端出现带下划线的英文字“more”，用鼠标单击“more”，可翻看下页的结果。**

```stata
//查看s与lnw的具体数据
list s lnw
//连续滚屏显示命令运行结果
set more off
//恢复分页显示运行结果
set more on
//只看 s与 lnw 的前5个数据
list s lnw in 1/5
//罗列从第 11-15 个观测值
list s lnw in 11/15
//列出所有满足条件“s>=16”的数据
list s lnw if s>=16
//删除满足“s>=16”条件的观测值
drop if s>=16
//保留满足“s>=16”条件的观测值，删除其它
keep if s>=16
```

![image-20230607170700560](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071707624.png)

**3.数据排序**

```stata
//将数据按照变量s的升序排列
sort s
list
//命令 sort 无法按照变量的降序排列。如想按降序排列，可使用命令 gsort
gsort -s
list
```

**4.画图**

直方图

```stata
//查看变量 s 的分布情况，“histogram”表示直方图,“width(1)”表示将组宽设为 1(否则将使用 Stata 根据样本容量计算的默认分组数),“frequency”表示将纵坐标定为频数(默认使用密度)
histogram s, width(1) frequency
//对于任何 Stata 命令，只要输入“help command_name”即可查看该命令的“帮助文件”
help histogram
```

![image-20230607171226339](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071712386.png)

散点图

```stata
//考察教育年限s与工资对数lnw之间的关系
scatter lnw s
//在散点图上标注出每个点对应于哪个观测值，可先定义变量n，表示第n个观测值
//“_n”表示第n个观测值
//然后以变量n作为每个点的标签来画散点图,选择项“mlabel(n)”表示，以变量 n 作为标签(mark label)
gen n=_n
scatter lnw s,mlabel(n)
```

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071715929.png" alt="image-20230607171522886" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071715019.png" alt="image-20230607171535964" style="zoom: 67%;" />

**5.统计分析**

```stata
//查看变量 s 的统计特征,此结果显示变量 s 的样本容量、平均值、标准差、最小值与最大值。
summarize s
```



<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071717939.png" alt="image-20230607171748904"  />

```stata
//如不指明变量，则显示所有变量的统计指标。
sum
```



<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071718168.png" alt="image-20230607171855113"  />

```stata
//显示变量 s 的经验累积分布函数(empirical cumulative distribution function)
tabulate s 
```

图中“Freq”表示频数，“Percent”表示百分比，而“Cum.”表示累积百分比

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071719163.png" alt="image-20230607171940130"  />

```stata
//要显示工资对数、教育年限、工龄之间的相关系数
//“pwcorr”表示“pairwise correlation”(两两相关)，“sig”表示显示相关系数的显著性水平(即p值，列在相关系数的下方)
//“star(.05)”表示给所有显著性水平小于或等于5%的相关系数打上星号。
pwcorr lnw s expr,sig star(.05)
```

 lnw 与 s 的相关系数为 0.5368，且在 1%水平上显著( *p*值为0.0022)。

 lnw 与 expr 的相关系数也达到 0.2029，但不显著( *p*值为 0.2823，可能因为样本容量较小，仅为 30)。

 s 与 expr 的相关系数为-0.1132，可能因为上学时间长的年轻人，参加工作时间就不长，但此负相关关系也不显著( *p*值为 0.5514)

![image-20230607172501128](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071725170.png)

**6.生成新变量**

```stata
//定义新变量通过命令 generate 来实现
generate lns=log(s)
//定义 s 的平方项
gen s2=s^2
//生成 s 与 expr 的互动项(interaction term)
gen exprs=s*expr
//根据工资对数 lnw 计算工资水平 w
gen w=exp(lnw)
```

在计量经济学中，常使用“虚拟变量”(dummy variable，也称“哑巴变量”)，即取值只能为 0 或 1 的变量，比如性别。

假设定义*s* >=16为“受过高等教育”，并使用变量 college 来表示：

![image-20230607174535808](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071745849.png)

```stata
//括弧“()”表示对括弧中的表达式“s>=16”进行逻辑评估：如果此式为真，则取值为 1；如果为假，则取值为 0。
gen colleg=(s>=16)
//在上面命令中，不慎把 college 打成 colleg 了。可使用如下命令将变量重新命名
rename colleg college

//如想将“受过高等教育”的定义改为“s>=15”，但仍用 college作为变量名。
//方法之一，去掉现有变量 college，再重新定义一次
drop college
gen college=(s>=15)
//方法之二
replace college=(s>=15)
```

**7.命令库更新**

```stata
//更新你的 Stata 命令库(Stata“ado”文件及其他可执行文件)
update all
//Stata 非官方命令下载平台为“统计软件成分”(Statistical Software Components，SSC)
ssc install newcommand
```

如非官方命令不是来自 SSC，一般需手工安装，将所有相关文件下载到指定的 Stata 文件夹中即可(通常为 ado\plus\)。如不清楚应把文件复制到哪个文件夹，可输入以下命令，显示Stata 的系统路径(system directories)：`sysdir`会看到类似于以下的结果(取决于 Stata 的安装位置)

![image-20230607175328235](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306071753273.png)



# 第三章 数学回顾

##3.1 微积分

### 1.导数

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061927538.png" alt="image-20230606192703463" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061925872.png" alt="image-20230606192520532" style="zoom: 67%;" />

<img src="C:\Users\12723\AppData\Roaming\Typora\typora-user-images\image-20230606192908171.png" alt="image-20230606192908171" style="zoom: 33%;" />



###2.一元最优化

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061932603.png" alt="image-20230606193223392" style="zoom:33%;" />

一元最大化问题：找到一个x使得f(x)最大

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061934795.png" alt="image-20230606193413710" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061934573.png" alt="image-20230606193431520" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061937632.png" alt="image-20230606193731536" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306061940442.png" alt="image-20230606194053377" style="zoom:33%;" />

### 3.偏导数

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081725837.png" alt="image-20230608172510295" style="zoom:80%;" />

目的是求出当x~1~发生变化，x~2~、x~3~...x~n~均不发生变化时，函数y变化的速度

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081730154.png" alt="image-20230608173001103" style="zoom:80%;" />

### 4.多元最优化

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081734292.png" alt="image-20230608173427230" style="zoom:80%;" />

目的是找到一组***X***使得f(***X***)最大

一阶条件要求在最优值**x***处，所有偏导数均为 0：

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081738536.png" alt="image-20230608173842479" style="zoom:80%;" />

多元最小化的一阶条件与此相同。此一阶条件要求在最优值**x***处，曲面 *f* (**x** )  在各个方向的切线斜率都为 0 。

### 5.积分

考虑计算连续函数y=f(x)在区间[a,b]上的面积

![image-20230608175756817](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081757860.png)

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081759336.png" alt="image-20230608175927356" style="zoom:33%;" />

![image-20230608180118697](https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081801743.png)

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081801718.png" alt="image-20230608180127651" style="zoom: 80%;" />

这里其实就是划分成无数个矩形的意思

<img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306081802507.png" alt="image-20230608180210443" style="zoom:80%;" />



## 3.2 线性代数

### 1. 矩阵



# 第四章 一元线性回归

## 4.1 一元线性回归模型

一元：一个解释变量

​                                   <img src="https://raw.githubusercontent.com/PineHan0817/ImageHost/main/202306062227761.png" style="zoom: 33%;" />

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