

#  Lecture 1: 基础

Java源于Sun Microsystems公司的James Gosling博士于1991年发明的一个名叫Oak的编程语言，当时主要想用在消费电子产品（比如机顶盒之类）上，但是没成气候。到了1995年，互联网在美国开始风起云涌，Sun是“网络就是计算机”这一著名理念的提出者，它敏锐地捕捉到这个机会，就将Oak改名叫Java，并在SunWorld大会上发布了Java 1.0，让Java程序（Java Applet）能运行在浏览器中，口号是：Write Once, Run Anywhere。这个事件标志着Java正式诞生，20多年过去了，Java和JVM演变成一个庞大的软件开发平台和生态系统，在企业应用和互联网领域大放异彩。

我们知道，计算机有不同的硬件结构，不同的操作系统，应用程序无法不加任何修改就运行在不同的软硬件平台上，肯定会有一些差异，这叫作“不兼容”。软件的兼容性分为两个层面：

- 源代码兼容

  如果您在编程的时候，只用了语言本身提供的机制以及标准库，那么编写出来的源代码，与更下一层的操作系统无关。这个源代码是可以被移植到其它系统上而不用修改的（或者很少量的修改），编译器知道怎么产生相应平台的目标代码。

- 二进制兼容

  如果源代码被编译成二进制之后的可执行文件（或者库），也能在不同平台上运行，这就是二进制层面的兼容了。这通常只能在同一种操作系统上才能做到，比如一个软件可以在Windows 8上运行，也能在Windows 10上运行；或者一个.so的动态链接库可以适用于多种Linux发行版。

当然，软件开发层面，还有API级别的兼容。这个跟我们要讲的东西，暂时无关，以后再细说。

所以您看，二进制兼容基本上做不到，源代码兼容也很麻烦（尤其是代码里用到了平台特定的API）。那么“Write Once, Run Anywhere”对开发者而言，就成了一个非常吸引人的特性。要实现这个目标，就得引入一个新的概念：虚拟机（Virtual Machine, VM。Java虚拟机被叫作JVM）。虚拟机的作用是，屏蔽不同的硬件架构和操作系统的差异，对软件开发者提供一个一致的开发模型和运行时环境。总而言之，Java就是一个基于虚拟机的编程语言，Java源代码被编译成一种叫做字节码（Byte Code）的中间结果，运行时再由虚拟机翻译成不同的目标平台的机器码执行。我们先建立这么一个初步的认识，以便能够继续学习后续内容，最后一讲将专门来讨论JVM。

几个常被提及的概念，和Java纷繁复杂的版本规则，也有必要专门解释一下：

- **Java** 狭义上讲，它是一种程序设计语言；广义上讲，它现在代表了以Java语言和JVM虚拟机为基础的一个软件开发平台。
- **JVM** Java虚拟机。Java的核心，早期只能支持Java语言，现在已经可以支持很多种语言比如Scala, Ruby， Python等。
- **JDK** Java Development Kit。Java的开发、运行环境，包含了编译器、运行时、标准库等基础设施。
- **JRE** Java Runtime Environment。Java运行时环境，去掉了一些开发工具和组件，体积小于JDK，通常用于软件发布。Java 11开始，不再提供单的JRE，直接安装JDK即可。
- **JCP** Java Community Process, 一个负责维护Java规范和标准化的国际组织，IBM, ORACLE, GOOGLE这些大企业都是其成员。
- **JSR** Java Specification Requests, Java规范提案。 如果要增加某一个新功能，就需要先提一个JSR，然后经过JCP的审议批准，体现在未来某个Java版本中。

历史上，根据不同的功能/用途（或市场定位），Java被划分为三个版本：J2ME, J2SE和J2EE。J2表示Java 2 Platform。后来为了应对快速的版本变化而保持命名统一，就改称为Java ME（Mobile Edition）, Java SE（Standard Edition）, Java EE（Enterprise Edition）。SE包含了Java的所有核心功能，而EE只是一些JSR和接口，一般需要由厂商提供实现。比如容器厂商实现Servlet，数据库厂商实现分布式事务（XA）等。

JavaSE采用数字来命名版本。早期用1.x表示，比如1.5（就是大名鼎鼎的Java 2），1.8（也被称作Java 8）。1.8之后就直接改为Java 9, Java 10......这种方式了，目前最新版本是18。除了一些顽固分子以及历史遗留的代码/系统还在使用Java 8，Java 11已经成为主流了，这是一个Long-Term-Support(LTS)版本，稳定可靠，可以在生产环境长期使用。前些时候，Java之父James Gosling博士推荐开发者尽快升级到Java 17（继11之后的另一个LTS版本），以便享受现代Java带来的性能、安全和开发效率的提升。

## 1.1 类型系统

计算机的程序设计语言(Programming Language, PL)跟我们人类交流、写作用的自然语言类似，只不过交流的双方，一方是软件开发者，另一方是计算机。机器是一个很死板的、不会变通的东西，只会作确定的事情（这个讲义里不讨论AI），所以程序设计语言要比自然语言更为抽象和严谨，比如机器就不懂得去猜测双关语的意思。类型系统就是一个抽象的模型，用来描述现实世界里纷繁复杂的事物。

程序设计语言的类型，大致有两种思路：静态类型（强类型）和动态类型（弱类型）。所谓静态类型就是任何变量，在编译期都必须明确地、显示地指定一个类型，在指定之后就不可更改了。C，C++, C#, Java包括现在流行的Rust都是静态类型的语言：

```java/rust
int i = 1；
float f = 2.0f;
let age : u8 = 32;
```

不同的语言，定义的语法有差别，但核心是一样的，都必须指定类型（ int, float, u8 这样的东西）。与之相反的是，弱类型的动态语言，不需要显示指定变量的数据类型，在运行时，虚拟机会去推断变量到底应该是个什么类型，最为典型的就是JavaScript和Python：

```javascript/python
let age = 32
var name = "John Denver";
address = '北京市海淀区上地七街'
```

弱类型的语言通常被称作动态语言或脚本语言，它的运行方式是由虚拟机逐行解释执行的，不需要由程序员手工编译成平台目标代码或者字节码(Byte Code)或者翻译为中间语言(IL)。它的优点是，编程相对自由一些，同一个变量，上面存放的是整数，隔几行代码，又用来存放字符串了。当然随之带来的麻烦就是更容易出错，尤其是在代码量较大的项目里。

我们也不一定要在静态或者动态类型的语言里分个优劣。风水是轮流转的，早些年的语言都是静态类型的，大家就觉得动态类型推断很酷，所以就涌现了很多动态类型的语言，甚至静态语言也从语法层面可以模拟一下：

```java/c#/rust/C++
var i = 32;    //Java, C#
let j = 1;	   //Rust
auto n = 123;  //C++
```

这看起来跟JavaScript颇为相像了，但它只是一个语法糖（Syntactic Sugar），类型推断是在编译时进行的，生成的目标代码中，变量i, j, n都具有明确的类型了，这和运行时类型推断有本质区别。

当弱类型语言盛极一时的时候，大家又开始反思它的缺点，尤其是带来的软件工程方面的问题，又设计出了像Rust这样的静态语言，用极度严格的类型检查来避免不必要的错误，降低软件开发和维护的成本，提升安全性。

但是，不管是静态还是动态类型的语言，或者说，不论是否要求程序员显示地声明变量类型，一个变量在某个时刻它都应该属于某个类型才对。我们把目光聚焦到Java的类型系统上来，考察它如何用有限的类型描述无穷的世界。

### 1.1.1 基本类型

在Java中，类型被分为基本类型（Primitive Types）和引用类型（Reference Types）。基本类型有时候也称之为内置类型（Built-in Types），包括了数值类型、布尔类型和字符（通常也被认为是数值的一种）几种，参见下表：

| #    | 类型名  | 中文名       | 说明                                                         |
| ---- | ------- | ------------ | ------------------------------------------------------------ |
| 1    | byte    | 字节         | 8 bits，表示-128 - 127之间的整数                             |
| 2    | short   | 短整型       | 2 bytes，表示 -32768 - 32767之间的整数                       |
| 3    | int     | 整型         | 4 bytes，表示的范围在正负20多亿之间                          |
| 4    | long    | 长整型       | 8 bytes，绝大多数情况下，它的表示范围都够用了                |
| 5    | float   | 单精度浮点数 | 4 bytes，遵循IEEE-754标准                                    |
| 6    | double  | 双精度浮点数 | 8 bytes，IEEE-754标准,范围和精度都比float大                  |
| 7    | boolean | 布尔型       | true or false，表达一种二值状态，黑/白，是/否，有/无，开/关，死/活... |
| 8    | char    | 字符         | 可以表示UNICODE字符集中从'\u0000' - ''\uffff'之间的字符，范围相当于 unsigned short |

我们发现，基本类型都跟数值有关系，这有两种可能：

1. 这个世界上的纷繁复杂的关系，多数都可以抽象成数量关系来描述；
2. 计算机擅长作数字运算，因此就想办法把别的关系转化为数量关系来表达。

不管以上哪一种可能成立，都不影响一个事实：所有的程序设计语言，基本类型都差不多是这个样子。

### 1.1.2 引用类型

为了增强表达能力，在基本类型之外扩展出了一些复合的类型，Java里面，称之为引用类型。理解引用（Reference）的概念，需要一些Java内存模型的知识，我们先略过。简而言之，一个变量如果是引用类型，这个变量存储的并不是这个数据本身，而是它在另外一块内存上的地址。Java中，引用类型有四种：

| #    | 类型      | 中文名   | 说明                                                         |
| ---- | --------- | -------- | ------------------------------------------------------------ |
| 1    | class     | 类       | 类是一种复合数据类型，是实现OOP的核心机制，将一组数据和行为封装在一起，作为整体 |
| 2    | interface | 接口     | 类的一种特殊形式，是一种抽象手段。它只包含行为声明(和缺省实现)，以及静态的数据 |
| 3    | type      | 类型变量 | 常用于泛型。比如：<T extends User>，这个T就是类型变量        |
| 4    | array     | 数组     | 多个元素组成的集合，通过下标来访问集合中的元素。元素可以是基本类型、也可以是引用类型 |

class和interface的内容较多，我们将专门用一小节来讲述。type variables留在泛型（Generics）部分展开讲。

在Java中，有的数据结构只接受引用类型，比如List, Map等。因此，语言提供了一种内置的机制，将基本类型转换为一个引用类型（这个过程叫做装箱，Boxing）或者将引用类型退化成一个基本类型（这个过程叫拆箱，Unboxing）。装箱和拆箱的过程是全自动的，只是有两个问题需要注意：

1. 大量的、频繁的装箱拆箱会带来一定的性能开销，应尽量避免；

2. 装箱以后，就可能出现null pointer的问题，使用时需要注意。比如下面的代码：

   ```java
   public int add(Integer arg0, Integer arg1){
   	Integer result = arg0 + arg1; //此处不考虑溢范围出问题
   	return result; //此处自动拆箱为基本类型int
   }
   
   var result = add(1, 2); //参数1，2将被装箱成两个Integer类型的对象传入，add方法工作正常
   var result = add(null, null); //传入两个null，作为参数，也是合法的。但是在add函数内会引发异常
   ```

   基本类型和它的引用类型之间的对应关系如下表：

| #    | 基本类型 | 引用类型（class） |
| ---- | -------- | ----------------- |
| 1    | byte     | Byte              |
| 2    | short    | Short             |
| 3    | int      | Integer           |
| 4    | long     | Long              |
| 5    | float    | Float             |
| 6    | double   | Double            |
| 7    | boolean  | Boolean           |

数组（Array）是一种集合（Collection）类型，它是一个容器，能盛放指定数量的元素。所有的元素的数据类型必须相同，可以是基本类型，也可以是应用类型。从数学角度看，我们可以将数组理解为一个向量（Vector），向量的维度（Dimension）必须大于或等于 1，即可以是一维的，也可以是高维的。二维的数组，就是一个矩阵。在使用的时候，通过下标（或者叫索引，Index）来访问。在绝大多数程序设计语言里，下标约定俗成从0开始，即array[0]表示第一个元素（也有例外：Fortran语言里，默认从1开始）。如果您发现某人在生活中计数喜欢从0开始，他可能是一个程序员。

```java
int[] numbers = new int[4]; //定义一个长度为4的、元素类型为int的数组
for(int i = 0; i < numbers.length; i++){
	numbers[i] = i;	//循环为数组每一个元素赋值
}

//这是另一种定义并初始化数组的方法，不用指定长度，但给出集合中的每个元素
int[] numbers = new int[]{1, 2, 3, 4};

int length = numbers.length; //数组是个引用类型，它除了包含元素之外，还有一个属性length，表示元素的个数。
```

Java是一种面向对象（Object Oriented，OO）语言，要求我们使用面向对象编程（Object Oriented Programming）范式，并且只支持OOP范式。它的OO特性，在引用类型的层次设计上，体现得淋漓尽致。所有引用类型，都有一个共同的祖先，即：java.lang.Object。其它的都是派生自这个class，然后构成了一棵复杂的类型树，有兴趣的读者可以看看Java标准库的层次结构。OOP是一个非常重要的话题，我们将专门用一讲来讨论。

### 1.1.3 缺省值和初始化

当我们定义了一个某种数据类型的变量，还没有给它赋值的时候，它会自动获得一个缺省值。Java的缺省值规则很简单，遵循一下几条：

1. 基本类型中，整数（ byte, short, int, long）类型的缺省值是0。当然这个0会被解释为对应类型的0，也就是占用的内存长度不一样；
2. 基本类型中，浮点数（float, double）类型的缺省值为0.0。占用4个字节或8个字节的内存空间；
3. boolean型，缺省值是false；
4. 引用类型，缺省值都是null。null是一个特殊的值，表示它不指向任何的内存区域。null和blank, empty并不是一回事，需要注意。null这个东西值得多说几句，全世界的软件，可能半数以上的bug，都是因它而引发的。C/C++语言的空指针，Java里的NullPointerException，都是让人头疼，编程时需要仔细。为了最大可能地杜绝这个问题，IDE、语言机制、编译器，都做了很多工作。现在的IDE比人还聪明，比如Eclipse，对于未初始化的引用类型变量会给出提示；对可能出现null的代码也会提示。Java 8开始，也提供了一种叫做Optional的机制，帮助程序员简化null指针判断，避免null pointer exception。

理论上，定义一个任何类型的变量，在使用之前，都需要被初始化。这是一个良好的编程习惯，因为编译器给出的缺省值未必符合您的心意。基本类型和引用类型的初始化方法略微不同：

- 初始化基本类型

  ```java
  //直接将值付给变量即可
  int a = 1;
  float f = 1.0;
  char ch = 'a';
  
  //编译期自动类型推断
  var d = 1d;
  var result = false;
  ```

- 初始化引用类型

  ```java
  Object obj = new Object();
  User user = new User();
  int[] array = new int[s4];
  Map<Integer, String> map = new HashMap<>();
  ```

  从上面的代码可见，引用类型的初始化，必须用new关键字，它会为对象在堆上分配内存空间。Java虚拟机有一个垃圾回收器(Garbige Collector, GC)，当对象不再被使用的时候，内存空间被自动释放。在C语言中，则需要手动调用free函数、C++需要使用delete语句，释放空间。内存泄漏(Memory Leak)也是一种令人困扰万分而且非常难以定位的BUG，跟null pointer一样臭名昭著。近年来流行的Rust语言，采用了一些近似严酷的方法来解决这个问题：既不需要垃圾回收器也不需要手工管理内存还绝对安全。

  有一个使用特别频繁的引用类型，叫做String，它代表了内存中的一个以Unicode （UTF-16）编码的字符序列，就是我们常常说的“字符串”。String的初始化，看起来有点不一样：

  ```java
  //初始化String时，可以不用new关键字，直接把双引号包裹起来的字符串赋给变量即可
  String name = "John Denver";
  var mobile = "13989898989";
  
  //当然，您也可以用new。就是有点画蛇添足的感觉
  String university = new String("MIT");
  ```

  String还有一些特别之处，以及围绕它，一些有意思的操作，我们在标准库那一讲再详细讨论。类型系统得讨论就到此为止，下面我们熟悉一下Java的基本语法，这样才能开始编程之旅。

## 1.2 语法基础

在自然语言中，语法（Syntax）是用来规范语言形式的，程序设计语言也是如此。我们写一篇文章，最基本的要求是不要有语法错误，编写一段程序亦然，也不能有语法错误，否则编译器不会同意。Java是一种类C（C-Liked）语言，学过C/C++的读者5分钟就能上手了，因为它们有一些共同的语法特征，比如语句都以分号（；）结尾，代码块用花括号（{}）包裹起来等等。程序设计语言的世界里，有很多种语法风格，比如Python就跟Java截然不同，也没必要去争出个优劣高低来，习惯了就好。

下面就从几个方面来介绍Java的语法，如果已经有Java编程经验的读者，可以跳过。

### 1.2.1 程序基本结构

Java是一种不是很彻底的面向对象的程序设计语言（基本类型不是对象），但是在宏观结构上，它又是完全面向对象的。也就是说，任何变量（属性）和函数（方法），不能像C/C++/JavaScript一样独立存在于源代码文件之中成为一等公民，它必须被封闭在以下几种结构之一：

1. 类（class）
2. 接口（interface）
3. 枚举（enum）
4. 注解（annotation）
5. 记录（record）

以上五种程序组织结构（都是引用类型，基本上，我们可以认为enum, annotation和record是一种特殊的class。因此，下文中用class泛指这五种类型），如果它的可见性是public的话，就必须存在于一个独立的物理文件中（*.java）。换言之，Java的代码组织结构是以文件为基本单位，每个文件代表了一个public的引用类型。我们不能把两个public class写在同一个.java文件里：

```java
//MyClass.java

public interface File{
	public int open();
	public byte[] read(long position, long length);
}

public class BinaryFile implements File{
	private int fid;
	private String name;
	
	public int open(){
		//Opens the file and returns the file descriptor
	}
	
	public byte[] read(long position, long length){
		//Read the given length from from the specified position in the file
	}
}
```

上面的代码是无法通过编译的，因为MyClass.java这个物理文件里，有两个public的引用类型的定义。这一点跟其它语言有所区别。C语言里，可以把多个struct定义在一个源文件中，C++也可以将多个class定义在一个源文件里，但是Java不允许。

大多数时候，为了实现某个功能，需要定义（Define）或声明（Declare）一系列的引用类型，它们之间或者构成层次结构，或者相互配合。这些引用类型将会被集中放在一起，构成一个包（Package）。包是一个同时具有逻辑性质和物理性质的概念：

- 逻辑层面，它将一组功能、语义、或者行为相似的引用类型组织在一起，在其它代码中可以通过import关键字引入使用；
- 物理层面，一个包就是一个磁盘目录（Directory），必须建一个真实存在的物理目录，JVM的Class Loader才能找到并加载它。

包既然是一个目录，那么它就具有目录树一样的嵌套结构，上下层级之间通过点（.）来表示。最前面我们讲Java诞生的背景时提到过，Java的出现，与互联网大潮紧密相关。互联网上组织资源的方式是URL，那么Java中包的层次结构，约定成俗的方法，也建议采用URL来命名，只不过把顺序倒过来，比如，程序员张三所在的公司的官网的URL是：gouge.com，他们公司开发了一个名叫BetaGo的项目，包名结构应该像以下样子：

```java
package com.gouge.betago;				//项目名就是顶层的报名
package com.gouge.betago.kernel;		 //betago的内核代码
package com.gouge.betago.service;		 //betago的对外的服务接口
package com.gouge.betago.service.restful; //Restful形式的接口
```

并不是只有最底层的包内才有.java文件。源文件可以存在于任何一层的包中，这要取决于您的设计逻辑。在Java的命名规则里，package name全部采用小写字母（Lower Case Letters），下划线和数字也是允许的。命名时以能够表达准确的语义和逻辑为准，同时兼顾一下美观。

我们再强调一下，Java源代码组织的一些最佳实践（Best practice）:

1. 把同一个功能的代码，放在同一个源文件里。尽量不要把所有的代码都搞在一个源文件中，变成裹脚布一样的风格；

2. 把同一个功能模块、或者逻辑上相似的源文件组织在同一个包中，比如java.net.http，所有的源文件，都是为了实现HTTP Client服务；

3. 一个Java项目，通常的目录结构如下：

   ```
   -- Project Name						 //项目名，最顶层的目录
      -- Source Files Folder（src）		 //源代码目录，所有的源代码都放在这下面
         -- Package					 //定义包的层级目录结构
            -- *.java			 		 //包下面放置源代码文件
      -- Binary Folder（bin）			//编译输出的结果
      -- Configuration Folder		 	  //存放配置文件的目录
   ```

   如果您使用Eclipse, IDEA,之类的IDE，在创建项目时，它会创建一个与上面类似的目录结构（不同项目类型有所不同，比如web项目，就会有一个WEB-INF的目录，以满足Servlet规范要求）；如果用VS CODE、VIM之类的工具时，就需要手工创建项目结构。

请注意：如果我们不声明任何package，会怎么样呢？编译器会默认您有一个package，就位于当前的源代码目录下。聪明的编译器知道按这些规则，去磁盘上搜寻源文件并编译成字节码。运行时，JVM的类加载器（Class Loader）也知道从什么地方去加载哪些字节码文件(.class)，这个内容在讲述JVM原理时详细讨论。

### 1.2.2 声明和定义

注意：这一节的内容，为了简单易懂，笔者尽量采用通俗化的语言描述。事实上，还有很学术化、形式化的表述方式，有兴趣的读者可以阅读Oracle官方网站上的 ”The Java Language Specification“的第二章”[Grammars](https://docs.oracle.com/javase/specs/jls/se17/html/jls-2.html)“。

声明（Declaration）和定义（Definition）是两种不同的行为。在Java中，区别并不是很明显，而有些语言（比如C/C++），声明和定义是截然分开的。按照约定的方式编写Java代码，甚至您意识不到是在Declare还是在Define。不过，从概念上还是应该搞清楚它们，专业和业余的区别，往往在于细节。

```java
public interface UserService{
	public boolean createUserAccount(User user);
	public List<User> findUserAccounts(int department);
}
```

上面的接口UserService中包含了两个方法的**声明**。换言之，这两个方法只给出了形式上的描述（可见性、返回值类型、方法名、参数类型），并没有实质，即没有给出方法具体的实现。又比如以下代码：

```java
String userName;
List<User> users;
UserService userService;
```

上面的三行代码有个共同特征：给出了变量名字和类型，但没有初始化。我们把这种情况叫作变量声明，就是只有形式没有实质。

以上两个例子，都提到了“实质”这个词。这个实质，就是声明和定义的分野，它有两个含义：

1. 分配了内存空间（定义变量）；
2. 实现了具体行为（定义类型）。

因此，上面的两段代码，都是声明。编译器只把它当作一个符号，这个符号如果是个变量，则不指向任何一块合法的内存空间；如果是个方法，则不具有任何行为。

我们再看以下有“实质”的代码：

```java
public class UserServiceImpl implements UserService{
	public boolean createUserAccount(User user){
		if(user == null) return false;
		return DatabaseUtil.insert(user);
	}
	public List<User> findUserAccounts(int department){
		if(department == 0) return List.of();
		var result = new ArrayList<User>();
		//Get user accounts from database
		return result;
	}	
}
```

这段代码定义了一个名为UserServiceImpl的类，它把interface UserService中声明的两个方法实现了，不再是一个空壳了，已经拥有了具体的行为。下面的代码则演示了分配内存空间：

```java
int a;	//没有显示地初始化，但编译器会给出缺省值
double d = 10d;
String userName = "John Denver";
List<User> users = new ArrayList<>(128);
UserService userService = new UserServiceImpl();
```

以上五行代码，每一行，都包含了变量类型、名字和初始值，让变量名字这个符号与栈或者堆上的一块内存空间联系起来了。需要注意的是：这些代码具有双重行为：先声明了符号，然后再赋予符合以实质意义。所以我们说：定义包含了声明，而声明不包含定义。

自Java 8开始，支持在接口中为方法提供缺省实现。这个新特性就模糊了声明和定义的区别，只能这么说：有些声明中也包含了定义。

在Java中，变量、函数和用户定义的引用类型，都必须先定义再使用。变量定义的一般形式为：

**修饰符 类型 名称 = 初始值**

```java
private int count = 10;
public String bandName = "The brothers four";
```

Java是一种类C语言，变量定义的语法和C/C++/C#是一样的。有些语言则稍有不同，变量名和类型名的顺序是相反的，比如：

```rust
//Rust
let v: Vec<i32> = Vec::new();
```

上面的Rust代码声明了一个变量并进行了初始化，它是一个存放4字节整数的向量容器。事实上，各种风格之间没有什么优劣之分，只是语言设计者的偏好，以及程序员的习惯。

在Java中，函数/方法不能独立存在，它被包含在一种引用类型的内部。其的定义遵循以下模式：

**修饰符 返回值类型 方法名(参数列表){ }**

```java
public long subtract(int i, int j){
	return i + j;
}

//没有返回值类型的函数
public void echo(String name){
	System.out.println("Your name is " + name);
}
```

引用类型是一种.符合类型，或者说是一种类型容器，它里面包含了若干成员。这些成员可以是基本类型、也可以是别的引用类型，甚至是自己。 其定义的一般形式为：

**修饰符 引用类型 名称{ 成员 }**

```java
//定义一个类
public class Node{
	private int id;
	private String name;
	private Node parent;
	
	public int getId(){
		return this.id;
	}
}

//定义一个枚举类型
public enum Color{
	RED(0xff0000),
	GREEN(0x00ff00),
	BLUE(0x0000ff);

	private int code;
	
	Color(int hexCode){
		this.code = hexCode
	}
	
	public int getCode(){
		return this.code;
	}	
}

//声明一个接口
public interface Cache{
    public boolean set(String key, String value);
    public String get(String key);
}
```

Java 8（通过JSR 330）引入了lambda表达式，使得Java支持一些函数式编程的特性，它使得方法的定义看起来不直观，比如以下代码，遍历了集合中的每个元素：

```java
List<User> users = new ArrayList<>();
users.forEach(user->{
	System.out.println(user.getName());
});
```

Lambda表达式里的“->”符号，有些语言里用“=>”，意思是一样的。

还有一种类的定义方式，叫匿名类，也叫做匿名内部类。所谓匿名就是没有名字，在需要用到的地方直接定义，最常见的是线程的Runnable接口：

```java
new Thread(new Runnable() {
	public void run() {
		//Do something here
	}			
}).start();
```

这种写法看起来要简洁一些，但会造成一个概念上困扰：Runnable是一个接口，而我们前面讨论过，interface是不能实例化的，这里恰恰new了一个Runnable。好在后面还有一对花括号,包含了对run方法的实现，而不是分号。换成常规的写法应该先定义一个实现了Runnable接口的类，再将其作为参数传递给Thread类的构造函数，就像下面这样：

```java
class Task implements Runnable{
	public void run(){
		//Do something here
	}
}
Thread thread = new Thread(new Task());
thread.start();
```

### 1.2.3 范围和修饰符

1.1.2小节中关于变量、方法、引用类型定义的一般形式中，反复出现了**修饰符**（Modifier）这个名词。修饰符是一类关键字（Keyword），用以表示被修饰对象具备某一种特性。修饰符可以是多个，比如定义一个常量：

```
public static final int POOL_SIZE = 1024;
```

我们将修饰符分成几类，先介绍最常用的一类，关于对象可见性的修饰符：

- public

  凡是被public所修饰的对象，是能够被其它对象通过一定的方式访问到的。

- private

  私有的意味着，它不能被外部访问，可见性局限于class的内部。

- protected

  如果一个东西，它的可见性是”被保护的“，它的可见性介于public和private之间，在内部、同一个package下以及继承体系内，可以被访问到。C++中，被称之为friend，友元。

  ```java
  //这个类，只能被com.gugou.core这个包下的其它类访问到
  package com.gugou.core;
  protected class User{
  	//......
  }
  ```

  ```
  public class Person{
  	protected double salary;
  }
  
  public class Man extends Person{
  	public double howMuch(){
  		return this.salary;
  	}
  }
  
  //子类Man能够访问父类中定义的protected属性，而外部则无法访问。
  ```

  除了以上三种之外，还有一种默认的可见范围。如果省略掉可见性关键字，默认的可见范围是class的内部以及同一个package下。上文在讨论程序基本结构时讲过，一个源代码文件中，只能有一个public修饰的class。这意味着，源文件里有非public修饰的class也是合法的：

  ```
  //Person.java
  
  public class Person{
  	private Name name;
  }
  //类Name的可见性，就是default
  class Name{
  	private String firstName;
  	private String lastName;
  	
  	//......
  }
  ```

  Java提供了一种机制叫作反射（Reflection），哪怕您声明成了private，也能够通过反射的途径访问到它，只不过麻烦一些。那么为什么还要多此一举地搞出可见性这个概念来呢？我们可以将它理解为一个软件工程的约束，而不是一个单纯的安全性解决方案。作为开发者，应该思考什么东西需要在什么范围内公开，才能营造一种更好的秩序和规范。

与可见性有一点相似的概念是变量的范围（Scope），也可以称之为变量的生命周期（Life Cycle）。离开有效范围之后，变量就失效了。Java中变量的范围有三种情况：

- 类变量（Class Variable）

  用static关键字修饰的类成员变量，无须实例化就能访问：

  ```java
  class ThreadPool{
  	public static int maxThreads = 8;
  }
  
  //在别处可通过如下方式访问：
  var threads = ThreadPool.maxThreads;
  ```

- 实例变量（Object Instance Variable）

  必须先实例化这个类，获得一个Object，然后才能通过Object访问到该变量：

  ```java
  class User{
      public int id;
      private String name;
      
      public User(int id, String name){
          this.id = id;
          this.name = name;
      }
      
      public String getName(){
          return this.name;
      }
  }
  
  //只能以如下方式访问实例变量id和name:
  var user = new User(1, "Eric Clapton");
  var id = user.id;
  var name = user.getName();
  
  //当实例对象失效以后，实例变量也就不存在了，它们之间是皮和毛的关系。
  user = null; 
  user.getName();	//此时，将引发NullPointerException
  ```

  访问类变量和实例变量的方式是统一的，都是用.操作符。在C++和Rust语言里，访问类变量用两个冒号（::）这个操作符。

- 局部变量（Local Variable）

  在一个方法、或者代码块内定义的变量，作用范围只在当前花括号范围内，而且与顺序严格相关：

  ```java
  class Test{
      public int test(){
          int i = 1;	//i是局部变量，只在test方法内有效
          return ++i;        
      }
      
      public int add(int i, int j){
          result = i + j;	//无法通过编译，此时result还没被定义
          var result = 0;
          return result;
      }
  }
  ```

第二类关键包含了两个：abstract（抽象）和static（静态）。把它们放在一起讨论，有一点强扭的意思，但也有共通之处，都与实例化（Instance）相关：

- abstract决定了一个类是否能够被实例化

  ```java
  public abstract class SecurityManager{
  	public boolean isAllowed(int userId);
  	public boolean checkSesssion(String session);
  }
  ```

  用abstract修饰的class，是不能被实例化的，比如以下代码，就无法通过编译：

  ```java
  SecurityManager manager = new SecurityManager();
  ```

  必须要有一个派生自SecurityManager的子类（subclass），实现这个抽象类的方法，然后实例化这个子类：

  ```
  public class MySecurityManager extends SecurityManager{
  	public boolean isAllowed(int userId){
  		return userId > 0;
  	}
  	
  	public boolean checkSession(String session){
  		return session != null && session.trim().length > 0;
  	}
  }
  
  //下面这一行代码就合法了
  var manager = new MySecurityManager(); 
  ```

  所以某种程度上，抽象类就充当了接口的作用。语言的设计者，为什么要高出两个类似的东西来呢？请看以下代码：

  ```java
  public abstract class SecurityManager{
  	public boolean isAllowed(int userId){
  		return userId > 0;
  	}
  	public abstract boolean checkSesssion(String session);
  }
  
  public class MyScurityManager extends SecurityManager{
      @Override
      public boolean checkSession(String session){
  		return session != null && session.trim().length > 0;
  	}
  }
  
  var manager = new MySecurityManager(); 
  //调用了super class的缺省实现
  var allowed = manager.isAllowed(3);
  //调用了子类自己的实现
  var blocked = !manager.checkSession(null);
  ```

  抽象类中的方法，可以有具体的实现而不仅仅是声明，它的子类只需要覆写(Override)被标记为abstract的方法即可。super class和sub class之间用的是extends这个关键字，表达的是继承关系。

  而接口和类之间的关系是实现（implement）。在接口中只能声明方法，有实现类来完成具体要做的什么事情。但是，自Java 8开始，也允许接口可以提供方法的缺省实现了：

  ```java
  public interface SecurityManager{
  	public default boolean isAllowed(int userId){
  		return userId > 0;
  	}
  	public boolean checkSesssion(String session);
  }
  ```

   所以，这二者之间可能仅仅是语义上的区别了，或者特定场合、特殊用途上必须用某一种方式（比如，如果要使用Java的动态代理功能，则需要用interface）。

- static决定了一个成员是否需要被实例化才能访问。

  先看一段代码：

  ```java
  long time = System.currentTimeMillis();
  int maxValue = Integer.MAX_VALUE;
  List<Object> array = List.of();
  ```

  这三行代码有一个共同点：直接用ClassName.xxx，而不是先new一个Object,再访问object.xxx。因为xxx在class里面，用static关键字修饰了，所以能够直接通过类名访问，而不需要实例化。下面是JDK中List.of方法的源代码：

  ```java
  //java.util.List 
  static <E> List<E> of() {
   	return ImmutableCollections.emptyList();
   }
  ```

  同样，JDK中Integer.MAX_VALUE被定义成一个常量（Constant）：

  ```java
  public static final int   MAX_VALUE = 0x7fffffff;
  ```

  简而言之，凡是被static所修饰的成员，不需要实例化就能访问。如果该成员是一个变量，称之为类变量（Class Variable），当ClassLoader第一次加载的时候，就被初始化了，它的值只有一份，不会被其它的实例化对象所影响。利用这个特征，我们可以用static来实现一个全局缓存，让一些值在整个应用的生命周期内，都能够被访问到，而且具有唯一性：

  ```java
  //缓存所有的登录用户
  public class Cache{
  	public static final Map<Integer, String> ONLINE_USERS = new HashMap<>(512);
  }
  
  public class UserManager{
  	//用户登录时，把它信息存起来
  	public boolean login(int user, String name){
  		if(user == 0 || name == null) return false;
  		Cache.ONLINE_USERS.put(user, name);
  		return true;
  	}
  	
  	//需要用的时候，从缓存中查找它即可
  	public boolean isUserOnline(int id){
  		return Cache.ONLINE_USERS.containsKey(id);
  	}
  }
  ```

  既然static具有这么多方便的特性，那我们把所有的属性和方法都用static修饰就好了嘛？理论上也不是不行，但是要考虑到一些问题：

  1. 面向对象编程的意义何在呢？这其实完全退化成过程式编程了；
  2. 占用不必要的存储空间，有些东西，用一次就完了，并不需要它常驻内存；
  3. 作为一个程序员，编程需要克制，否则再好的特性也会成为陷阱甚至地狱。

  static还可以被用来修饰一段代码。这样的代码块只在类加载时执行一次，可以用来作某些static修饰的变量的初始化，比如上面的Cache，可以这样实现：

  ```
  public class Cache{
  	private static Map<Integer, String> onlineUsers = null;
  	
  	static{
  		onlineUsers = new HashMap<>(512);
  	}
      
      public static void cacheUser(int id, String name){
          if(id == 0 || name == null) return;
      	onlineUsers.put(id, name);
      }
      
      public static String getUser(int id){
          return onlineUsers.get(id);
      }
  }
  ```

第三类修饰符，用来描述class的可继承性，包括final和sealed（Java 17）。final和sealed的意思差不多，但作用又不完全一样：

- final

  如果一个类用final修饰，任何情况下，都不能被继承了，最典型的例子就是标准库中的String类：

  ```java
  public final class String
      implements java.io.Serializable, Comparable<String>, CharSequence {
  	//......
  }
  ```

  假如我觉得标准库的String某些地方让我不爽，想改善一下，于是就这样：

  ```java
  public class MyString extends String{
  	//......
  }
  ```

  上面的方法是无法通过编译的。所以如果您认为某个类的行为是自洽的、自封的，不需要被任何人改写，就用final修饰它好了。

  如果一个变量用final修饰，就是readonly的意思，其值不可被改变。final通常和static结合，用来定义常量，这样的例子数不胜数，一个典型的用途就是，避免代码里频繁出现所谓的魔鬼数字（Magic Number），使用字面常量代替它：

  ```java
  public static final int TCP_PORT = 3721;
  public static final int PAGE_SIZE = 8192;
  public static final int MAX_CONNECTIONS = 16;
  ```

- sealed

  用sealed修饰的类叫做密封类，不能被继承（C#中也有seal这个关键字，一样的意思）。final太严格了，真的是密不透风，但seal在密封的时候又使用permits关键字给开了个口子，允许有例外，比如：

  ```
  //Ball.java
  public sealed class Ball permits Football{
  	public void play(String player){
  		System.out.println(player + "is playing the ball");
  	}
  }
  
  //FootBall.java
  public class Football extends Ball{
  	@Override
  	public void play(String player){
  		System.out.println(player + "is playing football.");
  	}
  }
  
  //VolleyBall.java
  public class Vollyball extends Ball{
  	@Override
  	public void play(String player){
  		System.out.println(player + "is playing volleyball.");
  	}
  }
  ```

  class Ball是一个密封类，它只允许名字叫Football的类可以继承它，其它则是不允许的，所以Volleyball就无法通过编译。请注意，permits关键字只能出现在extends, implements之后。如果您还没切换到Java 17，就用不了sealed这个feature了。

### 1.2.4 运算和运算符

计算机（Computer）的特长当然是计算（Computing）。在类型系统那一节中，我们发现基本数据类型几乎都是数值型，而数值型天然就是用于计算的。所以这一小节我们考察一下计算规则和运算符。根据运算符所需的操作数的数量，可以分为：

- 单目运算

  只需要一个操作数，比如布尔运算中的非运算：!false == true

- 双目运算

  运算符左右各需要一个操作数。绝大多数运算都是双目运算，比如：int c = 2 + 1

- 三目运算

  需要三个操作数的运算叫做三目运算，目前只有一个：? :，它可以让一些复杂的写法变得简单一些：

  ```java
  for(var i = 0; i < 100; i++){
  	boolean result = i % 2 == 0 ? false : true;
  }
  
  //上面的代码等价于：
  for(var i = 0; i < 100; i++){
  	boolean result = false;
      if(i % 2 == 0){
          result = false;
      }else{
          result == true;
      }
  }
  ```

  从运算的性质看，可以将运算符分为以下几大类：

1. 布尔运算

   布尔运算也叫逻辑运算，学过《离散数学》的读者会有比较熟悉。在介绍基本类型时，有个boolean类型，它的取值只有true or false两种情况，只有boolean类型参能参与逻辑运算。逻辑运算有三种：与（AND）、或（OR）、非（NOT）。

   | #    | 运算      | 运算符 | 优先级 | 类型     | 运算规则                                  |
   | ---- | --------- | ------ | ------ | -------- | ----------------------------------------- |
   | 1    | 与（AND） | &&     | 其次   | 双目运算 | 同时为真，则为真：true && true == true    |
   | 2    | 或（OR）  | \|\|   | 最低   | 双目运算 | 任一为真，则为真: true \|\| false == true |
   | 3    | 非（NOT） | !      | 最高   | 单目运算 | 对当前值取反: !true == false              |

   布尔运算的结果通常用来作为分支选择的依据，这在“流程控制”一节将详细讨论。掌握布尔运算不仅对编程有用处，它也能训练人的逻辑思维能力，任何地方都用得上。

2. 算术运算

   算术运算就是小学数学中讲的四则运算，加（+）减（-）乘（*）除（/）。CPU里面有个重要的部件叫做算术逻辑单元（Arithmetic&logical Unit，ALU），就是专门负责进行算术运算和布尔运算的。我们知道乘法可以转换为加法、除法可以转换成减法运算，所以一个简单的CPU，只要有加法器和减法器，就可以完成算术运算。对CPU来讲，整数的运算性能很高，而浮点运算则需要更多的时间开销（因为浮点数遵循IEEE-754规范，它的表达方式更复杂）。因此衡量CPU性能的一个常用指标就是FLOPS（每秒浮点数运算次数）。有一些应用涉及到大量的密集的算术运算，比如图形处理、神经网络等，于是设计出了具有更多的ALU的专用处理器，比如GPU，或者NPU。

   Java在语言层面，支持下表所列出的算术运算，其它的比如乘方、开方、三角函数等，则需要用到java.math.Math类来处理。

   | #    | 运算 | 运算符 | 类型     | 运算规则                   |
   | ---- | ---- | ------ | -------- | -------------------------- |
   | 1    | 加法 | +      | 双目运算 |                            |
   | 2    | 减法 | -      | 双目运算 |                            |
   | 3    | 乘法 | *      | 双目运算 |                            |
   | 4    | 除法 | /      | 双目运算 | 注意除数不能为0            |
   | 5    | 取模 | %      | 双目运算 | 求余数，比如： 10 % 5 == 0 |
   | 6    | 自增 | ++     | 单目运算 | i = i + 1的另一种形式      |
   | 7    | 自减 | --     | 单目运算 | i = i - 1的另一种形式      |

   这里着重谈一下自增（++）和自减（--）的问题。这是两个单目运算符，但操作数的位置可以在运算符的左边或者右边：

   - 操作数在左边，即：i++, i--

     如果在一个赋值语句中，它表示先赋值，再将变量自增1。

   - 操作数在右边，即：++i， --i

     如果在一个赋值语句中，它表示先将变量自增1，再赋值。

   ```java
   var i = 0;
   //此时，n = 0, i = 1
   var n = i++;
   
   var i = 0;
   //此时，n = 1, i = 1
   var n = ++i;
   ```

   对于变量i来讲，最终的值都是1，而n的值有所差别。所以读者应根据具体场景和目的，选择用哪一种表达方式。i++的赋值操作等价于以下代码：

   ```java
   var i = 0;
   var j = i;	//临时变量
   i = i + 1;
   var n = j;
   ```

   而++i的赋值语句等价于：

   ```java
   var i = 0;
   i = i + 1;
   var n = i;
   ```

   肉眼看起来，++i比i++要少一行代码，即先用临时变量j保存了i的值，所以可能会有极其微小的性能差别。但是现代编译器聪明得很，优化之后的字节码/汇编代码就没任何去别的了。一些教科书，或者一些试题，往往会过于纠结于这两个操作符，搞出一个包含了++/--的复杂无比的表达式来求值，其实除了绕晕脑袋之外，没什么意义。

   算术运算实在没什么可讲的，但是有一个小学生都知道，但程序员还经常搞出Bug来的问题：被零除。

   ```java
   public double divide(int i, int j){
       return i / j;
   }
   ```

   这段代码，如果参数j传入0，将会引发一个ArithmeticException，Divide by zero。所以，应该在方法入口处对参数做一些合法性检查。编程是一个高度细致的工作，要小心谨慎，步步为营，避免代码里出现这类难以察觉的bug。我们再看看下面的代码：

   ```java
   public int add(int i, int j){
   	return i + j;
   }
   
   public int multiply(int i, int j){
   	return i * j;
   }
   
   int m = add(2000000000 + 2000000000);
   int n = multiply(1000000 * 10000000);
   ```

   后面两行代码，就无法获得正确的结果，因为i和j相加、相乘，都已经超出了int型的最大表示范围，这叫作溢出（Overflow）。最致命的是，虚拟机还不会抛出任何异常，只会得到一个怪异的数字（比如循环相加的结果）。要解决这个问题，可以用表示范围更大的BigInteger类，或者long、double等数据类型。据说，1996年，NASA发射Ariane五号火箭失败，就是因为一行代码：

   ```ADA
   horizontal_veloc_bias := integer(horizontal_veloc_sensor);
   ```

   在将horizontal_veloc_sensor转换为一个integer（ADA语言的16位的int，相当于Java的short）时溢出了，导致火箭偏离飞行路径，爆炸了。

   从这里，我们发现了动态语言的一个绝妙的优势了：运行时类型推定，不会导致溢出，比如下面的代码：

   ```javascript
   //Javascript
   function add(i, j){
   	return i + j;
   }
   
   //Python
   def add(i, j):
   	return i * j
   ```

   Javascript里，数值类型都用Number来表示，就没有溢出的担忧；当Python的VM发现溢出时，就会自动转换到更宽范围的类型，比如从int升级到long，也不会发生溢出。如果NASA选用Javascript或者Python而不是Ada来开发，就可以避免这个悲剧啊。

3. 位运算

   现代计算机是一种以二进制（Binnary）为基础的计算工具，它的基本的表示单位是字节（Byte），而每个byte由8个位（Bit）构成，每个位的取值只有两种：0或者1。因此计算机内部信息表示的方式都是一串一串的0和1夹杂的数字，位操作（Bit Manipulation）就是针对这些二进制位进行的运算。二进制的知识就不赘述了，只介绍Java支持的位运算：

   | #    | 操作        | 运算符 | 优先级 | 运算规则                                                     |
   | ---- | ----------- | ------ | ------ | ------------------------------------------------------------ |
   | 1    | 位与（AND） | &      |        | 按位进行与运算，同为1则为1：00010001 & 11111111 == 00010001  |
   | 2    | 位或（OR)   | \|     |        | 按位进行或运算，任意为1则为1：00010001 \| 11111111 == 11111111 |
   | 3    | 异或（XOR） | ^      |        | 按位比较，相同为0，不同为1：00010001 ^ 10010100 = 10000100   |
   | 4    | 取反（NOT） | ~      |        | 按位取反: ~01010101 == 10101010                              |
   | 5    | 左移        | <<     |        | 左移1位相当于乘以2：2 << 2  == 8                             |
   | 6    | 右移        | >>     |        | 右移一位相当于除以2：8 >> 2 == 2                             |
   | 7    | 无符号右移  | >>>    |        | 右移，但不考虑符号位，一律补0                                |

   普通的布尔运算是针对boolean型变量的，而位运算中的按位与、或、异或、取反是针对byte进行的。它的用途很多，举几个例子：

   - 用左移右移代替乘法和除法，性能更好：

     ```java
     var result = 12 * 8；
     var result = 12 << 3;
     
     //以上两个语句是等价的，但第二个采用了位运算，执行效率更高
     ```

   - 将四个字节转换为一个int型整数（反之亦然）：

     ```java
     public int bytesToInt(byte[] bytes) {
     	if(bytes == null) return 0;
     	if(bytes.length != 4) return 0;
     	return  (bytes[3] & 0xFF) |
     		(bytes[2] & 0xFF) << 8 |
     	    (bytes[1] & 0xFF) << 16 |
     	    (bytes[0] & 0xFF) << 24;
     }
     ```

   - 业务场景上的用途，比如用来保存某用户的角色，以及判断该用户是否具有某个角色：

     ```java
     //假设一个应用系统，有8个用户角色，分别是Role1, Role2, Role3...Role8
     //假设用户张三具有Role2, Role5, Role7三个角色，那么我们可以用一个byte来表示：
     
     var roles = 74; //对应的二进制序列为：01010010 (第2、5、7位为1，表示该用户的角色)
     
     //假设，在执行某个操作时，需要该用户具有Role4的权限：
     //8的二进制序列是：00001000,即Role4的位置为1。将其与74进行位与预算：
     /**         01010010
     *         & 00001000
     *      ———————————————
     *           00000000
     */
     if(roles & 8 == 0){
     	//不具备Role4角色的权限
     }
     ```

4. 赋值和比较运算符

   赋值（Assign）是编程中时时刻刻都在进行的操作，就是把一个值赋予一个变量。大多数语言里，赋值操作符都是等号（=），也有一些语言用别的形式，比如Pasical就用冒号-等号（:=）表示赋值，读者们看见了也别觉得奇怪。赋值操作涉及到两个新的概念：左值（Lvalue）和右值（Rvalue）。从字面意义理解的话，在一个赋值语句中，左值就是出现在等号左边的值，右值就是出现在等号右边的值。但是这是一句正确的废话，等于什么也没说，因此还需要深入一点考察：

   - 左值：该符号指向一块内存区域的地址（Addressable），该地址所存储的内容可以被改变；
   - 右值：该符号是一块内存区域中的内容（Readable），或者由这类符号、字面量所组成的表达式。

   这个说法不太好理解，还是用代码来解释更直观一些。当然如果实在理解不了就算了，不影响您写代码的：

   ```java
   // i是一个变量，它指向了栈上的一块内存区域（地址），而这块内存里存放了4个字节。
   // 所以，i是左值，而右边的 2 * 3，就是右值，它由两个字面量构成了一个表达式。
   int i = 2 * 3;
   
   //下面这行代码是错误的，2 * 3 不能作为左值，编译器会报错：
   // The left-hand side of an assignment must be a variable
   2 * 3;
   
   //下面这行代码是正确的，它意味着，变量既可以是左值，也可以是右值。但是它出现在赋值符号左右的含义不一样：
   //在左边表示内存区域的地址，而在右边表示了该内存区域所存放的4个字节所代表的int数值。
   var i = 2 * 3;
   i = i + 2;
   ```

   赋值符可以与前述的算术运算符和位操作符（只能是双目运算符）组合起来，形成组合运算符：

   | #    | 组合运算符 | 含义                      |
   | ---- | ---------- | ------------------------- |
   | 1    | +=         | i += j 等价于 i = i + j   |
   | 2    | -=         | i -= j 等价于 i = i - j   |
   | 3    | *=         | i *= j 等价于 i = i * j   |
   | 4    | /=         | i /= 2 等价于 i = i / 2   |
   | 5    | %=         | i %= 2 等价于 i = i % 2   |
   | 6    | &=         | i &= 2 等价于 i = i & 2   |
   | 7    | \|=        | i \|= 2 等价于 i = i \| 2 |
   | 8    | ^=         | i ^= 2 等价于 i = i ^ 2   |
   | 9    | <<=        | i <<= 2 等价于 i = i << 2 |
   | 10   | >>=        | i >> 2 等价于 i = i >> 2  |

   请注意，组合运算符中间不能有空白字符，必须紧紧靠在一起，它的作用就是让代码看起来更简洁一些，如此而已。

   比较运算符都是双目运算符，用来比较运算符左右的两个对象的等价情况：

   | #    | 运算符 | 含义                     |
   | ---- | ------ | ------------------------ |
   | 1    | >      | 判断左边是否大于右边     |
   | 2    | <      | 判断左边是否小于右边     |
   | 3    | ==     | 判断左边是否等于右边     |
   | 4    | !=     | 判断左边和右边是否不相等 |
   | 5    | >=     | 判断左边是否大于等于右边 |
   | 6    | <=     | 判断左右是否小于等于右边 |

   比较运算符通常用于if-else、while等需要条件判断的语句中。只有数值类型的数据才能进行大小比较，基本类型都能用等号进行判等操作，单引用类型就稍微有些不同了，一般不使用等号去判断，而是用一个专门的方法：equals。我们说，两个引用类型相等，不是指字面量的相等，而是这两个类型所指向的是同一块内存空间，它们的地址相等。这是最基本的比较方法，参见Object类的equals方法的实现：

   ```java
   public boolean equals(Object obj) {
   	return (this == obj);
   }
   ```

   也就是说，如果您定义了一个class，没有覆写继承自Object的equals方法，那么缺省的行为是比较两个对象的内存地址。但是Java也允许您通过覆写equals方法改变比较规则，比如，两个用户的ID相同，我们就认为是同一个人：

   ```java
   public class User{
   	private int id;
       private String name;
       
       @Override
       public boolean equals(Object obj){
           if(obj == null) return false;
           if(!(obj instanceof User)) return false;
           return this.getId() == obj.getId();
       }
   }
   ```

   Java标准库中，也有很多覆写equals改变判等规则的例子，最典型的就是基本类型对应的引用类型。下面是Integer类的equals方法：

   ```java
   public boolean equals(Object obj) {
   	if (obj instanceof Integer) {
       	return value == ((Integer)obj).intValue();
        }
        return false;
   }
   ```

   它将包装在引用类型中的基本类型的数值取出来，直接比较大小，跟基本型比较的行为保持一致。Java要求（至少是强烈建议）当覆写equals的时候，也要override另一个方法hashCode，不违背“相等的对象的hash code也必须相等”这个约定。

5. 运算符重载

   运算符重载（Operator Overloading）是C++提供的一个很酷的功能，就是可以改变运算符原本的用途和语义。Java不支持程序员进行运算符重载，但是我们又可以找到这种用法的影子，比如：

   ```java
   String name = "Jhon " + "Denver";
   //此时，name的值为“John Denver”
   ```

   很明显，此处的运算符（+）并不是原来的算术运算的意思了，而是进行了字符串连接（Concatenate）操作，相当于：

   ```java
   String name = "Jhon ".concat("Denver");
   
   //或者：
   String name = new StringBuilder("Jhon ").append("Denver").toString();
   ```

   同样地，两个数值类型的包装类，也可以用关系运算符进行比较。这是运算符重载的另一个例子：

   ```java
   Integer i = 100;
   Integer j = 110;
   if(i > j){
       System.out.println("That's impossible!");
   }
   ```

   当然，这可以有另一种理解：当Integer型的变量i和j，出现在比较操作符的左右时，瞬间完成了一次Unboxing操作，退化成两个int型的数值了，可以直接比较。

### 1.2.5 流程控制

我们反复说，程序设计语言是用来描述现实世界的，它一定要有足够的表达能力。现实世界充满了无数的流程性的事务，比如老师说：单词听写没有全对的同学，放学回家后，把写错了的单词抄写50遍。这句话就涉及到了条件判断（是否听写全对）和循环（重复抄写50遍）。如果用Java代码来表达，类似下面这样：

```java
if(hasError){
	for(int i = 0; i < 50; i++){
		writeErrorWords();
	}
}
```

最简单的流程就是顺序结构，一行一行的语句，按先后顺序执行。从微观层面看，所有的源代码被编译成机器指令之后，CPU也是这样按顺序逐条执行指令的，当然设计微架构时可以通过流水线等手段提升执行效率或者SIMD实现并行处理，这涉及到计算机体系结构的知识，先不深究它。如果要打破这种顺序结构的简单流程，想跳来跳去、重复执行或者延迟执行，就需要引入其它几种流程控制手段：

- 分支（Branch）

  根据不同条件，做出不同的反应，这可能是编程中最频繁的操作了，任何一个有用的软件，代码里都充满了各种各样的分支。Java支持两种分支语句，if-else和switch-case。

  如果分支条件比较少（具体多少？比如少于5），就用if-else，它类似于一个有限状态机，代码逻辑会清晰一些：

  ```java
  public void doSomething(int hour){
      if(hour == 8){
          haveBreakfast();
      }else if(hour == 12){
          haveLunch();
      }else if(hour == 18){
          haveDinner();
      }else{
          workOrSleep();
      }
  }
  ```

  需要注意的是，所有的条件应该是同一维度的，而且条件要互斥，而且要覆盖到全部可能性，才有选择的价值，否则就可能跳转到您不想要的分支上了。这个条件，可以是单一的条件，也可以是几个条件通过逻辑运算组合的结果。针对条件比较多的情况，可以用switch-case来表达，下面的函数将颜色十六进制值转换为颜色名字：

  ```java
  public String colorCode2Name(int code){
      var result = "";
      switch(code){
          case 0xff00:
              result = "red";
              break;
          case 0x00ff00:
              result = "green";
              break;
          case 0x0000ff:
              result = "blue";
              break;
          case 0x000000:
              result = "black";
              break;
          case 0xffffff:
              result = "white";
              break;
          default:
              result = "unknown";
              break;
      }
      return result;
  }
  ```

  最后一个default分支，和else的用途一样。当前面所有case都无法匹配时，给出一个缺省的行为。switch-case没有if-else那么聪明，匹配上某个条件之后，它还会继续往后匹配，所以通常结合break一起使用，实现if-else的效果。但是程序员也可以不用break，做一些投机取巧的事情。下面的代码利用于计算每个月有多少天：

  ```java
  public int maxDays(int year, int month) {
  	switch(month) {
  		case 1:
  		case 3:
  		case 5:
  		case 7:
  		case 8:
  		case 10:
  		case 12:
  			return 31;
  		case 4:
  		case 6:
  		case 9:
  		case 11:
  			return 30;
  		case 2:
  			return year % 4 == 0 ? 29 : 28;
  		default:
  			return 0;
  	}
  }
  ```

  只要month的值为[1、3、5、7、8、10、12]的任意一个，都会执行return 31这行代码，这个特性叫做fall through。Java和C语言是一个套路，默认为break语句是标配。当然这也是个双刃剑，有时候搞忘了写break，就引入一个莫名其妙的Bug。为了杜绝这个问题，语言的设计者们，都想了一些办法，比如：

  - C++：如果缺少break，编译器会给出一个Warning。程序员要么去补上break，要么加上fallthrough这个语句（C++ 17），编译器才会善罢甘休；
  - GoLANG：默认情况下，写或者不写break效果都跟if-else一样。想要特殊效果，程序员就用fallthrough关键字告诉编译器，改变默认行为即可，Apple设计的Swift语言也是如此。

  Java中的switch-case还有一些有别于if-else的特点，包括：

  - 每个case语句并不是一个独立的代码块，所以不能在不同的case下定义相同名字的变量：

    ```
    switch(condition){	//请注意花括号的位置
    	case 0:
    		int i = 0;	// 第一处
    		break;
    	case 1:
    		int i = 2;	//第二处
    		break;
    	default:		
    }
    ```

    上面的代码无法通过编译，因为整个switch是一个代码单元，根据前述的变量作用范围的规则，不能重复定义变量i。

  - switch条件变量的类型。早期版本只支持整数型的（byte, short, int, long , char）变量，Java 7之后，又支持String类型了：

    ```java
     switch(name){
     	case "John":
     	case "Mike":
     	case "tony":
     }
    ```

    这样的写法是合法的。一个小小的改变，却大大地提高了switch-case的表达力，能够应对一些非整数的场景。

  - switch-case的条件变量还支持枚举类型：

    ```java
    public enum Color{
        RED,
        GREEN,
        BLUE;
    }
    
    public int translate(Color color){
     	switch(color){
         	case RED:
                return 0xff0000;
            case GREEN:
                return 0x00ff00;
            case BLUE:
                return 0x0000ff;
            default:
                return 0x00; //ERROR
        }   
    }
    ```

    从代码风格的角度，提倡将一组常量用一个枚举类型替代。switch-case支持枚举类型，为更好的编程实践提供了便利。

- 循环 （Loop）

  反复做同一件事情叫作循环。Java提供了for, while, do-while以及forEach四种方式实现循环，它们在语义和应用场景方面有些差别：

  1. 预先知道要做多少次，就用for语句来实现。例如把1到100的数字相加求和：

     ```java
     int result = 0;
     for(int i = 1; i <= 100; i++){
     		result += i;
     }
     ```

  2. 不知道要做多少次，什么时候退出循环靠外部条件，就用while语句来实现。比如当一个随机数模3余2时退出：

     ```java
     boolean condition = true;
     while(condition){
     	var random = new Random();	
     	if(random.nextInt() % 3 == 2){
     		condition = false;
     	}
     }
     ```

  3. 无论什么情况，先做一次再看条件是否继续做，就用do-while语句来实现。比如一帮法外狂徒决定：今天先去抢一次银行，如果没被警察逮住的话，明天继续抢，直到被抓住为止：

     ```java
     boolean caught = false;
     do{
     	robBank();
     	caught = catchRobbers();
     }while(!caught)
     ```

  4. 无限循环，直到系统shutdown，或者因为意外而退出。可以用for或者while来实现：

     ```java
     for(;;){
     	doSomething();
     }
     
     while(true){
     	doSomething();
     }
     ```

  5. 支持Lambda表达式的forEach遍历，List, Map, Set等等实现了Iterable接口的集合类型都支持这种写法：

     ```
     List<User> users = new ArrayList<>();
     users.forEach(user->{
     	System.out.println(user.getId());
     });
     
     //上面的代码等价于：
     for(int i = 0; i < users.size(); i++){
     	var user = users.get(i);
     	System.out.println(user.getId());
     }
     ```

  6. 通过迭代器进行集合遍历，数组和List, Map, Set等实现了Iterable接口的集合类型都支持这种写法：

     ```java
     List<User> users = new ArrayList<>();
     
     for(var user : users){
         System.out.println(user.getId());
     }
     
     //另一种迭代器模式是显示地使用Iterator接口，与上面代码等价的
     Collection<User> users = new ArrayList<>();
     var iterator = users.iterator();
     while(iterator.hasNext()){
         var user = iterator.next();
         System.out.println(user.getId());
     }
     ```

  有时候，当循环体内检测到满足/不满足某些条件的时候，想不继续执行后续的代码，或者直接退出，要用到continue和break两个关键字：

  ```java
  int count = 0;
  List<User> onlineUsers = new ArrayList<>();
  
  for(var user : users){
  	if(!user.isOnline()) continue;	//当用户不在线时，跳过
  	if(++count > 10) break; //只挑选前10名在线用户出来，抽奖
  	onlineUsers.add(user);
  }
  ```

  

- 异步（Asynchronous）

- 并行（Parallel）

