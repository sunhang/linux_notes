# 启动机箱
在intel的32位的cpu上，cs初始化为F000H，段基地址为FFFF0000H，IP为FFF0H。然后书上说，第一条指令的地址是：段基地址+IP = FFFFFFF0H。疑问点在于：此时系统还未进入保护模式，为什么要用到段基地址，为什么不是实模式的地址：CS x 10H + IP = FFFF0H。

看了帖子，总结：
在32位处理器上，段基地址是cs对应高速缓存寄存器，它的作用是，在保护模式下，cs存放fragment selector，它存放段的线性地址，从LDT或者GDT中载入进来的。

在cpu reset过程中，可能此时还未进入实模式，处于一种"unreal mode"，地址的计算方式要靠段基地址+IP的方式，进入实模式后（或者第一个jmp后），计算方式用CS x 10H + IP的方式，求出来20位的地址。

亦或是在reset时进入了实模式，但是“我們忽略了segment register cache register在real mode 下所起的作用”，reset时（during reset）进入了实模式我个人认为可能性比较小。

网上帖子参考：
<https://www.itdaan.com/tw/5f65afcceed0d676bb32e078ba11527c>
<https://news.ycombinator.com/item?id=18160029>