# C++基础

## 1 c++初识

### 1.1 第一个C++程序 HelloWorld!

1. 创建项目

2. 创建文件

3. 编写代码

   ```C++
   #include<iostream>
   using namespace std;
   
   int main(){
   
       cout << "HelloWorld!" << endl;
   
       system("pause");
   
       return 0;
   }
   ```

4. 编译执行

### 1.2 注释

- 单行注释

  ```C++
  // 单行注释
  ```

- 多行注释

  ```C++
  /*
   * 多行注释
   */
  ```

### 1.3 变量

**作用：**给一段内存起名，便于操作这块内存

**变量的创建：**`变量类型 变量名 = 初始值;`

```C++
int a = 10;
```

### 1.4 常量 

**作用：**用于记录程序中不更改的数据

定义常量的两种方式：

1. **#define宏常量**：`#define 常量名 常量值`
   - 通常在文件上方定义，表示一个常量
2. **const修饰的变量**：`const 数据类型 常量名 = 常量值;`
   - 通常在变量定义前加关键字const，修饰该变量为一个常量，不可修改

```C++
#include<iostream>
using namespace std;

#define DAY 7
int main() {
    cout << "一周有" << DAY << "天" << endl;

    const int day = 365;
    cout << "一年有" << day << "天" << endl;

    system("pause");
    return 0;
}
```



### 1.5 关键字

作用：关键字是C++中预先保留的单词(标识符)

- 在定义变量或常量的时候不要使用关键字

### 1.6  标识符命名规则

- 标识符不可以是关键字
- 标识符由字母、数字、下划线组成
- 标识符第一个字符只能是字母或下划线
- 标识符区分大小写

## 2 数据类型

C++明确规定在创建变量或常量前必须指出数据类型，否则无法分配内存。

数据类型的意义：给变量分配合适的内存空间。

### 2.1 整型

**作用:**整型变量表示的是整数类型的数据。

C++中能够表示整型的类型有以下几种，**区别在于所占内存空间不同**：

| 数据类型  | 占用空间                                       | 取值范围         |
| --------- | ---------------------------------------------- | ---------------- |
| short     | 2字节                                          | (-2^15~^2^15^-1) |
| int       | 4字节                                          | (-2^31~^2^31^-1) |
| long      | windows为4字节,Linux为4字节(32位)，8字节(64位) | (-2^31~^2^31^-1) |
| long long | 8字节                                          | (-2^63~^2^63^-1) |

### 2.2 sizeof关键字

**作用**：利用sizeof关键字可以统计数据类型所占内存大小

**语法**：`sizeof (数据类型 / 变量)`

```C++
#include<iostream>
using namespace std;

int main() {
    cout << sizeof(short) << endl;	  // 2
    cout << sizeof(int) << endl;	  // 4
    cout << sizeof(long) << endl;	  // 4
    cout << sizeof(long long) << endl;// 8
    system("pause");
    return 0;
}
```

### 2.3 实型(浮点型)

**作用**:用于表示小数

浮点型变量分为两种：

- 单精度浮点数 float
- 双精度浮点数 double

两者的区别在于表示的有效数字范围不同。默认情况下输出都为6位有效数字。

| 数据类型 | 占用空间 | 有效数字范围    |
| -------- | -------- | --------------- |
| float    | 4字节    | 7位有效数字     |
| double   | 8字节    | 15~16位有效数字 |

**科学计数法**：3e5 = 3*10^5

```C++
#include<iostream>
using namespace std;
int main()
{
    float f1 = 1.11111111f;
    double d1 = 1.11111111111;
    cout << "float:" << f1 << endl;	// 1.11111
    cout << "double:" << d1 << endl;// 1.11111

    cout << "float占用内存空间：" << sizeof(float) << endl;// 4
    cout << "double占用内存空间：" << sizeof(double) << endl;// 8
    // 科学计数法
    float e = 3e2;
    cout << "3e2" << e << endl; // 300
    system("pause");
    return 0;
}
```

### 2.4 字符型

**作用：**字符型变量用于显示单个字符

**语法：**`char ch = 'a';`

> 1、在现实字符型变量时，用单引号将字符括起来，不要用双引号
>
> 2、单引号内只能有一个字符，不可以是字符串

- C和C++字符型变量只占用1个字节
- 字符型变量并不是把字符本身放到内存中储存，而是将对应的ASCII编码放到存储单元

### 2.5 转义字符

**作用**：用于表示一些不能显示出来的ASCII字符

|      |            |      |
| ---- | ---------- | ---- |
| \n   | 换行       |      |
| \t   | 水平制表符 |      |
| \\   | 反斜杠 \   |      |

### 2.6 字符串型

**作用：**用于表示一种字符

**两种风格**：

1. **C风格字符串**：`char 变量名[] = "字符串值"`

2. **C++风格字符串**：`string 变量名 = "字符串值"`

   > 需要引入头文件 #include<string>

```C++
string str = "C++风格字符串";
char string1[] = "C风格字符串";
```

### 2.7 布尔型 bool

**作用**：布尔型表示真或假

**bool类型只有两个值**：

- true	真(本质是1)

- false   假(本质是0)

  > 在输入时 只要是非0的值都代表真

**bool类型占1个字节大小**

```C++
#include<iostream>
using namespace std;

int main(){
    bool flag = true;
    cout << flag << endl; // 1
    flag = false;
    cout << flag <<endl; // 0

    cout << "布尔类型大小：" << sizeof(bool) << endl; // 1
    return 0;
}
```

### 2.8 数据的输入

**作用**：用于从键盘获取数据

**关键字**：cin

**语法：**`cin >> 变量名`

```C++
#include<iostream>
#include<string>

using namespace std;

int main() {
    //整型数据输入
    int a = 0;
    cout << "a的值为:\t\t" << a << endl;
    cout << "请修改a的值：\t";
    cin >> a ;
    cout << "您输入的值是：\t" << a << endl;
    //浮点型数据输入
    float f = 0.001f;
    cout << "f的值为:\t\t" << f << endl;
    cout << "请修改f的值：\t";
    cin >> f ;
    cout << "您输入的值是：\t" << f << endl;
    //char 型数据输入
    char c = 'a';
    cout << "c的值为:\t\t" << c << endl;
    cout << "请修改c的值：\t";
    cin >> c ;
    cout << "您输入的值是：\t" << c << endl;
    //字符串类型数据输入
    string str = "abcd";
    cout << "str的值为:\t\t" << str << endl;
    cout << "请修改str的值：\t";
    cin >> str ;
    cout << "您输入的值是：\t" << str << endl;
    //bool 型数据输入
    bool flag = true;
    cout << "flag的值为:\t\t" << flag << endl;
    cout << "请修改flag的值：\t";
    cin >> flag ;
    cout << "您输入的值是：\t" << flag << endl;
    return 0;
}
```

## 3 运算符

**作用：**用于执行代码的运算

**运算符类型：**

| 运算符类型 | 作用                                   |
| ---------- | -------------------------------------- |
| 算术运算符 | 用于处理四则运算                       |
| 赋值运算符 | 用于将表达式的值赋给变量               |
| 比较运算符 | 用于表达式的比较，并返回一个真值或假值 |
| 逻辑运算符 | 用于表达式的值返回真值或假值           |

### 3.1 算术运算符

**作用：**用于处理四则运算

算术运算符包含以下符号：

| 运算符 | 术语       | 示例       | 结果     |
| ------ | ---------- | ---------- | -------- |
| +      | 正号       | +3         | 3        |
| -      | 负号       | -3         | -3       |
| +      | 加         | 1+2        | 3        |
| -      | 减         | 2-1        | 1        |
| *      | 乘         | 2*3        | 6        |
| /      | 除         | 6/3        | 2        |
| %      | 取模(取余) | 10%3       | 1        |
| ++     | 前置递增   | a=2;b=++a; | a=3;b=3; |
| ++     | 后置递增   | a=2;b=a++; | a=3;b=2; |
| --     | 前置递减   | a=2;b=--a; | a=1;b=1; |
| --     | 后置递减   | a=2;b=a--  | a=1;b=2; |

> 两个小数不能做取模运算

```C++
#include<iostream>
using namespace std;

int main(){
    int a,b;
    a = 5;
    b = 2;
    cout << "a=" << a << ";b=" << b << endl;
    //四则运算
    cout << "a+b=" << a+b <<endl;
    cout << "a-b=" << a-b <<endl;
    cout << "a*b=" << a*b <<endl;
    cout << "a/b=" << a/b <<endl;
    //取模
    cout << "a%b=" << a%b << endl;
    cout << "---------------------------" << endl;
    //前置自增
    a = 2;
    cout << "a=" << a << endl;
    b = ++a;
    cout << "a=" << a << endl;
    cout << "++a=" << b << endl;
    //后置自增
    a = 2;
    cout << "a=" << a << endl;
    b = a++;
    cout << "a=" << a << endl;
    cout << "a++=" << b << endl;
    //前置自减
    a = 2;
    cout << "a=" << a << endl;
    b = --a;
    cout << "a=" << a << endl;
    cout << "--a=" << b << endl;
    //后置自减
    a = 2;
    cout << "a=" << a << endl;
    b = a--;
    cout << "a=" << a << endl;
    cout << "a--=" << b << endl;
    //浮点数相除
    double d1 = 3.3;
    double d2 = 1.111;
    cout << d1 / d2 << endl;
    return 0;
}

```

### 3.2 赋值运算符

**作用：**用于将表达式的值赋给变量

赋值运算符包含以下几个符号：

| 运算符 | 术语   | 示例      | 结果     |
| ------ | ------ | --------- | -------- |
| =      | 赋值   | a=2;b=3;  | a=2;b=3; |
| +=     | 加等于 | a=0;a+=2; | a=2;     |
| -=     | 减等于 | a=5;a-=3; | a=2;     |
| *=     | 乘等于 | a=2;a*=3; | a=6;     |
| /=     | 除等于 | a=4;a/=2; | a=2;     |
| %=     | 模等于 | a=3;a%=2; | a=1;     |

### 3.3 比较运算符

**作用：**用于表达式的比较，并返回一个真值或假值

比较运算符包含以下符号：

| 运算符 | 术语     | 示例 | 结果 |
| ------ | -------- | ---- | ---- |
| ==     | 相等于   | 4==3 | 0    |
| !=     | 不等于   | 4!=3 | 1    |
| <      | 小于     | 4<3  | 0    |
| >      | 大于     | 4>3  | 1    |
| <=     | 小于等于 | 4<=3 | 0    |
| >=     | 大于等于 | 4>=3 | 1    |

### 3.4 逻辑运算符

**作用：** 用于根据表达式的值返回真值或假值

逻辑运算符包含以下符号：

| 运算符 | 术语 | 示例   | 结果                                                     |
| ------ | ---- | ------ | -------------------------------------------------------- |
| !      | 非   | !a     | 如果a为假,则!a为真;如果a为真,则!a为假。                  |
| &&     | 与   | a&&b   | 如果a和b都为真，则结果为真，否则为假。                   |
| \|\|   | 或   | a\|\|b | 如果a和b至少一个为真，则结果为真；二者都为假，结果为假。 |

## 4 程序流程结构

C/C++支持最基本的三种程序运行结构：`顺序结构、选择结构、循环结构`

- 顺序结构：程序按顺序执行，不发生跳转。
- 选择结构：依据条件是否满足。有选择的执行相应的功能。
- 循环结构：依据条件是否满足，循环多次执行某段代码。

### 4.1 选择结构

#### 4.1.1 if语句

**作用：**执行满足条件的语句。

if语句的三种形式：

- 单行格式if语句

- 多行格式if语句

- 多条件的if语句

  

1. **单行格式if语句：**`if(条件){条件满足执行的语句}`
   
   ```flow
   st=>start: 开始
   cond=>condition: 判断条件
   sub1=>subroutine: 执行程序
   e=>end: 结束
   st->cond
   cond(true)->sub1->e
   cond(false)->e
   ```
   
2. **多行格式if语句：**`if(条件){条件满足执行的代码}else{条件不满足执行的代码}`

   ```flow
   st=>start: 开始
   cond=>condition: 判断条件
   sub1=>subroutine: 执行语句1
   sub2=>subroutine: 执行语句2
   e=>end: 结束
   st->cond
   cond(true)->sub1->e
   cond(false)->sub2->e
   ```

   

3. **多条件的if语句(级联)：**`if(条件1){条件一满足执行的语句}else if(条件2){条件2满足执行的语句} ...else{以上条件都不满足执行的语句}`

   ```flow
   st=>start: 开始
   cond1=>condition: 判断条件1
   cond2=>condition: 判断条件2
   cond3=>condition: ......
   condn=>condition: 判断条件n
   sub1=>subroutine: 执行语句1
   sub2=>subroutine: 执行语句2
   sub3=>subroutine: 执行语句3
   sub4=>subroutine: 执行语句4
   sub5=>subroutine: 执行语句5
   e=>end: 结束
   st->cond1
   cond1(true)->sub1->e
   cond1(false)->cond2
   cond2(true)->sub2->e
   cond2(false)->cond3
   cond3(true)->sub3->e
   cond3(false)->condn
   condn(true)->sub4->e
   condn(false)->sub5->e
   ```

4. **嵌套if语句：**在if语句中，可以嵌套使用if语句，达到更精确的条件判断。

#### 4.1.2 三目运算符

**作用：**通过三目运算符实现简单的判断

**语法：**`表达式1 ? 表达式2 : 表达式3`

**解释：**

> 如果表达式1的值为真，执行表达式2，并返回表达式2的结果；
>
> 如果表达式1的值为假，执行表达式3，并返回表达式3的结果。

> 三目运算符返回的是变量，可以继续赋值。

```C++
#include<iostream>

using namespace std;

int main() {
    int a = 10, b = 20;
    cout << "a:" << a << endl; //10
    cout << "b:" << b << endl; //20
    cout << "-----------------" << endl;
    cout << "(a>b?a:b):" << (a > b ? a : b) << endl;//20
    cout << "a:" << a << endl; //10
    cout << "b:" << b << endl; //20
    cout << "-----------------" << endl;
    cout << "((a>b?a:b)=100):" << ((a > b ? a : b) = 100) << endl;//100
    cout << "a:" << a << endl;//20
    cout << "b:" << b << endl;//100
    return 0;
}
```

#### 4.1.3 选择结构

**作用：**执行多条分支语句

**语法：**

```C++
switch(表达式)
{
    case 结果1:执行语句;break;
    case 结果2:执行语句;break;
    ......
    default:执行语句;break;
}
```

> switch 语句中的表达式类型只能是整型或字符型

> case里如果没有break，那么程序会一直向下执行

> 总结：与if语句相比，对于多条件判断时，Switch的结构清晰，执行效率高，缺点是switch不可以判断区间

### 4.2 循环结构

#### 4.2.1 while循环语句

**作用：**满足循环条件，执行循环语句

**语法：**`while(循环条件){循环语句}`

**解释：**只要循环条件的结果为真，就执行循环语句

```flow
st=>start: 开始
cond=>condition: 循环条件
sub1=>subroutine: 执行语句
e=>end: 结束
st->cond
cond(true)->sub1->cond
cond(false)->e
```

**示例：**

```C++
#include<iostream>
using namespace std;
int main(){
    int n = 0;
    while(n<10){
        cout << n << endl;
        n++;
    }
    return 0;
}
```

> 注意：在执行循环语句的时候，程序必须提供跳出循环的出口，否则出现死循环

#### 4.2.2 do...while循环语句

**作用：**满足循环条件，执行循环语句

**语法：**`do{循环语句}while(循环条件)`

**注意：**与while的区别在于do...while会先执行一次循环语句，再判断循环条件

```flow
st=>start: 开始
cond=>condition: 循环条件
sub1=>subroutine: 循环语句
e=>end: 结束
st->sub1->cond
cond(true)->sub1
cond(false)->e
```

#### 4.2.3 for循环语句

**作用：**满足循环条件，执行循环语句

**语法：**`for(起始表达式;条件表达式;末尾循环体){循环语句}`

**执行顺序：**起始表达式->条件表达式->循环语句->末尾循环体->条件表达式->...

```C++
#include<iostream>
using namespace std;
// 从0打印到9
int main() {
    for (int i = 0; i < 10; i++) {
        cout << i << endl;
    }
    return 0;
}
```

> 注意：for循环中的表达式，要用分号进行分割，

> 总结：while,do...while,for 都是常用的循环语句，for循环结构比较清晰比较常用

#### 4.2.4 嵌套循环

**作用：**在循环体中再嵌套一层循环，解决一些实际问题

**示例：**

```C++
#include<iostream>
using namespace std;
int main() {
    // 外层循环
    for (int i = 0; i < 10; i++) {
        // 内层循环
        for (int j = 0; j < 10; j++) {
            cout << "* ";
        }
        cout << endl;
    }
    return 0;
}
```

### 4.3 跳转语句

#### 4.3.1 break语句

**作用：**用于跳出选择结构或者循环结构

break使用的时机:

- 出现在switch条件语句中，作用是种植case并跳出switch
- 出现在循环语句中，作用是跳出当前的循环语句
- 出现在嵌套循环中，跳出最近的内层循环语句

```C++
#include<iostream>
using namespace std;

int main() {
    cout << "请选择副本难度：" << endl;
    cout << "1-简单难度" << endl;
    cout << "2-困难难度" << endl;
    cout << "3-地狱难度" << endl;

    int select = 0;
    cin >> select;

    switch (select) {
        case 1:
            cout << "您选择的是简单难度！" << endl;
            break;
        case 2:
            cout << "您选择的是困难难度！" << endl;
            break;
        case 3:
            cout << "您选择的是地狱难度！" << endl;
            break;
        default:
            cout << "默认" << endl;
            break;
    }
    return 0;
}
```

#### 4.3.2 continue语句

**作用：**在循环语句中，跳过本次循环中余下尚未执行的语句，进入下一次循环

**实例：**

```C++
#include<iostream>
using namespace std;

int main() {
    for (int i = 0; i < 10; i++) {
        if(i==5){
            continue;
        }
        cout << i << endl;
    }
    return 0;
}
```

> 注意：continue不会终止整个循环，而break会跳出循环

#### 4.3.3 goto语句

**作用：**可以无条件跳转语句

**语法：**`goto标记`

**解释：**如果标记的名称存在，执行到goto语句，会跳转到标记的位置

**示例：**

```C++
#include <iostream>
using namespace std;

int main() {
    cout << "1.xxxxx" << endl;
    cout << "2.xxxxx" << endl;
    goto Flag; // goto 标记
    cout << "3.xxxxx" << endl;// 不再执行
    cout << "4.xxxxx" << endl;// 不再执行
    Flag://标记
    cout << "5.xxxxx" << endl;
    cout << "6.xxxxx" << endl;
    return 0;
}
```

> 注意：程序中不建议使用goto语句，以免造成程序流程混乱

## 5 数组

### 5.1 概述

> 所谓数组，就是一个存放了很多相同类型数据元素的集合

**特点1：**数组中每个数据元素都是相同的数据类型

**特点2：**数组是由连续的内存位置组成的

### 5.2 一维数组

#### 5.2.1 一维数组的定义方式

一维数组的三种定义方式：

1. `数据类型 数组名[数组长度];`
2. `数据类型 数组名[数组长度] = {值1,值2,....};`
3. `数据类型 数组名[] = {值1,值2,....};`

> 注意：数组下标从0开始

> 总结：数组命名规范与变量命名规范一致，不要和变量重名

```C++
#include <iostream>

using namespace std;

int main() {
    // 第一种数组定义方式
    int num1[5];
    // 按数组下标赋值
    num1[0] = 10;
    num1[1] = 20;
    num1[2] = 30;
    num1[3] = 40;
    num1[4] = 50;
    // 按数组下标输出
    for (int i = 0; i < 5; i++) {
        cout << num1[i] << endl;
    }
    // 第二种数组定义方式
    int num2[5] = {10, 20, 30, 40, 50};
    for (int i = 0; i < 5; i++) {
        cout << num2[i] << endl;
    }
    // 第三种数组定义方式
    int num3[] = {10, 20, 30};
    for (int i = 0; i < 3; i++) {
        cout << num3[i] << endl;
    }
    return 0;
}
```

#### 5.2.2 一维数组数组名

一维数组名称的用途：

1. 可以统计整个数组在内存中的长度
2. 可以获取数组在内存中的首地址

```C++
#include <iostream>
using namespace std;

int main() {

    int arr[] = {10, 20, 30};
    // 可以统计整个数组在内存中的长度
    cout << "数组在内存中的长度：" << sizeof(arr) << endl;
    cout << "数组第一个元素在内存中的长度：" << sizeof(arr[0]) << endl;
    cout << "数组元素个数：" << sizeof(arr) / sizeof(arr[0]) << endl;
    // 可以获取数组在内存中的首地址
    cout << "数组在内存中的首地址:" << arr << endl;
    cout << "数组在内存中的第一个元素地址:" << &arr[0] << endl;
    cout << "数组在内存中的第二个元素地址:" << &arr[1] << endl;
    return 0;
}
```

#### 5.2.3 冒泡排序

**作用：**最常用的排序算法，对数组内元素进行排序

1. 比较相邻的元素，如果第一个比第二个大，就进行交换
2. 对每一对相邻元素进行上述工作，执行完毕后，找到第一个最大值
3. 重复以上的步骤，每次比较次数-1，直到不需要比较

> 排序总轮数 = 元素个数 - 1
>

> 内层循环对比次数 = 元素个数 - 排序轮数 - 1
>

```C++
#include<iostream>
using namespace std;

int main() {
    int nums[] = {9, 8, 7, 6, 5, 4, 3, 2, 1, 0, 10};
    int n = sizeof(nums) / sizeof(nums[0]);
    cout << "排序前：" << endl;
    for (int i = 0; i < n; i++) {
        cout << nums[i] << " ";
    }
    cout << endl;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            int temp = nums[j];
            if (nums[j] > nums[j + 1]) {
                nums[j] = nums[j + 1];
                nums[j + 1] = temp;
            }
        }
    }
    cout << "排序后：" << endl;
    for (int i = 0; i < n; i++) {
        cout << nums[i] << " ";
    }
    return 0;
}
```

### 5.3 二维数组

> 在一位数组的基础上添加一个维度

#### 5.3.1 二维数组的定义方式

二维数组的四种定义方式：

```
1. 数据类型 数组名[行数][列数];
2. 数据类型 数组名[行数][列数] = {数据1,数据2},{数据3,数据4};
3. 数据类型 数组名[行数][列数] = {数据1,数据2,数据3,数据4};
4. 数据类型 数组名[][列数] = {数据1,数据2,数据3,数据4};
```

> 建议使用第二种，更加直观，提高代码可读性
>

```C++
#include<iostream>
using namespace std;

int main() {
    // 数据类型 数组名[行数][列数]
    int arr1[2][2];
    // 数据类型 数组名[行数][列数] = {数据1,数据2},{数据3,数据4};
    int arr2[2][2] = {
            {1, 2},
            {3, 4}
    };
    // 数据类型 数组名[行数][列数] = {数据1,数据2,数据3,数据4};
    int arr3[2][2] = {1, 2, 3, 4};
    //数据类型 数组名[][列数] = {数据1,数据2,数据3,数据4};
    int arr4[][2] = {1, 2, 3, 4};
    return 0;
}
```

#### 5.3.2 二维数组数组名

- 查看二维数组所占内存空间
- 获取二维数组首地址

```C++
cout << "二维数组所占内存空间：" << sizeof(arr2) << endl;
cout << "二维数组第一行所占内存空间：" << sizeof(arr2[0]) << endl;
cout << "二维数组首元素所占内存空间：" << sizeof(arr2[0][0]) << endl;
cout << "二维数组行数：" << sizeof(arr2) / sizeof(arr2[0]) << endl;
cout << "二维数组元素数：" << sizeof(arr2) / sizeof(arr2[0][0]) << endl;
cout << "二维数组首地址：" << arr2 << endl;
cout << "二维数组首行地址：" << arr2[0] << endl;
cout << "二维数组第二行地址：" << arr2[1] << endl;
cout << "二维数组首元素地址：" << &arr2[0][0] << endl;
cout << "二维数组第二个地址：" << &arr2[0][1] << endl;
```

#### 5.3.3 二维数组应用案例

**考试成绩统计：**

案例描述：有三名同学(张三、李四、王五)，在一次考试中的成绩分别如下，**请分别输出三名同学的总成绩**

|      | 语文 | 数学 | 英语 |
| ---- | ---- | ---- | ---- |
| 张三 | 100  | 100  | 100  |
| 李四 | 90   | 50   | 100  |
| 王五 | 60   | 70   | 80   |

```C++
#include<iostream>
#include<string>
using namespace std;

int main() {
    int score[3][3] = {
            {100, 100, 100},
            {90,  50,  100},
            {60,  70,  80}
    };
    string names[3] = {"张三", "李四", "王五"};
    for (int i = 0; i < 3; i++) {
        int sum = 0;
        for (int j = 0; j < 3; j++) {
            sum += score[i][j];
        }
        cout << names[i] << "的总成绩为：" << sum << endl;
    }
    return 0;
}
```

## 6 函数

### 6.1 概述

**作用：**将一段经常使用的代码封装起来，减少重复代码

一个较大的程序，一般分为若干个程序块，每个模块实现特定的功能。



### 6.2 函数的定义

函数的定义一般主要有5个步骤：

1. 返回值类型
2. 函数名
3. 参数列表
4. 函数体语句
5. return 表达式

**语法：**

```C++
返回值类型 函数名 (参数列表)
{
    函数体语句;
    
    return 表达式;
}
```

- 返回值类型：一个函数可以返回一个值
- 函数名：给函数起个名
- 参数列表：使用该函数时，传入的数据
- 函数体语句：花括号内的代码，函数内需要执行的语句
- return 表达式：和返回值类型挂钩，函数执行完后，返回相应的数据



### 6.3 函数的调用

**功能：**使用定义好的函数

**语法：**`函数名(参数)`

```C++
#include<iostream>
using namespace std;

// 定义函数
int add(int num1, int num2)// num1、num2为形式参数
{
    int sum = num1 + num2;
    return sum;
}

int main() {
    int num1, num2;
    cin >> num1 >> num2;
    // main 函数中调用 add() 函数
    int sum = add(num1, num2);// 此时num1、num2为实参
    cout << num1 << "+" << num2 << "=" << sum << endl;
    return 0;
}
```



### 6.4 值传递

- 所谓值传递，就是函数调用时实参将数值传给形参
- 值传递时，如果形参发生，并不会影响实参

```C++
#include<iostream>

using namespace std;

// 定义函数
void swap(int num1, int num2) {
    int temp = num1;
    num1 = num2;
    num2 = temp;
    cout << "交换后：" << endl;
    cout << "num1=" << num1 << endl;
    cout << "num2=" << num2 << endl;
}

int main() {
    int num1, num2;
    cin >> num1 >> num2;
    cout << "交换前实参：" << endl;
    cout << "num1=" << num1 << endl;//11
    cout << "num2=" << num2 << endl;//22
    swap(num1, num2);//num1=22,num2=11
    cout << "交换后实参：" << endl;
    cout << "num1=" << num1 << endl;//11
    cout << "num2=" << num2 << endl;//22
    return 0;
}
```



### 6.5 函数常见的样式

常见的函数样式：

1. 无参无返
2. 有参无返
3. 无参有返
4. 有参有返



### 6.6 函数的声明

**作用：**告诉编译器函数名称及如何调用函数。函数的实际主体可以单独定义。

> 函数的声明可以多次，但是函数的定义只能有一次。



### 6.7 函数的分文件编写

**作用：**让代码结构更加清晰

函数分文件编写一般有4个步骤：

1. 创建后缀名为.h的头文件
2. 创建后缀名为.cpp的源文件
3. 在头文件中写函数的声明
4. 在源文件中写函数的定义

```C++
// swap.h 头文件
#include<iostream>
using namespace std;
void swap(int num1, int num2) {
    int temp = num1;
    num1 = num2;
    num2 = temp;
    cout << "交换后：" << endl;
    cout << "num1=" << num1 << endl;
    cout << "num2=" << num2 << endl;
}
```

```C++
// .cpp 文件
#include<iostream>
#include "headfiles/swap.h" // 引入头文件
using namespace std;

int main() {
    int num1, num2;
    cin >> num1 >> num2;
    cout << "交换前实参：" << endl;
    cout << "num1=" << num1 << endl;
    cout << "num2=" << num2 << endl;
    swap(num1, num2);
    cout << "交换后实参：" << endl;
    cout << "num1=" << num1 << endl;
    cout << "num2=" << num2 << endl;
    return 0;
}
```

## 7 指针

### 7.1 指针的基本概念

**指针的作用：**可以通过指针间接访问内存

- 内存编号是从0开始记录的，一般用十六进制数字表示
- 可以利用指针变量保存地址



### 7.2 指针变量的定义和使用

**指针变量定义语法：**`数据类型 * 变量名;`

```C++
#include<iostream>
using namespace std;

int main() {
    // 定义整型变量a
    int a = 10;
    // 定义指针
    int *p;
    // 指针变量赋值
    p = &a;// 指针变量指向变量a的地址
    
    // 通过*操作指针变量指向的内存
    cout << "*p=" << *p << endl;
    cout << "p=" << p << endl;
    cout << "a的地址为=" << &a << endl;
    return 0;
}
```



### 7.3 指针所占内存空间

32位操作系统下为4字节，64位系统下为8字节。

```C++
#include<iostream>
using namespace std;

int main() {
    int a = 10;
    int *p = &a;
    cout << sizeof(*p) << endl;// 4
    cout << "sizeof(p)=" << sizeof(p) << endl; // 8
    cout << "char * 所占内存大小：" << sizeof(char *) << endl;		// 8
    cout << "int * 所占内存大小：" << sizeof(int *) << endl;		// 8
    cout << "float * 所占内存大小：" << sizeof(float *) << endl;	// 8
    cout << "double * 所占内存大小：" << sizeof(double *) << endl;	// 8
    return 0;
}
```



### 7.4 空指针和野指针

**空指针：**指针变量指向内存中编号为0的空间

**用途：**初始化指针变量

**注意：**空指针指向的内存是不可以访问的

> 0~255之间的内存编号为系统占用，不可以访问



**野指针：**指针变量指向非法的内存空间

```C++
#include<iostream>
using namespace std;

int main() {
    int *p = (int *) 0x1100;
    // 访问野指针报错
    cout << *p << endl;
    return 0;
}
```

> 总结：空指针和野指针都不是我们申请的内存，因此不要访问



### 7.5 const修饰指针

const修饰指针有三种情况：

1. const修饰指针 ---常量指针

   1. 特点：指针的指向可以修改，但指针指向的值不可以修改

   2. ```C++
      int a = 10;
      int b = 20;
      const int *p = &a;
      *p = 20;// 错的
      p = &b;// 对的
      ```

2. const修饰常量 ---指针常量

   1. 特点：指针的指向不可以改，指向的值可以改

   2. ```C++
      int * const p = &a;
      *p = 20; // 对的
      p = &b;  // 错的
      ```

3. const既修饰指针，又修饰常量

   1. ```C++
      const int * const p = &a;
      *p = 20; // 错的
      p = &b;	 // 错的
      ```

      

### 7.6 指针和数组

**作用：**利用指针访问数组中元素

```C++
#include<iostream>
using namespace std;

int main() {
    int array[] = {1, 2, 3, 4, 5, 6, 7};
    int *p = array;// *p指向array[0]
    cout << "array[0]=" << array[0] << endl;
    // 利用指针访问数组首元素
    cout << "*p=" << *p << endl;
    for (int i = 0; i < sizeof(array) / sizeof(array[0]); i++) {
        //利用指针遍历数组
        cout << *p << endl;
        p++;
    }
    return 0;
}
```



### 7.7 指针和函数

**作用：**利用指针做函数参数，可以修改实参的值

> 值传递：值传递不会改变实参

> 地址传递：地址传递会修改实参

```C++
#include<iostream>
using namespace std;
// 值传递
int swap(int num1, int num2) {
    int temp = num1;
    num1 = num2;
    num2 = temp;
}
// 地址传递
int swap(int *num1, int *num2) {
    int temp = *num1;
    *num1 = *num2;
    *num2 = temp;
}

int main() {
    int a = 10;
    int b = 20;
    cout << "初始值：" << endl;
    cout << "a=" << a << endl;
    cout << "b=" << b << endl;
    cout << "&a=" << &a << endl;
    cout << "&b=" << &b << endl;
    // 值传递
    swap(a, b);
    cout << "值传递后：" << endl;
    cout << "a=" << a << endl;
    cout << "b=" << b << endl;
    cout << "&a=" << &a << endl;
    cout << "&b=" << &b << endl;
    // 地址传递
    swap(&a, &b);
    cout << "地址传递后：" << endl;
    cout << "a=" << a << endl;
    cout << "b=" << b << endl;
    cout << "&a=" << &a << endl;
    cout << "&b=" << &b << endl;
    return 0;
}
```

> 值传递后实参的值可能会改变，但是实参的地址不变



## 8 结构体

### 8.1 结构体基本概念

结构体属于用户**自定义的数据类型**，允许用户存储不同的数据类型



### 8.2 结构体的定义和使用

**语法：**`struct 结构体名 { 结构体成员列表 };`

通过结构体创建变量的方式有三种：

1. struct 结构体名 变量名;
2. struct 结构体名 变量名 = {成员1值,成员2值,...}
3. 定义结构体是顺便创建结构体变量

```C++
#include<iostream>
#include<string>

using namespace std;
struct student {
    string name;
    int age;
    string sex;
} s3;// s3 为结构体变量
int main() {
    // 第一种定义方式 struct 结构体名 变量名;
    // struct 可以省略
    //struct student s1;
    student s1;
    s1.name = "XYY";
    s1.age = 10;
    s1.sex = "男";
    cout << s1.name << "是个" << s1.age << "岁的" << s1.sex << "孩儿。" << endl;

    //第二种定义方式 struct 结构体名 变量名 = {成员1值,成员2值,...}
    struct student s2 = {"DYY", 12, "女"};
    cout << s2.name << "是个" << s2.age << "岁的" << s2.sex << "孩儿。" << endl;

    // 第三种定义方式 定义结构体是顺便创建结构体变量
    s3.name = "YYY";
    s3.age = 14;
    s3.sex = "女";
    cout << s3.name << "是个" << s3.age << "岁的" << s3.sex << "孩儿。" << endl;

    return 0;
}
```

> 总结1：定义结构体时的关键字是struct，不可省略

> 总结2：创建结构体变量时，关键字struct可以省略

> 总结3：结构体变量利用操作符"."访问成员



### 8.3 结构体数组

**作用：**将自定义的结构体放入到数组中方便维护

**语法：**`struct 结构体名 数组名[元素个数] = { {} ,{} ,...};`

```C++
#include<iostream>
#include<string>
using namespace std;
struct student {
    string name;
    int age;
    string sex;
};

int main() {
    // 创建结构体数组
    struct student students[5] = {
            {"XYY", 10, "男"},
            {"DYY", 12, "女"},
            {"YYY", 14, "女"}
    };
    // 给结构体数组中的元素赋值
    students[3] = {"ZZZ", 16, "男"};

    students[4].name = "XXX";
    students[4].age = 18;
    students[4].sex = "男";
    // 遍历结构体数组
    for (int i = 0; i <5;i++){
        cout << students[i].name << "是个" << students[i].age << "岁的" << students[i].sex << "孩儿。" << endl;
    }
    return 0;
}
```



### 8.4 结构体指针

**作用：**通过指针访问结构体中成员

- 利用操作符`->`可以通过结构体指针访问结构体属性 

```C++
#include<iostream>
#include<string>

using namespace std;
struct student {
    string name;
    int age;
    string sex;
};

int main() {
    student s = {"ZZZ", 16, "男"};
    student *p = &s;
    // 通过操作符 -> 访问成员变量
    cout << p->name << "是个" << p->age << "岁的" << p->sex << "孩儿。" << endl;
    return 0;
}
```



### 8.5 结构体嵌套结构体

**作用：**结构体中的成员可以是另一个结构体

```C++
#include<iostream>

using namespace std;

struct phone {
    string type;
    string number;
};

struct person {
    string name;
    int age;
    string sex;
    struct phone phone;
};

int main() {
    struct person p1 = {"XYY", 11, "男", {"HUAWEI nova5Pro", "17539776800"}};
    cout << p1.name << " " << p1.age << " " << p1.sex << " " <<p1.phone.type << " " << p1.phone.number << endl;
    return 0;
}
```



### 8.6 结构体做函数参数

**作用：**将结构体作为参数向函数中传递

传递方式有两种：

- 值传递
- 地址传递

```C++
#include<iostream>
#include<string>

using namespace std;

struct student {
    string name;
    int age;
};

// 值传递
void printStudent(student student) {
    cout << "name:" << student.name << ";age:" << student.age << endl;
};

// 地址传递
void printStudent(student *student) {
    cout << "name:" << student->name << ";age:" << student->age << endl;
}

int main() {
    struct student student = {"XYY", 11};
    // 值传递
    printStudent(student);
    // 地址传递
    printStudent(&student);
    return 0;
}
```

> 注意：使用值传递时，会拷贝一份数据给函数;而使用地址传递时不会拷贝数据，减少了内存的使用





### 8.7 结构体中const使用场景

**作用：**用const来防止误操作

```C++
#include<iostream>
#include<string>

using namespace std;

struct student {
    string name;
    int age;
};

// const修饰，防止误操作
void printStudent(const student *student) {
    //student->age = 11; // 操作失败，因为加入了const修饰
    cout << "name:" << student->name << ";age:" << student->age << endl;
}

int main() {
    struct student student = {"XYY", 11};
    printStudent(&student);
    return 0;
}
```



## 9 案例

### 9.1、生成随机数

```C++
#include<iostream>
#include<ctime>
using namespace std;

int main() {
    srand((unsigned) time(NULL));
    int ran = rand() % 100 + 1;
    for (int i = 0; i < 100; i++) {
        cout << (rand() % 100 + 1) << endl;
    }

    return 0;
}
```

### 9.2、练习案例

#### 9.2.1猜数字游戏

```C++
#include<iostream>
#include<ctime>

using namespace std;

int main() {
    srand((unsigned) time(NULL));
    int ran = rand() % 100 + 1;
    int ci = 0;
    while (ci != ran) {
        cout << "请输入您要猜的数：";
        cin >> ci;
        if (ci > ran) {
            cout << "猜大了" << endl;
        } else if (ci < ran) {
            cout << "猜小了" << endl;
        }
    }
    // 跳出循环说明猜对了
    cout << "生成的随机数为：" << ran << endl;
    cout << "恭喜您获胜" << endl;
    return 0;
}
```



#### 9.2.2 水仙花数

**案例描述：**水仙花数是指一个三位数，它的每个位上的数字的三次幂等于它本身

例如：1^3+5^3+3^3 = 153

请利用do...while语句，求出所有三位数中的水仙花数

```C++
#include<iostream>
using namespace std;

int main() {
    int i = 100;// 初始值
    do {
        int gw = i % 10;// 获取个位数
        int sw = i / 10 % 10;//获取十位数
        int bw = i / 100; //获取百位数
        if ((gw * gw * gw + sw * sw * sw + bw * bw * bw) == i) {
            cout << i << endl;
        }
        i++;
    } while (i < 1000);
    return 0;
}
```

#### 9.2.3 敲桌子

**描述：**从1开始数到数字100，如果数字中含有7或者是7的倍数，打印“敲桌子”，其余数字直接打印输出。

```C++
#include<iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 100; i++) {
        if (i % 10 == 7 || i / 10 == 7 || i % 7 == 0) {
            cout << "敲桌子:" << i << endl;
        } else {
            cout << i << endl;
        }
    }
    return 0;
}
```

#### 9.2.4 打印九九乘法表

```C++
#include<iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 9; i++) {
        for (int j = 1; j <= i; j++) {
            cout << i << "*" << j << "=" << i * j << "\t";
        }
        cout << endl;
    }
    return 0;
}
```

#### 9.2.5 五只小猪称体重

```C++
#include <iostream>
using namespace std;

int main() {

    int arr[5] = {300, 350, 300, 400, 250};
    int max = 0;
    for (int i = 0; i < 5; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    cout << "最重的小猪体重：" << max << endl;
    return 0;
}
```

#### 9.2.6 元素逆置

一个数组中若干个元素，将数组中元素逆置。

```C++
#include <iostream>
using namespace std;

int main() {

    int arr[] = {1, 3, 2, 5, 4};
    int start = 0;//起始下标
    int end = sizeof(arr) / sizeof(arr[0]) - 1;//结尾下标
    do {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    } while (start < end);
    // 打印逆置元素
    for (int i = 0; i < (sizeof(arr) / sizeof(arr[0])); i++) {
        cout << arr[i] << endl;
    }
    return 0;

```



#### 9.2.7 指针、数组、函数 实例

**案例描述：**封装一个函数，利用冒泡排序，实现对整型数组的升序排序

例如数组：int arr[10] = {4,3,6,9,1,2,10,8,7,5};

```C++
#include<iostream>

using namespace std;
// 冒泡排序
void bubbleSort(int *arr, int len) {
    for (int i = 0; i < len - 1; i++) {
        for (int j = 0; j < len - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
};
// 打印数组
void printArray(int *arr, int len) {
    for (int i = 0; i < len; i++) {
        cout << arr[i] << " ";
    }
}

int main() {
    int arr[10] = {4, 3, 6, 9, 1, 2, 10, 8, 7, 5};
    int len = sizeof(arr) / sizeof(arr[0]);
    cout << "排序前：" << endl;
    printArray(arr, len);
    cout << endl;
    bubbleSort(arr, len);
    cout << "排序后：" << endl;
    printArray(arr, len);
    return 0;
}
```



#### 9.2.8 结构体案例

设计一个英雄的结构体，包括成员姓名，年龄，性别；创建结构体数组，数组中存放5名英雄。
通过冒泡排序的算法，将数组中的英雄按照年龄进行升序排序，最终打印排序后的结果。

```C++
#include<iostream>
#include<string>

using namespace std;

struct hero {
    string name;
    int age;
    string sex;
};

void bubbleSort(hero *arrHero, int len) {
    for (int i = 0; i < len - 1; i++) {
        for (int j = 0; j < len - i - 1; j++) {
            if (arrHero[j].age > arrHero[j + 1].age) {
                hero temp = arrHero[j];
                arrHero[j] = arrHero[j + 1];
                arrHero[j + 1] = temp;
            }
        }
    }
}

void printHero(hero *arrHero, int len) {
    for (int i = 0; i < len; i++) {
        cout << "姓名：" << arrHero[i].name << ";年龄：" << arrHero[i].age << ";性别：" << arrHero[i].sex << endl;
    }
}

int main() {
    struct hero arrHero[5] = {
            {"刘备",   20, "男"},
            {"关羽",   22, "男"},
            {"不知火舞", 18, "女"},
            {"诸葛亮",  16, "男"},
            {"小乔",   17, "女"}
    };
    int len = sizeof(arrHero) / sizeof(arrHero[0]);
    bubbleSort(arrHero, len);
    printHero(arrHero, len);
    return 0;
}
```



# 通讯录管理系统

## 1.系统需求

通讯录是一个可以记录亲人、好友信息的工具。
本教程主要利用C++来实现一个通讯录管理系统
系统中需要实现的功能如下：

- 添加联系人：向通讯录中添加新人，信息包括(姓名、性别、年龄、联系电话、家庭住址)最多记录1000人。
- 显示联系人：显示通讯录中所有联系人信息
- 删除联系人：按照姓名进行删除指定联系人
- 查找联系人：按照姓名查看指定联系人信息
- 修改联系人：按照姓名重新修改指定联系人
- 清空联系人：清空通讯录中所有信息
- 退出通讯录：退出当前使用的通讯录











# C++核心编程

## 1 内存分区模型

C++程序执行时，将内存大方向划分为4个**区域**

- 代码区：存放函数体的二进制代码，由操作系统进行管理
- 全局区：存放全局变量和静态变量以及常量
- 栈区：由编译器自动分配释放，存放函数的参数值、局部变量等
- 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

**内存分区的意义：**
不同区域存放的数据，赋予不同的生命周期，给我们更大的灵活编程

### 1.1 程序运行前
在程序编译后，生成exe可执行程序，**未执行该程序前**分为两个区域
**代码区：**
	存放CPU执行的机器指令
	代码区是**共享**的，共享的目的是对于频繁被执行的程序，只要在内存中有一份代码即可
	代码区是**只读**的，使其只读的原因是防止程序意外地修改了它的命令
**全局区：**
    全局变量和静态变量存放在此
    全局区还包含了常量区，字符串常量和其他常量也存放在此
    ==该区域的数据在程序执行结束后由操作系统释放==

**示例：**


```C++
#include<iostream>

using namespace std;
// 全局变量
int g_a = 10;
int g_b = 10;
// const 修饰的全局变量
const int c_g_a = 10;
const int c_g_b = 10;

int main() {
    // 局部变量
    int a = 10;
    int b = 10;
    cout << "局部变量a的地址为：\t\t" << &a << endl;
    cout << "局部变量b的地址为：\t\t" << &b << endl;
    // 全局变量
    cout << "全局变量g_a的地址为：\t" << &g_a << endl;
    cout << "全局变量g_b的地址为：\t" << &g_b << endl;
    // 静态变量
    static int s_a = 10;
    static int s_b = 10;
    cout << "静态变量s_a的地址为：\t" << &s_a << endl;
    cout << "静态变量s_b的地址为：\t" << &s_b << endl;
    //常量
    // 字符串常量
    cout << "字符串常量的地址为：\t" << &"Hello" << endl;
    cout << "字符串常量的地址为：\t" << &"World！" << endl;
    // const 修饰的局部变量
    const int c_l_a = 10;
    const int c_l_b = 10;
    cout << "const修饰的局部变量c_l_a的地址为：\t" << &c_l_a << endl;
    cout << "const修饰的局部变量c_l_b的地址为：\t" << &c_l_b << endl;
    // const 修饰的全局变量
    cout << "const修饰的全局变量c_l_b的地址为：\t" << &c_g_a << endl;
    cout << "const修饰的全局变量c_l_b的地址为：\t" << &c_g_b << endl;
    return 0;
}
```

**输出：**

```C++
局部变量a的地址为：		0xdd47fffb1c
局部变量b的地址为：		0xdd47fffb18
全局变量g_a的地址为：	0x7ff66a743010
全局变量g_b的地址为：	0x7ff66a743014
静态变量s_a的地址为：	0x7ff66a743018
静态变量s_b的地址为：	0x7ff66a74301c
字符串常量的地址为：	0x7ff66a7440f0
字符串常量的地址为：	0x7ff66a7440f6
const修饰的局部变量c_l_a的地址为：	0xdd47fffb14
const修饰的局部变量c_l_b的地址为：	0xdd47fffb10
const修饰的全局变量c_l_b的地址为：	0x7ff66a744004
const修饰的全局变量c_l_b的地址为：	0x7ff66a744008
```

### 1.2 程序运行后
**栈区：**
	由编译器自动分配释放，存放函数的参数值、局部变量等
	注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```C++
#include<iostream>

using namespace std;

int *func() {
    int a = 10;
    int *b = &a;
    return b;
}

int main() {
    int *p = func();
    cout << *p << endl;	// 输出10，这是因为编译器会做一个保存，但是不会一直保存
    cout << *p << endl; // 输出乱码
    return 0;
}
```

**堆区：**
	由程序员分配释放，若程序员不释放，程序结束时由操作系统回收
	在C++中主要利用new在堆区开辟内存

### 1.3 new关键字

C++中利用new操作符在堆区开辟数据

堆区开辟的数据由程序员手动删除、手动释放(利用操作符delete)

**语法：**`new 数据类型`

利用new创建的数据，会返回该数据对应的类型的指针

释放空间 delete

``` C++
int *p = new int(10);
delete p;
// 在堆区开辟数组
int *arr = new int[10];
delete[] arr;
```

## 2引用
### 2.1 引用的基本使用
**作用：**给变量起别名
**语法：**`数据类型 &别名 = 原名`
```C++
#include<iostream>

using namespace std;

int main() {
    //引用基本语法
    //数据类型 &别名 = 原名;
    int a = 10;
    cout << a << endl;
    // 创建引用
    int &b = a;// 变量b指向变量a的地址
    cout << b << endl;
    b = 20;
    cout << a << endl;
    return 0;
}

```

### 2.2 引用的注意事项

- 引用必须初始化
- 引用在初始化后，不可以改变

```C++
#include<iostream>

using namespace std;

int main() {
    int a = 10;
    cout << "a:" << a << endl;
    //int &b //错误的,必须初始化
    int &c = a;
    cout << "c:" << c << endl;
    c = 100;
    cout << "a:" << a << endl;
    cout << "c:" << c << endl;
    int d = 20;
    cout << "d:" << d << endl;
    //int &c = d;//错误的，引用不能改变
    c = d; //这是赋值操作，不是更改引用
    cout << "c:" << c << endl;
    d = 22;
    cout << "d:" << d << endl;
    cout << "c:" << c << endl;
    return 0;
}
```

### 2.3 引用做函数参数

**作用：**函数传参时，可以利用引用的技术让形参修饰实参

**优点：**可以简化指针修改实参

```C++
#include<iostream>

using namespace std;

//1、值传递
void swap01(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}

//2、地址传递
void swap02(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

//3、引用传递
void swap03(int &a1, int &b1) {
    int temp = a1;
    a1 = b1;
    b1 = temp;
}

int main() {
    int a = 10;
    int b = 20;
    cout << "**********值传递**********" << endl;
    cout << "交换前：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    swap01(a, b);
    cout << "交换后：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    cout << "**********地址传递**********" << endl;
    cout << "交换前：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    swap02(&a, &b);
    cout << "交换后：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    cout << "**********引用传递**********" << endl;
    cout << "交换前：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    swap03(a, b);
    cout << "交换后：" << endl;
    cout << "a:" << a << " b:" << b << endl;
    return 0;
}
```

> 总结：通过引用参数产生的效果同按地址传递是一样的，引用的语法更清楚简单。

### 2.4 引用做函数返回值

**作用：**引用是可以作为函数的返回值存在的

**注意：**==不要返回局部变量引用==

**用法：**函数调用作为左值

```C++
//返回局部变量引用
int &test01() {
    int a = 10;
    int &b = a;
    return b;
}

//返回静态变量引用
int &test02() {
    static int a = 10;
    return a;
}

int main() {
    //不能返回局部变量的引用
    int &ref1 = test01();
    cout << ref1 << endl;//第一次结果正确，因为编译器做了保留
    cout << ref1 << endl;//第二次结果错误，因为内存已将释放

    int &ref2 = test02();
    cout << ref2 << endl;
    cout << ref2 << endl;
    //如果函数的返回值是引用，那么这个函数可以作为左值
    test02() = 1000;
    cout << ref2 << endl;
    cout << ref2 << endl;
    return 0;
}
```

### 2.5 引用的本质

本质：==引用的本质在C++内部实现是一个指针常量==

![image-20220505184722523](D:\notes\笔记\后端\C++.assets\image-20220505184722523.png)

> 结论：C++推荐用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了

### 2.6 常量引用

**作用：**常量引用主要用来修饰形参，防止误操作
在函数形参列表中，可以加const修饰形参，防止形参改变实参

```C++
//常量引用
//使用场景：用来修饰形参，防止误操作
void showValue(const int &val) {
    //val = 1000;
    cout << "val=" << val << endl;
}

int main() {

    //int &ref = 10;//引用必须引一块合法的内存空间,这行错误
    //加上const之后，编译器将代码修改
    //int temp = 10; const int &ref = temp;
    //const int &ref = 10;
    //ref = 11;//加入const之后变为只读，不可以修改

    //函数中利用常量引用防止误操作修改实参
    int a = 100;
    showValue(a);
    return 0;
}
```

## 3 函数提高

### 3.1 函数默认参数

在C++中，函数的形参列表中的形参是可以有默认值的。

**语法：**`返回值类型 函数名 (参数 = 默认值) {}`

```C++
// 函数默认参数
//如果我们自己传入数据，就用自己的数据，如果没有，那么用默认值
//语法：返回值类型 函数名 (形参 = 默认值) {}
int sum(int a = 10, int b = 100, int c = 1000) {
    return a + b + c;
}

//注意事项：
//1、如果某个位置已经有了默认参数，那么从这个位置往后，从左到右都必须有默认值
//2、如果函数声明有默认参数，函数实现就不能有默认参数
//  声明和实现只能有一个有默认参数

int main() {
    int s0 = sum();//1110
    int s1 = sum(1);//1101
    int s2 = sum(1, 2);//1003
    int s3 = sum(1, 2, 3);//6

    return 0;
}
```

### 3.2 函数占位参数

C++函数的形参列表里可以有占位参数，用来做占位，调用该函数时必须填补该位置。

**语法：**`返回值类型 函数名 （数据类型) {}`

```C++
//目前用不到占位参数
//占位参数还可以有默认参数
void func1(int a, int) {
    cout << "*****************" << endl;
}

void func2(int a, int = 10) {
    cout << "*******" << endl;
}

int main() {
    func1(10, 20);
    func2(10);
    return 0;
}
```

### 3.3 函数重载

#### 3.3.1 函数重载概述

**作用：**函数名可以相同，提高重用性

**函数重载满足条件：**

- 同一个作用域下
- 函数名称相同
- 函数参数**类型不同** or **个数不同**  or (参数类型)**顺序不同**

**注意：**函数的返回值不可以作为函数重载的条件

```C++
void func() {
    cout << "*" << endl;
}

void func(int a) {
    cout << "**" << endl;
}

void func(char a) {
    cout << "***" << endl;
}

void func(int a, char b) {
    cout << "****" << endl;
}

void func(char b, int a) {
    cout << "*****" << endl;
}

int main() {
    func();
    func(1);
    func('a');
    func(1, 'c');
    func('c', 1);
    return 0;
}
```

#### 3.3.2 函数重载注意事项

- 引用作为重载条件
- 函数重载碰到函数默认参数

```C++
//1、引用作为重载的条件
void func(int &a) {
    cout << "func(int &a)调用" << endl;
}

void func(const int &b) {
    cout << "const func(int &b)调用" << endl;
}

//2、函数重载碰到默认参数
void func1(int a, int b = 10) {
    cout << "func(int a)调用" << endl;
}

void func1(int a) {
    cout << "func(int a = 10)调用" << endl;
}

int main() {
    int a = 10;
    func(a);//调用无const
    const int b = 10;
    func(b);//调用有const
    func(1);//调用有const

    //func1(10);//碰到默认参数产生歧义，需要避免
    return 0;
}

```

## 4 类和对象

C++面向对象三大特性：==封装==、==继承==、==多态==

C++认为==万事万物皆为对象==，对象上有其属性和行为

具有==相同性质==的对象，我们称之为==类==

### 4.1 封装

#### 4.1.1 封装的意义

封装是C++面向对象的三大特性之一

**封装的意义：**

- 将属性和行为作为一个整体，表现生活中的事物
- 将属性和行为加以权限控制

**封装的意义一：**

- 在设计类的时候，属性和行为写在一起，表现事物

**语法：**`class 类名 {访问权限: 属性 / 行为}`

**示例：**

```C++
//设计一个圆类，求周长
class Circle {
//访问权限
//私有权限
private:
    double PI = 3.14;
//公共权限
public:
    //属性
    //半径
    double m_r;

    //行为
    //获取圆的周长
    double calculateZC() {
        return 2 * PI * m_r;
    }
};

int main() {
    //通过类创建对象(实例化)
    Circle circle;
    //给对象赋值
    circle.m_r = 10;
    //调用对象的行为
    cout << "圆的周长为：" << circle.calculateZC() << endl;
    return 0;
}
```

> 实例化：`类名 对象名;`

**示例2：**

设计一个学生类，属性有姓名和学号，可以给姓名和学号赋值，可以显示学生的姓名和学号。

```C++
class Student {
    public:
    //类中的属性和行为统称为 成员
    //属性：成员属性、成员变量
    //行为：成员函数、成员方法

    //属性
    string m_Name;
    string m_Id;

    //方法
    void toString() {
        cout << "姓名：" << m_Name << ";学号：" << m_Id << endl;
    }

    void setName(string name) {
        m_Name = name;
    }

    void setId(string id) {
        m_Id = id;
    }

    string getName() {
        return m_Name;
    }

    string getId() {
        return m_Id;
    }
};

int main() {
    Student student;
    student.setName("东方不败");
    student.setId("10000001");
    student.toString();
    return 0;
}
```

**封装的意义二：**

类在设计的时候可以把属性和行为放到不同的权限下，加以控制

**访问权限**有三种：

1. public        公共权限	类内可以访问，类外也可以访问
2. protected 保护权限    类内可以访问，类外不可以访问，子类可以访问父类的保护内容
3. private      私有权限    类内可以访问，类外不可以访问

#### 4.1.2 struct和class的区别

在C++中struct和class唯一的区别在于**默认的访问权限不同**

区别：

- struct默认权限为公共
- class默认权限为私有

```C++
struct C1 {
    int m_a;

    void toString() {
        cout << "struct m_a:" << m_a << endl;
    }
};

class C2 {
    int m_a;
};

int main() {
    //struct和class区别
    //struct默认权限是公共 public
    //class 默认权限是私有 private
    C1 c1;
    c1.m_a = 10;//在struct默认的权限为公共，因此可以访问
    C2 c2;
    //c2.m_a = 11;//在class默认的权限为私有，因此不可以访问
    return 0;
}
```

#### 4.1.3 成员变量设置为私有

**优点1：**将所有成员属性设置为私有，可以自己控制读写权限

**优点2：**对于写权限，我们可以检测数据的有效性

```C++
#include<iostream>
#include<string>

using namespace std;

class Person {
public:
    //写姓名
    void setName(string name) {
        m_Name = name;
    }

    //读姓名
    string getName() {
        return m_Name;
    }

    //设置年龄
    void setAge(int age) {
        if (age < 0 || age > 150) {
            m_Age = -1;
            cout << "年龄有误！" << endl;
            return;
        }
        m_Age = age;
    }


    //获取年龄
    int getAge() {
        return m_Age;
    }

    //设置情人
    void setLover(string lover) {
        m_Lover = lover;
    }

    void toString() {
        cout << "姓名：" << m_Name <<
             "\n年龄：" << m_Age <<
             "\n情人：" << m_Lover << endl;
    }

private:
    //姓名    可读可写
    string m_Name;
    //年龄    可读可写
    int m_Age;
    //情人    只写
    string m_Lover;
};

int main() {
    Person person;
    person.setName("Eric");
    person.setAge(99);
    person.setLover("Lily");
    person.toString();
    return 0;
}
```

#### 4.1.4 案例：设计立方体类

设计立方体类(Cube)

求出立方体的面积和体积

分别用全局函数和成员函数判断两个立方体是否相等

```C++
#include<iostream>

using namespace std;

class Cube {
public:
    //设置长度
    void setM_L(int l) {
        if (l <= 0) {
            l = 0;
            cout << "长度小于等于0，错误！" << endl;
            return;
        }
        m_L = l;
    }

    //获取长度
    int getM_L() {
        return m_L;
    }

    //设置宽度
    void setM_W(int w) {
        if (w <= 0) {
            w = 0;
            cout << "宽度小于等于0，错误！" << endl;
            return;
        }
        m_W = w;
    }

    //获取宽度
    int getM_W() {
        return m_W;
    }

    //设置高度
    void setM_H(int h) {
        if (h <= 0) {
            h = 0;
            cout << "长度小于等于0，错误！" << endl;
            return;
        }
        m_H = h;
    }

    //获取高度
    int getM_H() {
        return m_H;
    }

    //获取全部属性
    void toString() {
        cout << "长：" << m_L << "  宽：" << m_W << "  高：" << m_H << endl;
    }

    //获取表面积
    int getArea() {
        return m_L * m_W * 2 + m_L * m_H * 2 + m_W * m_H * 2;
    }

    //获取立方体体积
    int getVolume() {
        return m_L * m_W * m_H;
    }

    //判断两个立方体是否相等
    bool isSame(Cube cube) {
        if (m_L == cube.getM_L() && m_H == cube.getM_H() && m_W == cube.getM_W()) {
            cout << "两个立方体相等。" << endl;
            return true;
        } else {
            cout << "两个立方体不相等。" << endl;
            return false;
        }
    }

private:
    int m_L;//长
    int m_W;//宽
    int m_H;//高
};

bool isSame(Cube c1, Cube c2) {
    if (c1.getM_L() == c2.getM_L() && c1.getM_H() == c2.getM_H() && c1.getM_W() == c2.getM_W()) {
        cout << "两个立方体相等。" << endl;
        return true;
    } else {
        cout << "两个立方体不相等。" << endl;
        return false;
    }
}

int main() {
    Cube c1;
    c1.setM_L(10);
    c1.setM_W(1);
    c1.setM_H(10);
    c1.toString();

    Cube c2;
    c2.setM_L(10);
    c2.setM_W(1);
    c2.setM_H(10);
    c2.toString();
    //成员函数判断
    c1.isSame(c2);
    //全局函数判断
    isSame(c1, c2);
    return 0;
}
```

#### 4.1.5 案例：点和圆的关系

设计一个圆类(Circle),和一个点类(Point)，计算点和圆的关系。

### 4.2 对象的初始化和清理

- 生活中我们买的电子产品都基本会有出厂设置，在某一天我们不用时候也会删除一些自己的信息数据，保证安全。
- C++中的面向对象来源于生活，每个对象也都会有初始设置以及对象销毁前的清理数据的设置。

#### 4.2.1 构造函数和析构函数

对象的**初始化和清理**也是两个非常重要的安全问题

- 一个对象或者变量没有初始状态，对其使用后果是未知的。

- 同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题。



C++利用了**构造函数和析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。

对象的初始化和清理工作是**编译器强制**要我们做的事情，因此如果我们**不提供构造和析构，编译器会提供**

**编译器提供的构造函数和析构函数是空实现。**

- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无需手动调用。
- 析构函数：主要作用在于对象销毁前自动调用，执行一些清理工作。



**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值，也不写void
2. 函数名称与类名相同
3. 构造函数**可以有参数**，因此可以发生**重载**
4. 程序在调用对象时会自动调用构造，无需手动调用，而且只会调用一次

**析构函数：**`~类名(){}`

1. 析构函数，没有返回值，也不写void
2. 函数名称与类名相同，在名称前加上符号~
3. 析构函数**不可以有函数**，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无需手动调用，而且只会调用一次

```C++
class Person {

public:
    //构造函数
    Person() {
        cout << "执行构造函数" << endl;
    }

    //析构函数
    ~Person() {
        cout << "析构函数" << endl;
    }
};
```

#### 4.2.2 构造函数的分类及调用

两种分类方式：

- 按参数分：**有参构造**和**无参构造**
- 按类型分：**普通构造**和**拷贝构造**

三种调用方式：

- 括号法
- 显示法
- 隐式转换法

**注意事项：**

1. 调用默认构造函数的的时候不要加()
2. 不要用拷贝构造函数构造匿名对象

```C++
class Person {

public:
    //普通构造
    //无参构造(默认构造)
    Person() {
        cout << "执行无参构造函数" << endl;
    }

    //有参构造
    Person(int a) {
        m_age = a;
        cout << "执行有参构造函数" << endl;
    }

    //拷贝构造
    Person(const Person &p) {
        //将拷贝对象的所有属性全部传到新建对象的身上
        cout << "执行拷贝参构造函数" << endl;
        m_age = p.m_age;
    }

    //析构函数
    ~Person() {
        cout << "执行析构函数" << endl;
    }

//private:
    int m_age;
};

//函数调用
void test01() {
    //1、括号法
    Person p1;
    Person p2(10);
    Person p3(p2);
    cout << "p2的年龄为：" << p2.m_age << endl;
    cout << "p3的年龄为：" << p3.m_age << endl;
}

void test02() {
    //2、显示法
    Person p1;
    Person p2 = Person(10);
    Person p3 = Person(p2);
    //匿名对象
    //特点：当前行执行结束后，系统会立即回收掉匿名对象
    Person(20);
    //Person(p2);//Person(p2) === Person p2;对象声明
}

void test03() {
    //3、隐式转换法
    Person p2 = 10; //相当于Person p2 = Person(10);
    Person p3 = p2; //相当于Person p3 = Person(p2);
    cout << "p2的年龄为：" << p2.m_age << endl;
    cout << "p3的年龄为：" << p3.m_age << endl;
}
```

#### 4.2.3 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况：

- 同时用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数参数传值
- 以值方式返回局部对象

```C++
#include<iostream>

using namespace std;

class Person {
public:
    int m_Age;

    //构造函数
    //无参构造
    Person() {
        cout << "Person调用无参构造函数" << endl;
    }

    //有参构造
    Person(int age) {
        m_Age = age;
        cout << "Person调用有参构造函数" << endl;
    }

    //拷贝构造
    Person(const Person &p) {
        cout << "Person调用拷贝构造函数" << endl;
        m_Age = p.m_Age;
    }

    ~Person() {
        cout << "Person调用析构函数" << endl;
    }
};

//1、使用一个已经创建的对象来初始化一个新对象
void test01() {
    Person p1(20);
    Person p2 = Person(p1);
}

//2、值传递方式给函数参数传值
void doWork1(Person p) {

}

void test02() {
    Person p;
    doWork1(p);
}

//3、以值的方式返回局部对象
Person doWork2() {
    Person p1;
    cout << &p1 << endl;
    return p1;
}

void test03() {
    Person p2 = doWork2();
    cout << &p2 << endl;

}

int main() {
    test03();
    return 0;
}
```

#### 4.2.4 构造函数调用规则

默认情况下，C++编译器至少给一个类添加三个函数：

1. 默认构造函数(无参，函数体为空)
2. 默认析构函数(无参，函数体为空)
3. 默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则如下：

- 如果用户定义有构造函数，C++不再提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数，C++不会再提供其他构造函数

#### 4.2.5 深拷贝与浅拷贝

深拷贝与浅拷贝是面试经典问题，也是常见的一个坑

- 深拷贝：在堆区重新申请空间，进行拷贝工作
- 浅拷贝：简单的赋值拷贝操作

```C++
#include<iostream>

using namespace std;

//深拷贝：在堆区重新申请空间，进行拷贝
//浅拷贝：简单的赋值拷贝
class Person {
public:
    Person() {
        cout << "执行无参构造函数" << endl;
    }

    Person(int age, int height) {
        m_Age = age;
        m_Height = new int(height);
        cout << "执行无参构造函数" << endl;
    }

    //深拷贝
    Person(const Person &p) {
        cout << "执行拷贝构造函数" << endl;
        m_Age = p.m_Age;
        m_Height = new int(*p.m_Height);
    }
    //浅拷贝(编译器默认提供的拷贝)
    //Person(const Person &p) {
    //    cout << "执行拷贝构造函数" << endl;
    //    m_Age = p.m_Age;
    //    m_Height = p.m_Height;
    //}

    ~Person() {
        //析构函数释放堆取数据
        if (m_Height != NULL) {
            delete m_Height;//浅拷贝执行该操作可能会报错，需要深拷贝
            m_Height = NULL;
        }
        cout << "执行析构函数" << endl;
    }

    int m_Age;
    int *m_Height;
};

void test01() {
    Person p1(18, 180);
    cout << "p1的年龄为：" << p1.m_Age << "  身高为：" << *p1.m_Height << endl;
    Person p2(p1);
    cout << "p2的年龄为：" << p2.m_Age << "  身高为：" << *p2.m_Height << endl;
}

int main() {
    test01();
    return 0;
}
```

> 总结：如果属性有在堆区开辟的，一定要自己写一个拷贝构造函数，防止浅拷贝带来的问题

#### 4.2.6 初始化列表

**作用：**C++提供了初始化列表语法，用来初始化属性

**语法：**构造函数():属性1(值1),属性2(值2)...{}

```C++
#include<iostream>

using namespace std;

//初始化列表
class Person {
public:
    //初始化列表（写死的）
    //Person() : m_A(10), m_B(20), m_C(30) {
    //
    //}
    //初始化列表（灵活的）
    Person(int a, int b, int c) : m_A(a), m_B(b), m_C(c) {

    }
    //传统初始化操作
    //Person(int a, int b, int c) {
    //    m_A = a;
    //    m_B = b;
    //    m_C = c;
    //}

    int m_A;
    int m_B;
    int m_C;


};

void test01() {
    Person p1(10, 20, 30);
    cout << "m_A=" << p1.m_A << "\nm_B=" << p1.m_B << "\nm_C=" << p1.m_C << endl;
}

int main() {
    test01();
    return 0;
}
```

#### 4.2.7 类对象作为类成员

C++中类中的成员可以是另一个类的对象，我们称该成员为**对象成员**

例如：

```C++
class A{}
class B{
    A a;
}
```

B类中有对象A作为成员，A为对象成员

**创建顺序：**

- 当创建对象时，先构造成员对象，在构造本身；析构时，先释放本身，在释放成员对象

```C++
#include<iostream>
#include<string>

using namespace std;

class Phone {
public:
    Phone(string brand) : m_brand(brand) {
        cout << "Phone 执行构造函数" << endl;
    }

    ~Phone() {
        cout << "Phone 执行析构函数" << endl;
    }

public:
    string m_brand;
};

class Person {
public:
    Person(string name, string pName) : m_Name(name), m_Phone(pName) {
        cout << "Person 执行构造函数" << endl;
    }

    ~Person() {
        cout << "Person 执行析构函数" << endl;
    }

public:
    string m_Name;
    Phone m_Phone;
};

void test01() {
    Person person("Eric", "Huawei");
    cout << person.m_Name << "的手机品牌是：" << person.m_Phone.m_brand << endl;
}

int main() {
    test01();
    return 0;
}
```

#### 4.2.8 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，成为静态成员。

静态成员分为：

- 静态成员变量
  - 所有对象共享同一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  - 所有对象共享同一个函数
  - 静态成员函数只能访问静态成员变量

静态成员的两种访问方式：

- 通过对象进行访问
- 通过类名进行访问

**示例：**静态成员变量：

```C++
#include<iostream>

using namespace std;

/**
 * 1、所有对象都共享同一份数据
 * 2、编译阶段就分配内存
 * 3、类内声明，类外初始化
 */
class Person {
public:
    static int m_Age;

private:
    static int m_B;
};

int Person::m_Age = 100;
int Person::m_B = 1111;

void test01() {
    Person p;
    cout << p.m_Age << endl;//100
    Person p2;
    p2.m_Age = 200;
    cout << p2.m_Age << endl;//200
    cout << p.m_Age << endl;//200
}

void test02() {
    //静态成员不属于某一个对象上，所有对象都共享同一份数据
    //因此静态对象有两种访问方式：
    //1、通过对象进行访问
    Person p;
    cout << p.m_Age << endl;
    p.m_Age = 200;
    //2、通过类名进行访问
    cout << Person::m_Age << endl;
    //静态成员变量的访问权限
    //cout << Person::m_B << endl;//m_B为private权限，类外不可访问

}
```

**示例：**静态成员函数：

```
#include<iostream>

using namespace std;

class Person {
public:
    static void func() {
        m_A = 199;//静态成员函数可以调用静态成员变量
        //m_B = 200;//静态成员函数不可以调用非静态成员变量，无法区分到底是哪个对象的m_B
        cout << "调用静态成员函数" << endl;
    }

private:
    static void func2() {
        cout << "static void func2() 的调用" << endl;
    }

public:
    static int m_A;
    int m_B;
};
//静态成员变量初始化
int Person::m_A = 100;

void test01() {
    //通过对象进行访问
    Person p;
    p.func();
    //通过类名进行访问
    Person::func();
    cout << Person::m_A << endl;
    //Person::func2();//类外访问不到私有的静态成员函数
}
```

### 4.3 C++对象模型和this指针

#### 4.3.1 成员变量和成员函数分开存储

在C++中，类内的成员变量和成员函数分开存储(在内存上的物理位置是分开的)

==只有**非静态的成员变量**才属于类的对象上==

```C++
#include<iostream>

using namespace std;

//成员变量和成员函数分开存储
class Person {

};

class Student1 {
    int age;//非静态成员变量   属于类对象上
};

class Student2 {
    int age;//非静态成员变量
    static int num;//静态成员变量 不属于类对象上
    void func() {}//非静态成员函数 不属于类对象
    static void func2() {}//静态成员函数 不属于类对象
};

void test01() {
    Person p;
    //空对象占用内存空间为1
    //C++编译器会给空对象分配一个字节内存空间，是为了区分空对象占内存的位置
    //每个空对象应该有一个独一无二的地质
    cout << "sizeof p = " << sizeof(p) << endl;
}

void test02() {
    Student1 s1;
    Student2 s2;
    cout << "sizeof s = " << sizeof(s1) << endl;
    cout << "sizeof s = " << sizeof(s2) << endl;
}
```

