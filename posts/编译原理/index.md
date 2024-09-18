# 编译原理


&lt;!--more--&gt;

## 第一章 引论

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200619122101408.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200619122101408.png)

### 什么是编译程序

- 翻译程序(Translator)：把某一种语言程序(称为源语言程序)等价地转换成另一种语言程序(称为目标语言程序)的程序

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200224230231285.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200224230231285.png)

- 编译程序(Compiler)：把某一种高级语言程序等价地转换成另一种低级语言程序(如汇编语言或机器语言程序)的程序

  ![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200224230240656.png)

  \* 运行编译程序的计算机：宿主机 * 运行目标程序的计算机：目标机 * 根据用途和侧重，编译程序可分为： 诊断编译程序(Diagnostic Compiler) 优化编译程序(Optimizing Compiler) 交叉编译程序(Cross Compiler)：编译程序产生不同于其宿主机的目标代码 可变目标编译程序(Retargetable Compiler：不需要重写编译程序中与机器无关的部分。

- 解释程序(Interpreter)：

  - 把源语言写的源程序作为输入，但不产生目标程序，而是边解释边执行源程序

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228113729557.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228113729557.png)

### 为什么要学习编译原理

- 理解计算系统，注意不是计算机系统。
- 设计计算系统
- 计算思维 Computational Thinking)
  - 计算思维是运用计算机科学的基础概念去求解问题、设计系统和理解人类的行为，它包括了一系列广泛的计算机科学的思维方法
  - 计算思维和阅读、写作和算术一样，是21世纪每个人的基本技能，而不仅仅属于计算机科学家
  - 包括
    抽象
    自动化
    问题分解
    递归
    权衡
    保护、冗余、容错、纠错和恢复
    利用启发式推理来寻求解答
    在不确定情况下的规划、学习和调度

### 编译过程

- 词法分析

  - 输入源程序字符串，扫描哪些字符构成了标识符，哪些字符构成了常数

  - 依循的原则：构词规则

  - 描述工具：有限自动机

  - ```
    for       i   :=     1     to     100   do
    基本字 标识符 赋值号 整常数 基本字 整常数 基本字
    ```

- 语法分析

  - 在词法分析的基础上，根据语法规则把单词符号串分解成各类 **语法单位(语法范畴)**，如下图得到了一棵语法树。
  - 依循的原则：语法规则
  - 描述工具：上下文无关文法

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228151145773.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228151145773.png)

- 中间代码产生
  - 对各类语法单位按语言的**语义**进行初步翻译
  - 依循的原则：语义规则
  - 描述工具：属性文法
  - 中间代码：三元式、四元式，树，…

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228151540768.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228151540768.png)

- 优化
  - 对前阶段产生的中间代码进行加工变换，以期在最后阶段产生更高效的目标代码
  - 依循的原则：程序的等价变换原则
  - 比如下图的憨憨程序就可以被编译器优化得很优秀

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228152021412.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228152021412.png)

- 目标代码产生
  - 把中间代码变换成特定机器上的目标代码
  - 依赖于硬件系统结构和机器指令的含义
  - 目标代码三种形式
    - 汇编指令代码: 需要进行汇编
    - 绝对指令代码: 可直接运行
    - 可重新定位指令代码: 需要链接

### 编译程序的结构

- 编译程序总框

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228152816930.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228152816930.png)

- 出错处理

  - 发现源程序中的错误，把有关错误信息报告给用户
  - 语法错误
    - 源程序中不符合语法（或词法）规则的错误
    - 非法字符、括号不匹配、缺少；、…
  - 语义错误
    - 源程序中不符合语义规则的错误
    - 说明错误、作用域错误、类型不一致、…

- 遍 (pass)

  - 所谓的 “遍”，就是对源程序或源程序的中间表示从头到尾扫描一遍

  - &gt; 编译程序的五个阶段，当然可以实现为五“遍”，也就是每个阶段都接受上一阶段的输出，然后完成本阶段的变换，生成本阶段完整的输出，每个阶段的输出都是源程序的一个完整表示，如一个完整的单词序列、一个完整的语法分析树、一个完整的中间代码表示、甚至是目标代码表示 等等

  - **阶段与遍是不同的概念**

    - 一遍可以由若干段组成

      &gt; 在实际工作中，从程序效率和软件设计的角度考虑，我们往往会把若干联系非常紧密的阶段，**合成一遍处理**，比如说我们通常把词法分析、语法分析、中间代码生成这三个阶段合成一遍处理。
      &gt;
      &gt; 把词法分析和中间代码产生实现为一些子程序，也就是词法分析子程序或者是语义子程序，这两类子程序由语法分析模块来驱动或者调用。
      &gt;
      &gt; 在这个过程中，语法分析起主导作用，在语法分析的驱动下，词法分析 语法分析和中间代码生成三个阶段穿插进行，当词法分析完成最后一个单词的识别的时候，整个分析树也就很快得到了，同时所有的语法单位的翻译也就完成了，三个阶段合成一遍完成

    - 一个阶段也可以分若干遍来完成

      &gt; 有些情况下一个阶段也可以分若干遍来实现，优化就是这样的例子，通常来说优化这个阶段被分成了很多遍，比如说 可以先对中间代码扫描一遍识别程序的基本结构，再扫描一遍完成简单的优化，再扫描一遍完成循环的优化，有时循环优化还有可能分成很多遍

- 编译前端和后端

  ![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228155322477.png)

  - 编译前端
    - 与源语言有关，如词法分析，语法分析，语义分析与中间代码产生，与机器无关的优化
  - 编译后端
    - 与目标机有关，与目标机有关的优化，目标代码产生
  - 带来的好处
    - 程序逻辑结构清晰
    - 优化更充分，有利于移植

### 编译程序的生成

工具：

- 以汇编语言和机器语言为工具

  - 优点: 可以针对具体的机器，充分发挥计算机的系统功能；生成的程序效率高
  - 缺点: 程序难读、难写、易出错、难维护、生产的效率低

- 以高级程序设计语言为工具

  - 程序易读、易理解、容易维护、生产的效率高

    ![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228175720075.png)

    上图表示已经有了 I 这种高级语言的实现和编译器，用 I 语言实现将 S 语言编译为 T 语言的编译器

高级语言书写：

- 利用有的某种语言的编译程序实现另一种语言的编译程序

  下图表示利用 P1 编译器，将 L1 语言写的 P2 编译器，编译成 A 代码写的 P2 编译器

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180026836.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180026836.png)

移植方法：

- 把一种机器上的编译程序移植到另一种机器上，跨机器

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180440787.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180440787.png)

自编译方式

- 你通过低级语言实现 L 语言的一部分 L1，然后拿 L1 和低级语言去实现 L1&#43;L2，然后拿 L1&#43;L2 去实现 L1&#43;L2&#43;L3，依次类推。

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180803482.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228180803482.png)

编译程序自动产生：

- 编译程序-编译程序，也叫编译程序产生器，也叫编译程序书写系统
- LEX：词法分析程序产生器
- YACC：语法分析程序产生器

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228181044912.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228181044912.png)

---

> Author: Kire  
> URL: http://localhost:1313/posts/%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86/  

