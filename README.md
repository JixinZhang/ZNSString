## 一、 NSString 

1. NSString被定义为类，是一个引用类型，
2. 初始化方法：字面量初始化，内存分配搭配初始化器，工厂方法;
3. NSString拥有恒定性，所有的操作无法更改字符串本身，如有更改都是返回新值的形式
4. NSString拥有共享机制，在运行时的时候将拥有相同字符串的string指向同一个指针（只有通过字面量初始化的string）

### 代码演示
####1.初始化

```
//----------------NSString初始化-------------
NSString *str1 = @"Hello World!";//字面量初始化
NSString *str2 = [[NSString alloc]initWithCString:"Hello World!" encoding:NSUTF8StringEncoding];//内存分配搭配初始化器
NSString *str3 = [NSString stringWithCString:"Hello World!" encoding:NSUTF8StringEncoding];//工厂方法
NSString *str4 = @"Hello World!";

```

####2.共享机制
1）代码

```
//---------共享机制------        
NSLog(@"str1 = %@",str1);
NSLog(@"str2 = %@",str2);
NSLog(@"str3 = %@",str3);
NSLog(@"str4 = %@",str4);

NSLog(@"str1 = %p",str1);
NSLog(@"str2 = %p",str2);
NSLog(@"str3 = %p",str3);
NSLog(@"str4 = %p",str4);
	
```
2）打印出四个字符串的内存地址如下

![image1.png](http://upload-images.jianshu.io/upload_images/2409226-4e23d37b3b9bd5ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**从上图中可以看出**
* 四个字符串拥有相同的内容，但是str1和str4的指针相同。运行时会将拥有相同字符串的string指向同一个指针（只有通过字面量初始化的string）

####3.恒定性
1）代码(接着上面的代码)

```
NSLog(@"----------------------------分割线----------------------------");
NSLog(@"str1.p = %p",str1);
[str1 stringByAppendingString:@" Jixin"];
NSLog(@"str1 = %@",str1);
NSLog(@"str1.p = %p",str1);
NSLog(@"");
str1 = [str1 stringByAppendingString:@" Jixin"];
NSLog(@"str1 = %@",str1);
NSLog(@"str1.p = %p",str1);
```
2) 打印出结果如下

![image2.png](http://upload-images.jianshu.io/upload_images/2409226-f528af0e6d2b893b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**从上图中可以看出**

* 在执行了`[str1 stringByAppendingString:@" Jixin"];`后str1的值和指针都没有改变；
* 但是在执行了`str1 = [str1 stringByAppendingString:@" Jixin"];`后str1的值和指针均改变了。
* 这说明对NSString的操作无法更改字符串本身，其实是返回的一个新的NSString。

## 二、NSMutableString

1. NSMutableString具有可变性，NSString具有恒定性；
2. NSMutableString为NSString的子类；
3. NSMutableString不具有共享机制，NSString具有共享机制；
4. NSMutableString并不是在原有内存上直接增长，而是重新分配一个更大或更小的缓存容量存放字符。

### 代码演示
####1.初始化

```
NSLog(@"----------------------------分割线----------------------------");
NSMutableString *mustr1 = [NSMutableString stringWithString: @"Hello,World!"];
NSLog(@"mustr1.p = %p",mustr1);

NSMutableString *mustr2 = [NSMutableString stringWithString: @"Hello,World!"];
NSLog(@"mustr2.p = %p",mustr2);

[mustr1 appendString:@" Very Good!"];
NSLog(@"mustr1 = %@",mustr1);
NSLog(@"mustr1.p = %p",mustr1);
```
####2.NSMutableString不具有共享机制，不具有恒定性
1) 打印出得结果如下

![image3.png](http://upload-images.jianshu.io/upload_images/2409226-1c68295879a9907f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**从上图中可以看出**

* 虽然mustr1和mustr2拥有相同的文字，但是指针却不相同；
* `[mustr1 appendString:@" Very Good!"];`可以直接修改NSMutableString的内容，不具有恒定性。

## 三、NSString，NSMutableString的copy和mutableCopy
> There are two kinds of object copying: shallow copies and deep copies. 

###1. NSString
1) 代码

```
NSLog(@"----------------------------分割线----------------------------");
NSString *string = @"Good to see you";
NSLog(@" string.p = %p",string);

NSString *strCopy = [string copy];
NSString *strMCopy = [string mutableCopy];
NSLog(@" strCopy.p = %p",strCopy);
NSLog(@"strMCopy.p = %p",strMCopy);
NSLog(@"");
NSLog(@" strCopy = %@",strCopy);
NSLog(@"strMCopy = %@",strMCopy);
```
2) 打印结果

![image4.png](http://upload-images.jianshu.io/upload_images/2409226-f82b98b2e24c89e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**从上图可看出**

* 对NSString执行copy的时候是指针赋值，属于浅拷贝(指针相同)；
* 对NSString执行mutableCopy的时候是内容复制，属于深拷贝（指针不同）。

###2. NSMutableString
1) 代码

```
NSLog(@"----------------------------分割线----------------------------");
NSMutableString *mStr = [NSMutableString stringWithString:@"Good day"];
NSLog(@"    mStr.p = %p",mStr);
NSString *strCopy = [mStr copy];
NSString *strMCopy = [mStr mutableCopy];
NSLog(@" strCopy.p = %p",strCopy);
NSLog(@"strMCopy.p = %p",strMCopy);
NSLog(@"");
NSMutableString *mStrCopy = [mStr copy];
NSMutableString *mStrMCopy = [mStr mutableCopy];
NSLog(@" mStrCopy.p = %p",mStrCopy);
NSLog(@"mStrMCopy.p = %p",mStrMCopy);

```
2) 打印结果

![image5.png](http://upload-images.jianshu.io/upload_images/2409226-14c0ce43d73a2591.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**从上图可看出**

* 对NSMutableString执行copy的时候是内容复制，属于深拷贝（指针不同）；
* 对NSMutableString执行mutableCopy的时候是内容复制，属于深拷贝（指针不同）。


### 3.copy和mutableCopy的结论如下

* 对NSString执行copy的时候是指针赋值，属于浅拷贝(指针相同)；
* 对NSString执行mutableCopy的时候是内容复制，属于深拷贝（指针不同）。
* 对NSMutableString执行copy的时候是内容复制，属于深拷贝（指针不同）；
* 对NSMutableString执行mutableCopy的时候是内容复制，属于深拷贝（指针不同）。