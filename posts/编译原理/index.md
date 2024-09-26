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

- 理解==计算系统==，注意不是计算机系统。
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

## 第二章 高级程序设计语言定义与语法描述

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200620000615982.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200620000615982.png)

### 程序设计语言的定义

语法：

- 一组规则，用它可以形成和产生一个合式 (well-formed) 的程序
- 词法规则：**单词符号**的形成规则
  - 单词符号是语言中具有独立意义的最基本结构
  - 一般包括：常数、标识符、基本字、算符、界符等
  - 描述工具：有限自动机
- 语法规则：**语法单位**的形成规则
  - 语法单位通常包括：表达式、语句、分程序、过程、函数、程序等;
  - 描述工具：上下文无关文法
- 例子：
  - E→iE→i：一个算术表达式可以由一个标识符构成；
  - E→E&#43;EE→E&#43;E：一个算术表达式可由两个算术表达式（也叫子表达式）通过 ‘&#43;’ 号连接构成；
  - $E\rightarrow E*E$：一个算术表达式可由两个算术表达式通过 ‘\*‘（星号，而不特指乘号）号连接构成；
  - E→(E)E→(E)：一个算术表达式外加括号，还是算术表达式；
- **语法规则**和**词法规则**定义了程序的形式结构
- 定义语法单位的意义属于**语义**问题

语义：

- 一组规则，用它可以定义一个程序的**意义**
- 描述方法
  - 自然语言描述
    - 二义性、隐藏错误和不完整性
  - 形式描述
    - 操作语义
    - 指称语义
    - 代数语义

程序，本质上是描述一定数据的处理过程；

程序语言的基本功能：描述数据和对数据的运算；

程序的层次结构：

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228184632420.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228184632420.png)

静态绑定：发生在程序编译过程中间的绑定，包括变量声明、类型定义、函数定义

动态绑定：发生在程序运行过程中间的绑定，包括 C&#43;&#43; 中的多态性虚函数

表达式：

- 表达式由运算量（也称操作数，即数据引用或函数调用）和算符（运算符，操作符）组成
- 形式：中缀、前缀、后缀

赋值语句

- A := B
- 名字的**左**值：该名字代表的存储单元的**地址**
- 名字的**右**值：该名字代表的**存贮单元的内容**
- C 语言中，`a&#43;5` 只有右值没有左值，因为该值只能用在赋值号右边

### 程序设计语言的描述

#### 上下文无关文法

文法：

- 描述语言的语法结构的形成规则
- 如 `He gave me a book.` 这句自然语言的文法规则为：

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228203556553.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228203556553.png)[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228203605324.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200228203605324.png)

相关概念：

- 字母表：一个有穷字符集，记为 ∑∑
- 字母表中每个元素称为字符
- ∑∑ 上的字（也叫字符串）是指由 ∑∑ 中的字符所构成的一个有穷序列
- 不包含任何字符的序列称为空字，记为 εε
- 用 ∑※∑※ 表示 ∑∑ 上的所有字的全体，包含空字 εε
  - 例如：设 ∑=a,b∑=a,b，则 ∑※=ε,a,b,aa,ab,ba,bb,aaa,⋯∑※=ε,a,b,aa,ab,ba,bb,aaa,⋯
- ∑※∑※ 的子集 UU 和 VV 的连接（积）定义为 UV=αβ | α∈U\andβ∈VUV=αβ | α∈U\andβ∈V
  - 例如：设 U=a,aa,V=b,bbU=a,aa,V=b,bb，则 UV=ab,abb,aab,aabbUV=ab,abb,aab,aabb，并且此时 UV≠VUUV≠VU
- VV 自身的 nn 次积记为 Vn=V⋯Vn个Vn=V⋯V⏟n个
- V0=εV0=ε
- V※V※ 是 VV 的闭包：V※=V0∪V1∪V2∪V3∪⋯V※=V0∪V1∪V2∪V3∪⋯
- V&#43;V&#43; 是 VV 的正规闭包： V&#43;=VV※V&#43;=VV※
  - 正规闭包与闭包的区别：如果 V 中原来没有空字，那么闭包有空字，正规闭包没空字
  - 例如：设 U=a,aaU=a,aa，那么 U※=ε,a,aa,aaa,aaaa,⋯,U&#43;=a,aa,aaa,aaaa,⋯U※=ε,a,aa,aaa,aaaa,⋯,U&#43;=a,aa,aaa,aaaa,⋯

上下文无关文法：

- 上下文无关文法 GG 是一个四元组 G=(VT,VN,S,P)G=(VT,VN,S,P)，其中

  - 

    VTVT

    ：

    终结符

     

    (Terminal) 集合 (非空)

    - 不能再分解的

  - 

    VNVN

    ：

    非终结符

     

    (Noterminal) 集合(非空)，且

    

    VT∩VN=∅VT∩VN=∅

    - 能再分解，比如上图中的 “主语”、”谓语”
    - 非终结符可由终结符和非终结符构成

  - 

    SS

    ：文法的

    开始符号

    ，

    

    S∈VNS∈VN

    - 是一个特殊的非终结符，它代表所定义的语言最终感兴趣的语法单位
    - 比如英语中的 “句子”，编程中的 “程序”

  - 

    PP

    ：

    产生式

    集合(有限)，每个产生式形式为

    - P→α，P∈VN，α∈(VT∪VN)※P→α，P∈VN，α∈(VT∪VN)※
    - 上式读成 “P 定义为 α”，即左边的终结符 P，是被定义的句法单位，右边的 α 是构成这个句法单位的一种组合。
    - (VT∪VN)(VT∪VN) 代表终结符和非终结符组成的字符集合，再打上 * 做闭包，代表该集合中的符号组成的字的全体

  - 开始符 SS 至少必须在某个产生式的左部出现一次

  - 例如，定义只含 &#43;, * 的算术表达式的文法：G=&lt;

     

    {i, &#43;, *, (, )}

    ,

     

    {E}

    ,

     

    E

    , P &gt;，其中，P 由下列产生式组成：

    - E→iE→i
    - E→E&#43;EE→E&#43;E
    - E→E∗EE→E∗E
    - E→(E)E→(E)

- 巴科斯范式：”-&gt;” 用 “::=” 来表示

- 上下文无关文法

  - 约定 {P→α1 P→α2cdots P→αn缩写为→P→α1|α2|⋯|αn{P→α1 P→α2cdots P→αn→缩写为P→α1|α2|⋯|αn
  - 其中 “|” 读成 “或”，称 α1α1 为 PP 的一个候选式
  - 表示一个文法时，通常只给出开始符号和产生式
  - 例如，定义只含 &#43;, * 的算术表达式的文法可以缩写为：
    G(E):E→i | E&#43;E | E∗E | (E)G(E):E→i | E&#43;E | E∗E | (E)

#### 文法与语言

推导：

- 定义：称 αAβαAβ 直接推出 αγβαγβ，即 αAβ⇒αγβαAβ⇒αγβ ，仅当 A→γA→γ 是一个产生式，且 α,β∈(VT∪Vn)※α,β∈(VT∪Vn)※
- 如果α1⇒α2⇒…⇒αnα1⇒α2⇒…⇒αn，则我们称这个序列是从 α1α1 到 αnαn 的一个推导。若存在一个从 α1α1 到αnαn 的推导，则称 α1α1 可以推导出 αnαn 。
- 对文法 G(E):E→i | E&#43;E | E∗E | (E)G(E):E→i | E&#43;E | E∗E | (E)，则 E⇒(E)⇒(E&#43;E)⇒(i&#43;E)⇒(i&#43;i)E⇒(E)⇒(E&#43;E)⇒(i&#43;E)⇒(i&#43;i)
- 称 α1∗⇒αnα1⇒∗αn 从 α1α1 经过 0 步或若干步推出 αnαn
- 称 α1&#43;⇒αnα1⇒&#43;αn 从 α1α1 经过 1 步或若干步推出 αnαn
- α1∗⇒αnα1⇒∗αn 即 α=βα=β 或 α1&#43;⇒αnα1⇒&#43;αn

句型：

- 假定 G 是一个文法， S 是它的开始符号，如果 S∗⇒αnS⇒∗αn，则称 αα 是一个句型
- S∗⇒SS⇒∗S，所以 S 也是句型

句子：

- 仅含终结符号的句型是一个句子。

语言：

- 文法 G 所产生的句子的全体是一个语言，记为 L(G)L(G)
  L(G)=α | S&#43;⇒α,α∈V※TL(G)=α | S⇒&#43;α,α∈VT※，即 L(G)L(G) 是由 αα 构成的集合， αα 属于终结符的闭包，且能由 S 推导得来。
- [![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104118219.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104118219.png)
- [![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104345152.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104345152.png)
- [![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104632829.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104632829.png)
- [![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104915177.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229104915177.png)
- [![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229105135900.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229105135900.png)

#### 语法树与二义性

最左推导和最右推导：

- 从一个句型到另一个句型的推导往往不唯一
  E&#43;E⇒i&#43;E⇒i&#43;i E&#43;E⇒E&#43;i⇒i&#43;iE&#43;E⇒i&#43;E⇒i&#43;i E&#43;E⇒E&#43;i⇒i&#43;i
- 最左推导：任何一步 α⇒βα⇒β 都是对 αα 中的最左非终结符进行替换
- 最右推导：任何一步 α⇒βα⇒β 都是对 αα 中的最右非终结符进行替换

语法树：

- 用一张图表示一个句型的推导，称为语法树
- 一棵语法树是不同推导过程的共性抽象
- 最左推导所对应的语法树的生长顺序是从上往下、从左往右

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229110330503.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229110330503.png)

二义性 ambiguity：

- 文法的二义性：如果一个文法存在某个句子对应两棵不同的语法树，则说这个文法是二义的
  G(E)： E → i|E&#43;E|E*E|(E) 是二义文法，对于 i∗i&#43;ii∗i&#43;i 可以画出两棵语法树

  ![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229111904441.png)

- 语言的二义性：一个语言是二义的，如果对它不存在无二义的文法

  - 对于语言 L，可能存在 G 和 G’，使得 L(G)=L(G’)=L，有可能其中一个文法为二义的，
    另一个为无二义的

- 自然语言的二义性举例 `John saw Mary in a boat`

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229112549846.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229112549846.png)

- 二义问题是不可判定问题，即不存在一个算法，它能在有限步骤内，确切地判定一个文法是否是二义的
- 可以找到一组无二义文法的充分条件

#### 形式语言鸟瞰

- 乔姆斯基于1956年建立形式语言体系，他把文法分成四种类型：0，1，2，3型
- 与上下文无关文法一样，它们都由四部分组成，
  但对产生式的限制有所不同

0 型（短语文法，图灵机）：

- 产生式形如：α→βα→β
- 其中：α∈(VT∪VN)※α∈(VT∪VN)※ 且至少含有一个非终结符；β∈(VT∪VN)※β∈(VT∪VN)※
- 乔姆斯基文法体系中，最通用，也是描述能力最强、最一般的文法，产生式的约束最弱

1 型（上下文有关文法，线性界限自动机）:

- 产生式形如：α→βα→β
- 其中：|α|≤|β||α|≤|β|，仅 S→εS→ε 例外
- 上下文有关文法非常复杂，等下会提到

2 型(上下文无关文法，非确定下推自动机)：

- 产生式形如： A→βA→β
- 其中：A∈VNA∈VN；β∈(VT∪VN)※β∈(VT∪VN)※

3 型 (正规文法，有限自动机)

- 产生式形如： A→αBA→αB 或 A→αA→α，终结符要么没有要么出现在右式最右边（右线性文法
- 其中： α∈V※Tα∈VT※；A，B∈V※NA，B∈VN※
- 产生式形如： A→BαA→Bα 或 A→αA→α （左线性文法
- 其中： α∈V※Tα∈VT※；A，B∈VNA，B∈VN

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229154446865.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229154446865.png)

四种类型文法的描述能力：

- L5=anbn | n≥1L5=anbn | n≥1 不能由正规文法产生，但可由上下文无关文法产生
  G5(S):S→aSb | abG5(S):S→aSb | ab
- L6=anbncn|n≥1L6=anbncn|n≥1不能由上下文无关文法产生，但可由上下文有关文法产生

[![img](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229155016138.png)](https://zhangt.top/CS/Compilation-Principles-Study-Notes/image-20200229155016138.png)

- 程序设计语言不是上下文无关语言，甚至不是上下文有关语言

- 

  L7=αcα|α∈a,b※L7=αcα|α∈a,b※

  不能由上下文无关文法产生，甚至连上下文有关文法也不能产生，只能由0型文法产生

  - 标识符引用。比如编程语言中要求使用的变量必须前面声明过
  - 过程调用过程中，“形-实参数的对应性”(如个数，顺序和类型一致性)

- 对于现今程序设计语言，在编译程序中，仍然采用上下文无关文法来描述其语言结构 （上下文无关文法的成熟高效）

---

> Author: Kire  
> URL: http://kiiiire.github.io/posts/%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86/  

