# 第一部分jni介绍

## 1.1什么是jni？

- jni全称java native interface（java本地开发接口）
- 相当于桥梁的作用，是一种协议
- 通过jni就可以让java调用C语言或者C++代码，并且可以让C调用java代码

## 1.2为什么要用jni？

- 通过jni技术，可以扩展Android手机的功能
- 弥补java语言功能的不足，利用jni调用C 、C++程序，可以执行大量的运算，视频的解码，Opengl的渲染等，这些操作都是java语言比较难处理的
- 代码复用强，如ffmpeg，onencv(人脸识别库)，7-zip
- 随着移动端技术的发展，越来越多的APP在开发中都需要底层复杂的处理，例如2016年最火热的直播技术，还有一些APP中的照片处理。。。学会这些技术刻不容缓

## 1.3怎么用jni？

- 学习C、C++语言，能看懂一定的C语言代码
- 利用ndk来（native develop kits）编写，并打包成.so文件供java代码调用

接下来就主要来重新复习一下大学学的C语言。

# 第二部分C语言

## 2.1开发工具

C语言的开发工具也比较多，大学的时候，写C语言代码经常使用**VC++ 6.0** 这个程序，这里使用 **Dev-C++** 这个程序来复习曾经学过的C语言，因为这款编辑器没有提示，所有的字母都需要自己手动打上去。

接下来以一个简单的C语言程序开启C语言历程。


```
#include<stdio.h>  //#include 相当于Java的import，stdio 是标准输入输出英文的缩写，.h 是头文件的后缀，包含一些函数 
#include<stdlib.h>  //标准的C语言函数库 
 main()  //相当于java的public static void main(String args[]){} 
{
 	printf("Hello world!\n");
 	system("calc");//打开电脑的计算器
	system("mspaint");//打开电脑的画板
	system("services.msc");//打开电脑的服务 
  	system("pause"); //让docs命令行执行pause命令，作用是控制台停留 
 
}
```

## 2.2C语言基本类型

> 回顾java的8大基本数据类型


byte | short| int| char| float| double| boolean| long
---|---|---|---|---|---|---|---|---
1个字节 | 2个字节| 4个字节| 2个字节| 4个字节| 8个字节| 1个字节|8个字节

至于boolean类型占几个字节，这个有不同的说法，这里就不过多的说明了。

> C语言中数据类型


char | int| float| double| long| short| signed| unsigned| void
---|---|---|---|---|---|---|---|---
1个字节 | 4个字节| 4个字节| 8个字节| 4个字节| 2个字节| 4个字节|4个字节|空

C语言中没有boolean、byte、String类型


```
#include<stdio.h>
#include<stdlib.h>
/**
计算类型的长度：sizeof("类型")返回int类型的长度
占位符：%d 
printf("内容");
*/
 main()
{
   printf("char类型的长度为：%d\n",sizeof(char)); 
   printf("int类型的长度为：%d\n",sizeof(int)); 
   printf("float类型的长度为：%d\n",sizeof(float)); 
   printf("double类型的长度为：%d\n",sizeof(double)); 
   printf("long类型的长度为：%d\n",sizeof(long));
   printf("short类型的长度为：%d\n",sizeof(short));    
   printf("signed类型的长度为：%d\n",sizeof(signed));   
   printf("signed类型的长度为：%d\n",sizeof(unsigned));   
   if(-1){
        printf("true\n");       
   }else{
         printf("flase\n");     
   }         
  system("pause"); 
}
```
> java基本数据类和C语言的一些区别：

1. Java中char类型的长度为2个字节,可以用来表示一个汉字，C语言中的长度为1个字节
1. Java中long类型的长度为8个字节，C语言中的长度为4个字节,C99标准规定：long类型的规定，不小于整形。
1. C语言中没有byte
1. C语言中 boolean类型，0表示flase,非零表示true 
1.  signed : 有符号：-128~127 = -2^7~ 2^7-1 
1.  unsigned：无符号 0~255 = 0~2^8-1 
1.  void: 无类型，代表任意类型 

> 结果：

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb7qkm7v7dj20f7079aag0.jpg)


## 2.3输入输出函数

###  2.3.1C语言中的各种占位符

int | long int| char| float| unsing|shor int|double|十六进制|八进制|字符串
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
整形 | 长整形|字符型|单精度浮点|无符号数|短整型 |双精度浮点|十六进制|八进制|字符串
%d | %ld|%c|%f|%u|%hd |%lf|%x|%o|%s


### 2.3.2输出函数

> 程序1：使用各种占位符

```
#include<stdio.h>
#include<stdlib.h>
main(){
       
       char c = 'A';
       int i = 12345678;
       long l = 123456789;
       float f = 3.1415;
       double d = 3.1415926535;
       
       printf("c==%c\n",c); //%c  - char
       printf("i==%d\n",i); //%d  -  int
       printf("l==%ld\n",l); //%ld - long int
       printf("f==%.4f\n",f); //保留4位小数 ，默认6位小数 
       printf("d==%.10lf\n",d); 
       printf("i==%hd\n",i); //%hd – 短整型
       //C语言的数组的括号不能写在左边 
       char cArray[]   = {'A','B'};
       printf("cArray内存地址==%#x\n",&cArray); 
       
       const char* text = "I love you!";//或者char text[] = "I love you!";
       printf("text内容==%s\n",text); 
       
       system("pause");
}
```
> 注意

1.  在C语言中，默认保留小数点后六位，要想保留对应的位数，就需要在百分号后边加上“**.数字**” 
 
2.  短整型hd占据2个字节，最大值32767 ,2的15次方-1 ，如果在将整形强制以短整型输出的时候可能会造成精度的丢失，例如上面程序：


```
十进制表示：12345678
 
二进制表示： 101111000110000101001110 
                     110000101001110
最后实际取值是110000101001110这个二进制所代表的十进制数据，高位都被省略了。
```

3.  不同的类型，要用不同的占位去输出，否则精度丢失。
4.  C语言的数组的括号不能写在左边，而在java中写在左右边都是可以的。
5.  C语言中是没有字符串类型的，可以使用指针或者字符数组代替
6.  十六进制%x输出的时候，可以输出地址，但使用%#x可以加上0x一起输出

> 结果

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb7spyz375j20eu06k3yu.jpg)

### 2.3.3输入函数

C语言中输入的时候主要用的就是 scanf函数，并使用&i取变量i的地址来完成输入，以供后面使用。

> 程序1：输入一个整形数字并输出

```
#include<stdio.h>
#include<stdlib.h>
main(){
       
       int i ;
       printf("请输入一个int类型数据\n"); 
       scanf("%d",&i);
       printf("您输入的数字是：%d\n",i); 
       
       //输入 
       char cArray[] = {'B','r','u','c','e'}; 
       int j;//注意j的位置，不能放在for循环内部 
       for( j=0;j<5;j++){
           printf("cArray[%d]==%c\n",j,cArray[j]);    
       }
       printf("cArray==%s\n",cArray); 
       system("pause");
}
 
```
> 结果

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb7t2tfkcfj20eq06uaa8.jpg)

> 程序2：输入一个字符串并输出


```
#include<stdio.h>
#include<stdlib.h>
main()
{
       //输入 
        char cArray[5]; 
        printf("请输入Hello：\n"); 
       //在C语言中没有String 类型，但是可以用char数组来表示 
       //scanf("%s",cArray); 可以加上&也可以不加，因为都表示数组的地址
       scanf("%s",&cArray);
       
       int j;
       for( j=0;j<5;j++){
           
           printf("cArray[%d]==%c\n",j,cArray[j]);    
                
       }
       printf("cArray==%s\n",cArray); 
       system("pause");
}

```
> 结果

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fb7t7wy1plj20eh06k74d.jpg)


> 程序3：


```
#include<stdio.h>
#include<stdlib.h>
main(){
       //输入 
        char cArray[] = {'a','b','c','d','e','\0','a','b'}; //\0代表结束 
         //数组是一块连续的内存空间 
        printf("cArray==%s\n",cArray); 
      	const  char* text = "I love you!!";
        printf("text==%s\n",text); 
         printf("地址==%#x\n",&text);//输出该指针的地址 
        system("pause");
}
```
字符数组中有一个\0，在输出这个数组的时候遇到\0代表结束，后面的字符将不会被输出

> 结果

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fb7u0dvgn0j20f6051t8p.jpg)


## 2.4什么是指针?(重点)

指针用于存放地址，指针就是内存地址，内存地址就是指针。

> 程序1：指针使用


```
#include<stdio.h>
#include<stdlib.h>
main(){
   
   //定义一个int类型的变量i,并且赋值为10； 
   int i = 10;
   //定义一个int类型的一级指针变量p 
   int* p; 
   //把i对应的地址赋值给p变量    
   p = &i; 
   
   //指针取值*p :把p变量对应的地址的值取出来 
   printf("*p===%d\n",*p); 
   *p = 100;//赋值 
   printf("*p===%d\n",*p); 
       
  system("pause");     
}     

```

> 结果

![image](http://wx2.sinaimg.cn/mw690/b0d9a523ly1fb7vexpc8jj20ci03qjra.jpg)

> 程序2：连连看小游戏作弊


```
#include<stdio.h>
#include<stdlib.h>

main(){
   
   printf("玩游戏倒计时开始了:\n"); 
   
   int i ;
   
   for( i = 10;i > 0 ;i--){
      
     _sleep(2000);
     printf("还剩下多少秒:%d\n",i);    
        
   }   
  printf("恭喜大哥，你真厉害！\n"); 
       
  system("pause");     
}     

```

当我们使用一些工具，可以获得i在输出时候的内存地址，通过修改其值，让i等于0；就可以迅速跳出for循环了，直接打印最后的输出语句，这就是利用指针来作弊。

## 2.5指针的深入理解(重点)


```
#include<stdio.h>
#include<stdlib.h>
main(){
   
  
  int i = 100;
  
  int* p = &i;
  
  //第一个实验：如果修改i值，p值有变化吗？
//  printf("修改i值前，p的值(地址)是：%#x\n",p); 
//   i = 200;
//  printf("修改i值后，p的值(地址)是：%#x\n",p); 
  
  
  //第二个实验： 如果修改p值，i值有变化吗？
  
//  printf("修改p值前，i的值是：%d\n",i); 
//   int j = 200;
//   p = &j;
//  printf("修改p值后，i的值是：%d\n",i); 
  
   //第三个实验：如果修改i值，*p值有变化吗？
//  printf("修改i值前，*p的值是：%d\n",*p); 
//  i = 200;
//  printf("修改i值后，*p的值是：%d\n",*p); 
  
   //第四个实验： 如果修改*p值，i值有变化吗？
  
   printf("修改*p值前，i的值是：%d\n",i); 
  
   *p =  200;
   printf("修改*p值后，i的值是：%d\n",i); 
       
  system("pause");     
}     

```
> 如果修改i值，p值有变化吗？输出结果：（p指向i的地址，i值发生变化，对p没有影响，因为i的地址没有变化）

![image](http://wx4.sinaimg.cn/mw690/b0d9a523ly1fb7w0v02cbj20ej03jdfs.jpg)

> 如果修改p值，i值有变化吗？输出结果：（修改p的值对i没有任何影响）

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fb7w1gkk6tj20ac03mdfs.jpg)


> 如果修改i值，*p值有变化吗？输出结果：（和第一个一样，一个取得是地址，一个取得是值，地址是不会变的，但值变化了，*p就变化了）

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fb7w1roeyoj20az03kq2w.jpg)

> 如果修改*p值，i值有变化吗？输出结果：（修改*p的值对i没有影响，所以不会发生变化）

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb7w256hwyj20f8040mx6.jpg)

> 指针和指针变量的关系

- 指针就是**地址**，地址就是指针，地址就是**内存单元**的编号
- 指针变量是存放地址的变量

指针和指针变量是两个不同的概念，**但是要注意**： 通常我们叙述时会把指针变量简称为指针，实际它们含义并不一样。
 
指针里存的是100,  此时指针指的是地址--具体
指针里存的是地址, 此时指针指的是指针变量 -- 可变

> 为什么要使用指针？？

-    直接访问硬件 (opengl 显卡绘图)
-    快速传递数据(指针表示地址)
-    返回一个以上的值(返回一个数组或者结构体的指针)
-    表示复杂的数据结构(结构体)
-    方便处理字符串 
-    指针有助于理解面向对象
 
> *号的三种含义

- 数学运算符:  3 * 5 
- 定义指针变量:  int*  p；
- 指针运算符(取值):  *p (取p的内容(地址)在内存中的值)


> 知识拓展 

内存分很多单元,每个单元对应一个编号，每个编号对用着一个地址。地址是从从0开始的非负整数。
例如：0000~FFFF。
  
XP操作系统 为什么只能显示3G内存？

常用的XP系统都是32位的系统，就是说在所有程序（包括）系统本身运行的时候，最多能使用2的32次方的地址，大家可以自己算一下2的32次方就是4G，但问题是，系统里面除了内存还有其它设备啊，显卡硬盘之类的都是需要地址的，所以，留给内存使用的地址只有3G多一点，剩下的要保留给其它设备。

## 2.6利用指针互换数字

> 程序1：


```
#include<stdio.h>
#include<stdlib.h>

/**
 互换两个数 

*/
void sitch(int a,int b){//传值无法改变值 
   int temp = a;
   a = b;
   b = temp; 
   printf("sitch 中a地址===%#x\n",&a); 
   printf("sitch 中b地址===%#x\n",&b); 
     
}

void sitch2(int* a,int* b){//传地址可以改变值 
   int temp = *a;
   *a = *b;
   *b = temp; 
   printf("sitch 中a地址===%#x\n",a); 
   printf("sitch 中b地址===%#x\n",b); 
     
}
main(){
   
   int a = 100;
   int b = 200;
   printf("main中a地址===%#x\n",&a); 
   printf("main中b地址===%#x\n",&b); 
   printf("a===%d\n",a); 
   printf("b===%d\n",b); 
   sitch2(&a,&b);
   printf("a===%d\n",a); 
   printf("b===%d\n",b);      
  system("pause");     
}     

```
因为指针指向的是变量的地址，所以可以通过地址改变变量的值。上面程序中通过sitch是不能改变Main方法中的a、b的值的，但是sitch2方法通过指针的操作确实可以的。

> 结果：

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb809oi4mmj20de05bt8i.jpg)

> 程序2：调用没有返回值的函数来得到“返回值”


```
#include<stdio.h>
#include<stdlib.h>

/**
 通过close利用指针方法返回多个值 
 close方法明明没有返回值，但用了指针就有了所谓的“返回值” 
*/
void colse(int* a,int* b){
    *a = 0;
    *b = 0; 
      
}
main(){
   
   //一键关闭GPS和wifi
   //1代表的是开，0代表是关闭 
   int a = 1;
   int b = 1; 
   colse(&a,&b);
   printf("a===%d\n",a); 
   printf("b===%d\n",b); 
  system("pause");     
}     

```

> 结果:a、b的值已经发生变化了

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fb80hvars9j208x034t8j.jpg)


> 小总结

通过被调函数修改主调函数普通变量的值

1. 实参必须是普通变量的地址
2. 形参必须是指针变量
3. 被调函数中通过修改 *形参名的方式修改主调函数相关变量的值

## 2.7多级指针

> 程序1：


```
#include<stdio.h>
#include<stdlib.h>

/**
 多级指针 
 指针指向的是内存地址
 地址就是指针 
*/

main(){
  
  //定义一个int类型的变量i,并且赋值为100； 
  int i = 100;  
  //定义一个int类型的一级指针变量address1,并且把i的地址赋值给它 
  int* address1 = &i;
  //定义一个int类型的二级指针变量 address2，并且把 address1对应的地址赋值给它 
  int** address2 = &address1;
  //定义三级指针 
  int*** address3 =  &address2;

  //多级指针取值 ***address3得到的值是100
  printf("***address3==%d\n",***address3); 
  //直接给三级指针赋值
  ***address3 = 2000;
  printf("***address3==%d\n",***address3); 
       
  system("pause");      
} 

```

> 结果

![image](http://wx2.sinaimg.cn/mw690/b0d9a523ly1fb810fkxquj20cl042jr8.jpg)

> 小总结
 
一级指针一个* ，二级指针两个* ，三级指针三个* ，   我们在取该指针对应的值的时候，  只需要加上相应个数的*就可以取到该指针最原始的值。


## 2.8指针和数组的关系

数组是一块连续的内存空间，数组的地址和数组的首元素的地址相同。

可以利用指针来获取数组的各个值。

> 程序1：数组的基本使用


```
#include<stdio.h>
#include<stdlib.h>
main(){
  
	int iArray[] = {1,2,3,4,5}  ;

	printf("取整形数组的值\n");
	printf("iArray[0]==%d\n",iArray[0]);  
	printf("iArray[1]==%d\n",iArray[1]); 
	printf("\n");

	printf("用指针的方式取内存地址\n");
	printf("iArray + 0==%#x\n",iArray+0);
	printf("iArray + 1==%#x\n",iArray+1);
	printf("iArray + 2==%#x\n",iArray+2);  
	printf("iArray + 3==%#x\n",iArray+3); 
	printf("\n");
	 	  

	printf("内存是一块连续的内存空间 \n");
	printf("iArray地址==%#x\n",&iArray);  
	printf("iArray[0]地址==%#x\n",&iArray[0]);  
	printf("iArray[1]地址==%#x\n",&iArray[1]); 
	printf("iArray[2]地址==%#x\n",&iArray[2]); 
	printf("iArray[3]地址==%#x\n",&iArray[3]); 
	printf("\n");
   
     
	printf("用指针取值\n");
	printf("iArray==%d\n",*iArray);  
	printf("iArray[0]==%d\n",*iArray+0);
	printf("iArray[1]==%d\n",*iArray+1);
	printf("iArray[2]==%d\n",*iArray+2);  
	printf("iArray[3]==%d\n",*iArray+3);  
	printf("\n");
     
	printf("用指针取值\n");
	printf("iArray[0]==%d\n",*(iArray+0));
	printf("iArray[1]==%d\n",*(iArray+1));
	printf("iArray[2]==%d\n",*(iArray+2));  
	printf("iArray[3]==%d\n",*(iArray+3));  
	system("pause");      
} 
```

> 结果，上面是用的是int类型的数组，在+1时候，每次地址增加4个字节，如果是字符类型数组，将会增加1个字节

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb8sj9v4ytj20f90hbjuv.jpg)


> 程序2：用户输入数组并打印输出数组的各个值


```
#include<stdio.h>
#include<stdlib.h>
main(){
       
       printf("请输入数组的长度：\n"); 
      //1.用户输入数组的长度
      int length;
      scanf("%d",&length);//输入的时候指向的是内存地址
      printf("您输入的数组长度为：%d\n",length);
     //2.根据用户输入的长度创建数组
     int iArray[length]; 
     //3.让用户输入数组的值
     int i;
     for(i=0;i<length;i++){
        printf("请输入iArray[%d]的值:\n",i);  
        //scanf("%d",&iArray[i]); 
         scanf("%d",iArray+i);                 
     }                     
     //4.把数组内容打印出来 
     for(i=0;i<length;i++){
        printf("iArray[%d]==%d\n",i,*(iArray+i));  
                       
    }  
    system("pause");   
}       

```

> 结果

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb8szcoe05g20g40cyjrz.gif)

> 程序3：指针的长度 


```
#include<stdio.h>
#include<stdlib.h>

/**
 指针的长度 是8个字节 
*/

main(){
       
     int* iPoint;
     float* fPoint;
     char* cPoint;
     
     printf("iPoint的长度==%d\n",sizeof(iPoint));  
     printf("fPoint的长度==%d\n",sizeof(fPoint));  
     printf("cPoint的长度==%d\n",sizeof(cPoint));  
    
    system("pause");   
}       

```

> 结果：这里我的开发工具编译环境是64位的，所以指针的长度是8个字节，如果用的是32位的，那么指针的长度就是4个字节。

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb8txxw0ccj20ek04gq3n.jpg)

## 2.9静态内存分配

> 程序1：

```
#include<stdio.h>
#include<stdlib.h>

/**
 静态内存分配 
*/

void func(int** address){
     //定义int类型的i变量，并且赋值100 
     int i = 100;
     // 把i对应的地址赋值给 iPoint变量 
     *address = &i; 
}
main(){
      
      //定义int类型的一级指针变量 iPoint
     int* iPoint; 
     
     func(&iPoint); 
     printf("*iPoint===%d\n",*iPoint);
     printf("*iPoint===%d\n",*iPoint);
     printf("*iPoint===%d\n",*iPoint);
     system("pause");   
}       

```
> 结果：程序运行结束后i被回收，所以再重复取*iPoint的值的时候都等于0.

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fb8zhpgabdj20aq049gm5.jpg)

> 说明

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fb8zg5gwe2j20tg0dkwhy.jpg)

## 2.10动态内存分配

> 程序1：


```
#include<stdio.h>
#include<stdlib.h>

/**
 动态内存分配 
*/

void func(int** address){
     
    int i = 100;
     
    int* temp;
   
   //malloc(int)-内存地址 
    temp = (int*)malloc(sizeof(int)); //需要强转成int类型指针 
    
    //把i对应的值，赋值给 temp地址对应的值 
    *temp = i;
    //把address 对应的地址对应的值（还是一个地址）修改成 temp
    *address = temp;
    
//    free(temp);   //释放内存  
}
main(){
    //定义int类型的一级指针变量 iPoint
    int* iPoint; 
     
    func(&iPoint); 
    printf("*iPoint===%d\n",*iPoint);
    printf("*iPoint===%d\n",*iPoint);
    printf("*iPoint===%d\n",*iPoint);
    printf("*iPoint===%d\n",*iPoint);
    system("pause");   
}       

```

> 结果就是iPoint的值全部都偶是100，但当执行free(temp)方法释放手动分配的内存空间的时候，输出结果将会是100，0，0，0。



> 知识拓展

动态内存和静态内存？？

- 静态内存是程序编译执行后系统自动分配，由系统自动释放, 静态内存是栈分配的.
- 动态内存是开发者手动分配的, 是堆分配的.  


## 2.11动态创建数组(重点)

> 程序1


```
#include<stdio.h>
#include<stdlib.h>

/**
 动态创建数组 
 输出函数 printf(); 
 输入函数：scanf("占位符"，内存地址); 
 realloc()重新分配内存 
*/


main(){
      
// 动态数组的创建?
printf("请输入您要创建数组的长度:\n");
//1、让用户输入一个长度
int length;
scanf("%d",&length);
printf("您输入数组的长度为:%d\n",length);
//2、根据长度，分配内存空间
int* iArray = malloc(length*4);
//3、让用户把数组中的元素依次的赋值；
int i;
for(i=0;i<length;i++){
   printf("请输入iArray[%d]的值：",i);   
   scanf("%d",iArray+i);                
}
//4、接收用户输入扩展数组长度
int suppLength;
printf("请输入您要扩展数组的长度:\n");
scanf("%d",&suppLength); 
printf("您要扩展数组的长度:%d\n",suppLength);
//5、根据扩展的长度重新分配空间?
//realloc
iArray = realloc(iArray,(length+suppLength)*4);
//6、把扩展长度的元素让用户赋值；
for(i=length;i<length+suppLength;i++){
   printf("请输入iArray[%d]的值：",i);   
   scanf("%d",iArray+i);                
}
//7、输出数组?
for(i=0;i<length+suppLength;i++){
   printf("iArray[%d]==%d\n",i,*(iArray+i));   
                
}
     system("pause");   
}       

```

> 结果

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fbd713bg7rj20fx0cuq65.jpg)

## 2.12其他重要的知识点

### 2.12.1函数的指针(重点)

> 程序1


```
#include<stdio.h>
#include<stdlib.h>

/**
 函数指针 
*/

//定义一个函数 
int add(int x,int y)
{
    return x + y;
} 

main(){
     //定义函数指针
     int (*android)(int x,int y); 
     //函数指针赋值
     android = add;
     //使用函数指针 
     int result = add(99,1);
     printf("result==%d\n",result);

     system("pause");   
}       

```

> 结果

输出结果result就是100，这里通过函数指针方法调用了add方法。

### 2.12.2Unition联合体

> 程序1

```
#include<stdio.h>
#include<stdlib.h>
//定义一个联合体，特点，所有的字段都是使用同一块内存空间； 
union Mix {
     long i; //4个字节 
     int k; //4个字节 
     char ii;//1个字节 
};
main() { 
     
       printf("mix:%d\n",sizeof(union Mix)); 
       //实验 
        union Mix m;
        m.k = 123;
        m.i = 100;
        printf("m.i=%d\n",m.i);         
        printf("m.k=%d\n",m.k);            
        system("pause");
} 

```


> 结果，通过sizeof函数可以看到占用4个字节，这是因为long类型和int类型占4个字节，取结构体中类型的最大字节数作为联合体所占字节数，因为占用的是同一块内存空间，所以m.k所附的值被m.i所附的值覆盖掉，最后的输出m.i和m.k都是100。

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fbd7awfzb3j20ao03qmxj.jpg)

### 2.12.3枚举

> 程序


```
#include<stdio.h>
#include<stdlib.h>

/**
枚举的值递增 
默认首元素的值是0 
*/
enum WeekDay {
     Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday
};
 
main() {
       enum WeekDay day = Wednesday;
       printf("%d\n",day);
       system("pause");
}

```

> 结果

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fbd7k9xgj6j20ct03174m.jpg)

### 2.12.4Typedef别名(重点)

> 程序


```
#include <stdio.h> 
#include <stdlib.h>
typedef int i;
typedef long l;
typedef float f;
main() {
       i m = 10;
       l n = 123123123;
       f s = 3.1415;
       printf("%d\n", m);
       printf("%ld\n", n);
       printf("%f\n", s);
       system("pause");       
}
```

> 结果，输出程序上面m、n、s的值，这个别名挺好的，可以java中没有这方法。

### 2.12.5结构体

> 程序


```
#include<stdio.h>
#include<stdlib.h>
//定义结构
struct student{
    int age;//4个字节 
    float score;//4个字节 
    char sex;   //1个字节 
} ;     
main(){
       //使用结构体 
       struct student stu = {18,98.9,'W'};
      //结构体取值
	  printf("stu.age == %d\n",stu.age); 
      printf("stu.score == %.1f\n",stu.score); 
      printf("stu.sex == %c\n",stu.sex); 
      //结构体赋值
	 
	  stu.age=333;
	  stu.score=123.445;
	  stu.sex = 'M';
	  printf("重新赋值\n");
	  printf("stu.age == %d\n",stu.age); 
      printf("stu.score == %.1f\n",stu.score); 
      printf("stu.sex == %c\n",stu.sex); 
	  
	  //结构体的长度
	  printf("struct student结构体的长度是%d\n",sizeof(struct student));	
      
      system("pause"); 
}

```

> 结果,结构体的长度是12字节，如果float score换成double score那么就是24字节

![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fbd8bkamn2j20br06gmyb.jpg)

### 2.12.6结构体指针(重点)

> 程序


```
#include<stdio.h>
#include<stdlib.h>

//结构体指针 

//定义结构
struct student{
    int age;//4个字节 
    float score;//4个字节 
    char sex;   //1个字节 
} ;     
main(){
       //使用结构图
       struct student stu = {18,98.9,'W'};
       
       //结构体指针指向stu的地址 
       struct student* point = &stu;
       //二级结构体指针 
       struct student** point2 = &point;
       
       //取值运算(*point).age  等价于 point->age ，一个箭头相当于一颗星 
       printf("(*point).age ==%d\n",(*point).age ); 
       printf("point->age ==%d\n",point->age ); 
       printf("point->score ==%f\n",point->score ); 
       printf("point->sex ==%c\n",point->sex ); 
       printf("\n");
       //赋值运算
       point->age = 20; 
       point->score = 100;
       point->sex = 'M';
       printf("point->age ==%d\n",point->age ); 
       printf("point->age ==%d\n",point->age ); 
       printf("point->score ==%f\n",point->score ); 
       printf("point->sex ==%c\n",point->sex );  
        printf("\n");
       //二级结构体指针取值 (*point).age  等价于 point->age   所以  (**point).age 等价于 (*point)->age,，一个箭头相当于一颗星
        printf("(**point2).age ==%d\n",(**point2).age ); 
        printf("(*point2)->age ==%d\n",(*point2)->age ); 
         printf("\n");
        //二级指针赋值
        (**point2).age = 2000;
        printf("(**point2).age ==%d\n",(**point2).age ); 
        printf("(*point2)->age ==%d\n",(*point2)->age ); 
       system("pause"); 
}

```

> 结果,一个箭头相当于一颗星,一颗星是一级结构体，两颗星是二级结构体。

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fbd8s6f029j20fk0ac0ur.jpg)



# 总结


C语言的部分也就这么多，所有的东西大学学习期间都详细的学习过，当时还认真的的做过相关的图书馆what系统，但后来也就没怎么用，忘得也就差不多了，今天重新复习一下当初的C语言，为下一步Android中的jni做个基础。

语言这东西时间长不用就忘记的差不多了。。。

> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人微信公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")