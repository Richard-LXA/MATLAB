# Matlab 第二章

## 数据类型

| 数据格式                                          | 示例                                           | 说明                                                         |
| ------------------------------------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| int8,uint8,int16,uint16,int32,uint32,int64,uint64 | 输入int16(2^15) %2^15 =32768   得到：ans=32767 | 分别表示有符号和无符号的整数类型；相同数值的整数类型占用比浮点类型更少的内存 |
| single                                            | single(0.1)                                    | 单精度浮点类型                                               |
| double                                            | 5.324、1.043-0.714i                            | 双精度浮点类型，默认数值类型                                 |



| 函数  | 运算法则                                        | 示例                                               |
| ----- | ----------------------------------------------- | -------------------------------------------------- |
| floor | 向下取整                                        | floor(1.4)=1<br />floor(3.5)=3<br />floor(-3.5)=-4 |
| ceil  | 向上取整                                        | ceil(1.4)=2<br />ceil(3.5)=4<br />floor(-3.5)=-3   |
| round | 取最近的整数，如果小数为0.5则向绝对值大的方向取 | round(1.4)=1<br />round(3.5)=4<br />round(-3.5)=-4 |
| fix   | 向0取整                                         | fix(1.4)=1<br />fix(3.5)=3<br />fix(-3.5)=-3       |



eps函数可以获取一个数值和最接近该数值的浮点数之间的间隔。

如输入： 

```matlab
e1=eps(6), e2=eps(single(6))
```

输出：

```matlab
e1 = 8.881784197001252e-16

e2=  4.7683716e-07
```

在matlab中 eps可视为0，0附近的eps近似为2.2e-16，这种特殊表达在避免0为分母时很有用。



```matlab
string = 'good boy'
whos
s1=abs(string)
s2=abs(string+'0')
```



### 结构：

- 创建结构对象

1.通过字段赋值创建结构

```matlab
patient.name = 'Ruoyo'
patient.billing = 127.00
patient.test = [70,75;20,10]
patient
whos
```

2.利用struct函数创建结构

```matlab
patient = struct('name','Ruoyo','billing','127.00','test',[70,75;20,10])
whos
```

- 访问结构对象

```matlab
patient（1） = struct('name','Ruoyo','billing','127.00','test',[70,75;20,10])
patient(2).name = 'John'
patient(2).billing = 500
patient(2).test = [40,60;30,40]

p1=patient(1),p2=patient(2), p1name=patient(1).name, p2name=patient(2).name
```

- 连接结构对象

```matlab
patient1=struct(...)
patient2=struct(...)
patient = [ patient1, patient2];
```



### 单元数组

单元数组是一种***广义矩阵***，每一个单元可以包括一个任意数组，每一个单元可以具有不同的尺寸和内存占用空间



- 创建单元数组

```matlab
A = {'x',[2;3;6];10,2*pi}
B = cell(2,2)
```

- 访问单元数组

可以选择访问单元，也可以访问单元的内容

```matlab
A = {'x',[2;3;6];10,2*pi}
b = A(1,2)
c = A{1,2}
```

输出为

```matlab
b = [3x1 double]
c = 2
    3
    6
```



- 单元数组的操作

操作包括合并、删除单元数组中的指定单元和改变单元数组的形状

（1）单元数组的合并

```matlab
A = {'x',[2;3;6];10,2*pi}
B = {'Jan'}
C = {A B}
```

输出：

```matlab
C = {2x2 cell}  {1x1 cell}
```

（2）删除单元数组中指定单元

```matlab
A = {'x',[2;3;6];10,2*pi}
A{1,2} = []
```

A中的[2;3;6]即被替换为[]

（3）使用reshape函数改变单元数组的形状

```matlab
A = {'x',[2;3;6];10,2*pi}
newA = reshape(A,1,4)
```

```
newA = {'x'}   {[10]}    {3x1 double}   {[6.2832]}
```



### 函数句柄

MATLAB中可以实现对函数的间接调用，这归功于函数句柄提供了一种间接调用函数的方法

创建函数句柄需用到操作符@，对matlab库函数中提供的各种M文件中的函数和使用者自主编写的程序中的内部函数，也都可以创建函数句柄，进而通过函数句柄来实现对这些函数的间接调用

格式：

```matlab
Function_Handle = @Function_Filename;
```

Function_Filename是函数所对应的M文件的名称或MATLAB内部函数的名称



例如

```matlab
F_Handle = @sin
x = 0 : 0.25*pi : pi;
F_Handle(x)
```



| 函数名称            | 函数功能                                              |
| ------------------- | ----------------------------------------------------- |
| function_handle或@  | 间接调用函数                                          |
| func2str            | 将函数句柄转换为函数名称字符串                        |
| str2func            | 将字符串代表的函数转换为函数句柄                      |
| fuctions(funhandle) | 返回一个存储函数名称、函数类型及函数M文件位置的结构体 |



例如

```matlab
>> F_handle = @sin
>> f1 = functions(F_handle);
>> t = func2str(F_handle);
>> F_handle1 = str2func(t);
>> f2 = functions(F_handle1)
```

输出

```matlab
t = sin
F_handle1 = @sin
f2 = 
	包含以下字段的struct
	function: 'sin'
	    type: 'simple'
	    file: ''
```



### 映射容器

​        例如一个字符串映射为一个数值，则相应字符串就是映射的键（key），相应值就是映射的数据（value）。可以将Map容器理解为一种快速键查找数据结构

​    	对一个Map元素进行寻访的索引称为“键”，一个key可以是以下任何一种数据类型

​		（1）1xN字符串；

​		（2）单精度或双精度实数标量

​		（3）有符号或无符号标量整数



## 运算符与运算

| 运算符 | 运算法则                                 |
| ------ | ---------------------------------------- |
| A.*B   | A与B相应元素相乘（A、B为相同维度的矩阵） |
| A./B   | A与B相应元素相除（A、B为相同维度的矩阵） |
| A.^b   | A的每个元素的B次幂（b为数值）            |

p.s. 不等于 ~=

| 逻辑运算符 | 说明 |
| ---------- | ---- |
| &          | 与   |
| \|         | 或   |
| ~          | 非   |

部分逻辑函数

| 函数     | 运算法则                                                     |
| -------- | ------------------------------------------------------------ |
| xor(x,y) | 异或运算                                                     |
| any(x)   | 或，向量x中，有任何元素是非零，返回1<br />        矩阵x中的每一列有非零元素，返回1 |
| all(x)   | 向量或矩阵中所有元素非0，返回1                               |

## 矩阵基础

### 创建矩阵

- 对变量直接进行赋值
- 使用特殊矩阵创建函数

### 改变矩阵结构



### 矩阵下标



### 矩阵信息



