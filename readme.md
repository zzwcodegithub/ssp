# 1 数据结构

## 快排

一次partition：选定一个标的X，把所有比X小的数都移到X左边去，把所有比X大的数都移到X右边去。

整体流程：对数组V进行一次partition，得到V(内含V1、X、V2)，其中X已移动到其应该在的位置，再递归地对V1和V2进行partition操作。

```java
public int partion(int[] arr, int left , int right){
    // 不判断 left > right 的原因，因为不可能，起码都是 == 号
    int x = arr[left];
    while(left<right){
        while(left < right && arr[right] >= x) --right;
        arr[left] = arr[right];

        while(left < right && arr[left] <= x) left++;
        arr[right] = arr[left];
    }
    arr[left] = x;
    return left;
}

public void quickSort(int[] arr, int left, int right){
    if(left<right){
        int i = partition(arr, left, right);
		quickSort(arr, left, i - 1);
		quickSort(arr, i + 1, right);
    }
}
```

## 归并排序

```c++
void Merge(vector<int> &a, int low, int mid, int high) {
	//low-mid和mid+1到high都是有序的
	vector<int> b;
	int i = low, j = mid + 1;
	while (i <= mid && j <= high) {
		if (a[i] < a[j]) b.push_back(a[i++]);
		else b.push_back(a[j++]);
	}
	while (i <= mid) a.push_back(b[i++]);
	while (j <= high)a.push_back(b[j++]);
}
void MergeSort(vector<int> &v, int low, int high) {
	if (low < high) {
		int mid = low + (high - low) / 2;
		MergeSort(v, low, mid);
		MergeSort(v, mid + 1, high);
		Merge(v, low, mid, high);
	}
}
```

# 标准模板库

```java
// https://blog.csdn.net/yaomingyang/article/details/78748130 
Map<String, String> map = new HashMap<String, String>();    
  map.put("key1", "value1");    
  map.put("key2", "value2");    
  map.put("key3", "value3");    
      
//第一种：普遍使用，二次取值    
System.out.println("通过Map.keySet遍历key和value：");    
for (String key : map.keySet()) {    
    System.out.println("key= "+ key + " and value= " + map.get(key));    
}    

//第二种    
System.out.println("通过Map.entrySet使用iterator遍历key和value：");    
Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();    
while (it.hasNext()) {    
    Map.Entry<String, String> entry = it.next();    
    System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());    
}    

//第三种：推荐，尤其是容量大时</span>    
System.out.println("通过Map.entrySet遍历key和value");    
for (Map.Entry<String, String> entry : map.entrySet()) {    
    System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());    
}    

//第四种    
System.out.println("通过Map.values()遍历所有的value，但不能遍历key");    
for (String v : map.values()) {    
    System.out.println("value= " + v);    
}
```



# Redis常见面经问题

Redis热点问题：https://github.com/ewenliu/Learning_Note/blob/master/Redis%E7%83%AD%E7%82%B9%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md

aof & rdb 是什么: https://segmentfault.com/a/1190000012908434

布隆过滤器：https://zhuanlan.zhihu.com/p/43263751





# 2 JAVA语言

## 终端命令

```shell
javac Helloworld.java
java Helloworld
```

javac是编译指令，文件名要带上.java后缀。将源码文件编译为字节码（.class文件）。

java是运行指令，**不要**带.class后缀，因为java命令指定是类名。将字节码文件交给jvm开始运行。

如果键入`java Helloworld`，而虚拟机没有找到Helloworld类，就应该检查一下是否有人设置了系统的CLASSPATH环境变量（不推荐）。

## 注释

//：单行注释

/* */：多行注释

/** */：文档注释

## 数据类型

| 类型  | 字节 |
| ----- | ---- |
| int   | 4    |
| short | 2    |
| long  | 8    |
| byte  | 1    |
| char  | 2    |

int最大值大约2*10^9

INT_MAX:0X7FFFFFFF

INT_MIN:0X7FFFFFFF+1 或 0X80000000

JAVA类型范围与机器无关，这是与C++的区别。

JAVA没有无符号数类型。

JAVA中0不能转false，所以不能写while(1)这种东西。

在Java中，**只有基本类型不是对象**，例如，数值、字符和布尔类型的值都不是对象。所有的数组类型，不管是对象数组还是基本类型的数组都扩展了Object类。

## 变量

JAVA中不区分变量的声明和定义。

### final关键字

用final定义常量，习惯上将常量名全部大写。（const是关键字，但是目前并没有使用。）

用final定义的对象变量，只是表示该对象变量不会更改引用，但是这个对象本身是可以改变的。

用final定义的类，不允许被继承。

用final定义的方法，不允许被覆盖。

### 类型转换

```java
double x = 1.2;
int y = (int) x;
```

### 字符串

Java字符串是不可变的。优点：字符串共享。缺点：更改效率低。

用equals方法检测两个字符串是否相等。因为==符号只能确定两个字符串是否放在同一个位置上。

如果要构建一个经常需要改变的字符串，应该使用StringBuilder类。StringBuffer类是线程安全的StringBuilder类。

## 标准输入输出

```java
Scanner in = new Scanner(System.in);
String s = in.nextLine(); //读取一行
String name = in.next(); //读取一个单词（以空白符作为分隔符）
int i = in.nextInt(); //读取一个整形
```

```java
System.out.printf("%8.2f",x); //宽度为8，保留小数点后两位
System.out.printf("Hello, %s, next year you will be %d", name, age); //可以使用多个参数
```

## 文件输入输出

```
Scanner in = new Scanner(Path.get("c:\\myfile.txt"),"UTF-8");
PrintWriter out = new PrintWriter("outfile.txt","UTF-8");
```

## 大数值

`BigInteger`：任意精度的整数

`BigDecimal`：任意精度的浮点数

使用valueOf方法将普通数值转换为大数值:

```java
BigInteger a = BigInteger.valueOf(100);
```

大数值不能使用传统的运算符，而要使用add和multiply方法。

## 一个文件有多个类

一个源代码文件中，**只能有一个public类**，但是可以有任意数目的非共有类。在编译时，会把每一个类编译成一个单独的class文件。

## 内联方法

在C++中，通常在类的外面定义方法，如果在类的内部定义方法，这个方法将自动地成为内联（inline）方法。在Java中，所有的方法都必须在类的内部定义，但并不表示它们是内联方法。是否将某个方法设置为内联方法是Java虚拟机的任务。

## static关键字

### 静态域

即使没有一个instance，静态域也存在，因为它属于类，而不属于任何独立的对象。

### 静态方法

静态方法是一种不能向对象实施操作的方法。

静态方法不使用任何instance，换句话说，没有隐式的参数。可以认为静态方法是没有this参数的方法。

静态方法不能访问实例域，因为它不能操作对象。但是，静态方法可以访问自身类中的静态域。

不需要使用对象调用静态方法。

### 继承

```java
public class Bark {
    public static void main(String[] args) {
        Father father = new Father();
        Father fs = new Son();
        Son son = new Son();
        father.stcSay(); //static father
        father.say(); //father
        fs.stcSay(); //static father
        fs.say(); //son
        son.stcSay(); //static son
        son.say(); //son
    }

}

class Father {
    public static void stcSay() {
        System.out.println("static father");
    }

    public void say() {
        System.out.println("father");
    }
}

class Son extends Father {
    public static void stcSay() {
        System.out.println("static son");
    }

    public void say() {
        System.out.println("Son");
    }
}
```

对于static修饰的变量，当子类与父类中存在相同的static变量时，是根据“静态引用”而不是根据“动态引用”来调用相应的变量的！换句话说，**static是跟着类本身走的，而不是跟着实例走的**。static修饰的内容**只看你的声明是什么，而不管你的实现是什么**。

总结：

子类是不继承父类的static变量和方法的。因为这是属于类本身的。但是子类是可以访问的。 
子类和父类中同名的static变量和方法都是相互独立的，并不存在任何的重写的关系。

### 静态导入

使用静态导入，就可以使用类的静态方法和静态域，而不必加类名前缀。

## 方法参数

**JAVA永远使用值传递！**

## 函数重载

JAVA只有方法重载，没有操作符重载。

函数签名：函数名+参数类型。

重载一个方法，需要函数名相同，参数类型不同。与返回值无关。

## 初始化块

在一个类的声明中，可以包含多个代码块。只要构造类的对象，这些块就会被执行。

无论使用哪个构造器构造对象，域都在对象初始化块中被先初始化。

首先运行初始化块，然后才运行构造器的主体部分。

## 包

一个类可以使用所属包中的所有类，以及其他包中的公有类（public class）。

## 面向对象

### 抽象

抽象是从众多的事物中抽取出共同的、本质性的特征，而舍弃其非本质的特征。

抽象用 abstract 关键字来修饰，用 abstract 修饰类时，此类就不能被实例化，从这里可以看出，抽象类就是为了继承而存在的。

### 封装

隐藏对象的属性和实现细节，仅对外提供公共访问方式，将变化隔离，便于使用，提高复用性和安全性。

### 继承

提高代码复用性；继承是多态的前提。

### 多态

父类或接口定义的引用变量可以指向子类或具体实现类的实例对象。提高了程序的拓展性。

根据何时确定执行多态方法中的哪一个，多态分为两种情况：编译时多态和运行时多态。如果在编译时能够确定执行多态方法中的哪一个，称为编译时多态，否则称为运行时多态。

* 编译时多态

  方法重载都是编译时多态。根据实际参数的数据类型、个数和次序，在编译时能够确定执行重载方法中的哪一个。

* 运行时多态

  方法覆盖表现出两种多态性，当对象引用本类实例时，为编译时多态，否则为运行时多态。

## 继承

### 覆盖

JAVA中所有的继承都是公有继承，没有C++中的私有继承和保护继承。

**子类会继承父类的private域和方法，但是不能访问。**同样，子类也不能覆盖父类的private方法。

```java
class Father{
    private say(){
        System.out.println("I'm father");
    }
    public f(){
        say();
    }
}

class Child extends Father{
    //可以定义。并无覆盖。
    private say(){
        System.out.println("I'm child"); 
    }
    public c(){
        say();
    }
}

public class Test{
    public static void main(String[] args){
        Father father = new Father();
        Child child  = new Child();
        father.f(); //I'm father
        child.f(); //I'm father 表明子类中继承了父类的say方法
        child.c(); //I'm child
    }
}
```

在覆盖一个方法的时候，子类不能低于父类的可见性。

### super关键字

使用super关键字指示编译器调用父类方法。`int a = super.getSalary();`

或者

使用super关键字表示调用父类的构造函数。

### 子类构造函数

如果子类的构造函数没有显式地调用父类的构造函数，则将自动地调用父类默认（没有参数）的构造函数。

如果父类没有不带参数的构造函数，并且在子类的构造函数中又没有显式地调用父类的其他构造函数，则Java编译器将报告错误。

### 多态

一个对象变量可以指向多种实际类型的现象被称为多态。

在运行时能够自动地选择调用哪个方法的现象称为动态绑定。

调用方法的时候，调用哪个方法取决于对象的实际类型。

父类型可以引用子类型，子类型不可以引用父类型。换句话说，不支持类的向下转型。

```java
Son son = new Parent(); //不合法
```

一个instance可以调用哪些方法，是由其声明类型决定的。而其调用的是哪个class的方法，是由其实际类型决定的。`Father f = new Son();`即使f实际是`Son`类的实例，但是只能在其上调用`Father`类定义的方法。但是调用某个方法之后，如果该方法被`Son`类覆盖过，那么实际调用的就是`Son`类的该方法。

## 抽象类

包含抽象方法的类，必须被声明为抽象的。

除了抽象方法之外，抽象类还可以包含具体数据和具体方法。

抽象类不能被实例化。但是可以定义一个抽象类的对象变量，引用非抽象的子类。

## 四个访问修饰符

1. public：对所有类可见。
2. private：仅对本类可见。
3. protected：对本包和所有子类可见。
4. 默认：对本包可见。（不需要修饰符。）

## Object类

### Objects类

**Object** 是 Java 中所有类的基类，位于`java.lang`包。

**Objects** 是 Object 的工具类，位于`java.util`包。它从jdk1.7开始才出现，被final修饰不能被继承，拥有私有的构造函数。它由一些静态的实用方法组成，这些方法是空指针安全的，用于计算对象的hash code、返回对象的字符串表示形式、比较两个对象。

### equals方法

默认的equals方法只检测两个对象的地址是否相等。需要自行覆盖。

注意不要使用instance of关键字。如下例:

```java
//不满足自反性
kid instance of father //true
father instance of kid //false
```

建议使用如下代码代替：

```java
if(getClass() != otherObject.getClass()) return false;
```

常见错误：

```java
public class Employee{
    public boolean equals(Employee other){
        ...
    }
}
```

因为显式参数是Employee，并没有覆盖Object类的equals方法。

改为：

```java
@Override
public class Employee{
    public boolean equals(Object other){
        ...
    }
}
```

### hashCode方法

散列码（hash code）是由对象导出的一个整型值。

**如果要将用户自定义类作为散列表的key值，必须重新定义`equals`方法和`hashCode`方法。**

自定义`hashCode`方法：

```java
public class Employee{
    public int hashCode(){
        return 7*name.hashCode()
            + 11*new Double(salary).hashCode() //使用Double类
            + 13*hireDay.hashCode();
    }
}
```

还可以做的更好一点。首先，最好使用null安全的方法`Objects.hashCode`。如果其参数为null，这个方法会返回0，否则返回对参数调用`hashCode`的结果。另外，使用静态方法`Double.hashCode`来避免创建Double对象：

```java
public class Employee{
    public int hashCode(){
        return 7*Objects.hashCode(name)
            + 11*Double.hashCode(salary)
            + 13*Objects.hashCode(hireDay);
    }
}
```

还有更好的做法，需要组合多个散列值时，可以调用`Objects.hash`并提供多个参数。这个方法会对各个参数调用`Objects.hashCode`，并组合这些散列值：

```
public class Employee{
    public int hashCode(){
        return Objects.hash(name, salary, hireDay);
    }
}
```

`Equals`与`hashCode`的定义必须一致：如果`x.equals(y)`返回true，那么`x.hashCode()`就必须与`y.hashCode()`具有相同的值。

如果存在数组类型的域，那么可以使用静态的`Arrays.hashCode`方法计算散列码。

### toString方法

只要对象与一个字符串通过操作符“+”连接起来，Java编译就会自动地调用`toString`方法，以便获得这个对象的字符串描述。

`println`方法会调用`toString()`，并打印输出得到的字符串。

`Object`类默认的`toString`方法，会打印**对象所属的类名**和**散列码**。

强烈建议为自定义的每一个类增加`toString`方法。这样做不仅自己受益，而且所有使用这个类的程序员也会从这个日志记录支持中受益匪浅。

## 集合类

### ArrayList

可以省去右边的类型参数：

```java
ArrayList<Empolyee> staff = new ArrayList<>();
```

如果调用add且内部数组已经满了，数组列表就将自动地创建一个更大的数组，并将所有的对象从较小的数组中拷贝到较大的数组中。

一旦能够确认数组列表的大小不再发生变化，就可以调用`trimToSize`方法。这个方法将存储区域的大小调整为当前元素数量所需要的存储空间数目。垃圾回收器将回收多余的存储空间。



==总结==

- add()直到满了，然后copy到一个新数组

- 当数组不在变动的时候，trimToSize()压缩多余空间

  

C++向量是值拷贝。如果a和b是两个向量，赋值操作`a=b`将会构造一个与b长度相同的新向量a，并将所有的元素由b拷贝到a，而在Java中，这条赋值语句的操作结果是让a和b引用同一个数组列表。

### ==HashMap==

Java为数据结构中的映射定义了一个接口`java.util.Map`，此接口主要有四个常用的实现类，分别是`HashMap`、`Hashtable`、`LinkedHashMap`和`TreeMap`，类继承关系如下图所示：

![img](https://pic2.zhimg.com/26341ef9fe5caf66ba0b7c40bba264a5_b.png)

* `HashMap`：它根据键的hash code值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。
  *  `HashMap`最多只允许一条记录的键为null，允许多条记录的值为null。
  * `HashMap`非线程安全，即任一时刻可以有多个线程同时写`HashMap`，可能会导致数据的不一致。如果需要满足线程安全，可以用 `Collections`的`synchronizedMap`方法使`HashMap`具有线程安全的能力，或者使用`ConcurrentHashMap`。
* `Hashtable`：`Hashtable`是遗留类，很多映射的常用功能与`HashMap`类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写`Hashtable`，并发性不如`ConcurrentHashMap`，因为`ConcurrentHashMap`引入了分段锁。`Hashtable`不建议在新代码中使用，不需要线程安全的场合可以用`HashMap`替换，需要线程安全的场合可以用`ConcurrentHashMap`替换。
* `LinkedHashMap`：`LinkedHashMap`是`HashMap`的一个子类，==保存了记录的插入顺序==，在用Iterator遍历`LinkedHashMap`时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。
* `TreeMap`：`TreeMap`实现`SortedMap`接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历`TreeMap`时，得到的记录是排过序的。如果使用排序的映射，建议使用`TreeMap`。在使用`TreeMap`时，key必须实现`Comparable`接口或者在构造`TreeMap`传入自定义的`Comparator`，否则会在运行时抛出`java.lang.ClassCastException`类型的异常。

对于上述四种Map类型的类，要求映射中的key是不可变对象。不可变对象是该对象在创建后它的哈希值不会被改变。如果对象的哈希值发生变化，Map对象很可能就定位不到映射的位置了。

从结构实现来讲，`HashMap`是==数组+链表+红黑树==（JDK1.8增加了红黑树部分）实现的，如下如所示。

> 数据：就是hashtale，用链地址法进行冲突处理，如果拉链太长，就用红黑树处理。
>
> 红黑树讲解：https://www.jianshu.com/p/e136ec79235c

![img](https://pic1.zhimg.com/8db4a3bdfb238da1a1c4431d2b6e075c_b.png)

`HashMap`类中有一个非常重要的字段，就是 `Node[] table`，即哈希桶数组，明显它是一个Node的数组。Node是`HashMap`的一个内部类，实现了`Map.Entry`接口，本质是就是一个映射(键值对)。上图中的每个黑色圆点就是一个Node对象。

`HashMap`就是使用哈希表来存储的。哈希表为解决冲突，可以采用开放地址法和链地址法等来解决问题，Java中HashMap采用了==链地址法==。

如果哈希桶数组很大，即使较差的Hash算法也会比较分散，如果哈希桶数组数组很小，即使好的Hash算法也会出现较多碰撞，所以就需要在空间成本和时间成本之间权衡，其实就是在根据实际情况确定哈希桶数组的大小，并在此基础上设计好的hash算法减少Hash碰撞。

Node[] table的初始化长度默认值是16，Load factor为负载因子(默认值是0.75)，threshold是HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

这里存在一个问题，即使负载因子和Hash算法设计的再合理，也免不了会出现拉链过长的情况，一旦出现拉链过长，则会严重影响HashMap的性能。于是，在JDK1.8版本中，对数据结构做了进一步的优化，引入了红黑树。==而当链表长度太长（默认超过8）时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能，其中会用到红黑树的插入、删除、查找等算法==。

#### 扩容机制

当元素个数超过阈值，就要扩容：

- 扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。当然Java里的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组，就像我们用一个小桶装水，如果想装更多的水，就得换大水桶。

这里就是使用一个容量更大的数组来代替已有的容量小的数组，transfer()方法将原有Entry数组的元素拷贝到新的Entry数组里。使用了单链表的头插入方式，同一位置上新元素总会被放在链表的头部位置；这样先放在一个索引上的元素终会被放到Entry链的尾部(如果发生了hash冲突的话），这一点和Jdk1.8有区别，下文详解。在旧数组中同一条Entry链上的元素，通过重新计算索引位置后，有可能被放到了新数组的不同位置上。

1. 扩容是一个特别耗性能的操作，所以当程序员在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的数值，避免map进行频繁的扩容。
2. 负载因子是可以修改的，也可以大于1，但是建议不要轻易修改，除非情况非常特殊。
3. HashMap是线程不安全的，不要在并发的环境中同时操作HashMap，建议使用ConcurrentHashMap。
4. JDK1.8引入红黑树大程度优化了HashMap的性能。



> HashMap的扩容机制：https://blog.csdn.net/lky5387/article/details/103042972
>
> 1. add()元素，当超过thresh时，进行扩容。当然不能thresh不能越界
> 2. transfer()讲原来的Entry的元素，经过计算rehash，计算移入的新的Entry中的位置，并用头插入处理。

### ==[ConcurrentHashMap](https://www.jianshu.com/p/865c813f2726)==

- 在扩容 `transfer`  和 并发线程 `put` 时，可能出现环的问题的问题，所以线程不安全的。



资源：

- 红黑树详解：https://www.jianshu.com/p/e136ec79235c
- HashMap和ConcurrentMap详解：https://blog.csdn.net/weixin_44460333/article/details/86770169



## ==并发==

### 创建线程的方法

1. 实现Runnable接口

```java
Runnable r = ()->{task code;}
Thread t = new Thread(r);
t.start()
```

2. 继承Thread类

```java
class MyThread extends Thread{
    public void run(){
        // task code;
    }
}
MyThread t = new MyThread();
t.start();
```

3. 实现Callable接口

```java
Class MyCallble implements Callble<Integer>{
    public Integer call(){
        // task code;
    }
}
MyCallble mc = new MyCallble();
Thread t = new Thread(mc);
t.start();
```

### Interrupt

当对一个线程调用interrupt方法时，线程的中断状态将被置位。这是每一个线程都具有的boolean标志。

要想弄清中断状态是否被置位，首先调用静态的`Thread.currentThread()`方法获得当前线程，然后调用`isInterrupted()`方法：

```java
while(!Thread.currentThread().isInterrupted && more work){
    do more work;
}
```

如果线程被阻塞，就无法检测中断状态。

当在一个被阻塞的线程上调用`interrupt()`方法时，将会产生`Interrupted Exception`异常。

如果在中断状态被置位时调用`sleep()`方法，它不会休眠。相反，它将清除这一状态并抛出`InterruptedException`异常。

考虑到interrupt方法的线程设计：

```java
Runnable r = ()->{
    try{
        do something;
        while(!Thread.currentThread().isInterrupted() && more work){
            do more work;
        }
    }
    catch(InterruptedException e){
        //处理异常
    }finally{
        //clean up
    }
}
```

对于catch内处理异常的问题，可以在catch子句中调用`Thread.currentThread().interrupt()`来设置中断状态。因为如果在中断状态被置位时调用`sleep()`方法，它将清除这一状态。

或者，更好的选择是，用`throws InterruptedException`标记你的方法，不采用try语句块捕获异常。这样调用者可以捕获这一异常：

```java
void mySubTask() throws InterruptedException{
        do something;
        while(!Thread.currentThread().isInterrupted() && more work){
            do more work;
        }
}
```

### 线程的6种状态

* New

  当用`new`操作符创建一个新线程后，线程处于New状态。

* Runnable

  一旦调用start方法，线程处于runnable状态。一个**可**运行的线程可能正在运行也可能没有运行。

* Blocked

  当一个线程试图获取一个内部的对象锁（而不是可重入锁），而该锁被其他线程持有，则该线程进入阻塞状态。当所有其他线程释放该锁，并且线程调度器允许本线程持有它的时候，该线程将变成非阻塞状态。

* Waiting

  当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。在调用Object.wait方法或Thread.join方法，或者是等待可重入锁或Condition时，就会出现这种情况。

* Timed waiting

  有几个方法有一个超时参数。调用它们导致线程进入计时等待（timed waiting）状态。这一状态将一直保持到超时期满或者接收到适当的通知。带有超时参数的方法有Thread.sleep和Object.wait、Thread.join、Lock.tryLock以及Condition.await的计时版。

* Terminated

### Join方法

* void join（）

  等待终止指定的线程。

* void join（long millis）

  等待指定的线程死亡或者经过指定的毫秒数。

```java
public class JoinTest implements Runnable{
    public void run() {
        do something;
    }
    public static void main(String[] args) throws Exception {  
        Runnable r = new JoinTest();  
        Thread t = new Thread(r);  
        t.start();
        t.join();//主线程会等待t线程结束之后再结束
    }         
}  
```

### 线程优先级

每一个线程有一个优先级，默认继承它的父线程的优先级。

可以用setPriority方法为线程设置优先级。可以将优先级设置为在1与10之间的任何值，默认为5。

每当线程调度器有机会选择新线程时，它首先选择具有较高优先级的线程。但是，线程优先级是高度依赖于系统的。当虚拟机依赖于宿主机平台的线程实现机制时，Java线程的优先级被映射到宿主机平台的优先级上，优先级个数也许更多，也许更少。例如，Windows有7个优先级别。在Oracle为Linux提供的Java虚拟机中，线程的优先级被忽略——所有线程具有相同的优先级。

如果确实要使用优先级，应该避免使低优先级的线程完全饿死。

### 守护线程

可以使用`t.setDaemon(true)`将线程转换为守护线程。

守护线程的唯一用途是为其他线程提供服务。当只剩下守护线程时，虚拟机就退出了，由于如果只剩下守护线程，就没必要继续运行程序了。

守护线程应该永远不去访问固有资源，如文件、数据库，因为它会在任何时候甚至在一个操作的中间发生中断。

### ==可重入锁==

用ReentrantLock保护代码块的基本结构如下：

```java
public class Bank{
    private Lock bankLock = new Reentrantlock();
    public void transfer(){
        bankLock.lock();
        try{
            do something;
        }finally{
            bankLock.unlock();
        }
    }
}
```

**把解锁操作括在finally子句之内是至关重要的。如果在临界区的代码抛出异常，锁必须被释放。否则，其他线程将永远阻塞。**

**线程可以重复地获得已经持有的锁**。锁保持一个持有计数（holdcount）来跟踪对lock方法的嵌套调用。线程在每一次调用lock都要调用unlock来释放锁。由于这一特性，被一个锁保护的代码可以调用另一个使用相同的锁的方法。

线程在调用lock方法来获得另一个线程所持有的锁的时候，很可能发生阻塞。应该更加谨慎地申请锁。tryLock方法试图申请一个锁，在成功获得锁后返回true，否则，立即返回false，而且线程可以立即离开去做其他事情。可以调用tryLock时，使用超时参数。

```java
if(myLock.tryLock(100,TimeUnit.MILLISECONDS))
```

lock方法不能被中断，然而，如果调用带有用超时参数的tryLock，那么如果线程在等待期间被中断，将抛出InterruptedException异常。这是一个非常有用的特性，因为允许程序打破死锁。

也可以调用lockInterruptibly方法。它就相当于一个超时设为无限的tryLock方法。

### 条件对象

线程进入临界区，却发现在某一条件满足之后它才能执行。要使用一个条件对象来管理那些已经获得了一个锁但是却不能做有用工作的线程。这时就要使用条件对象。

等待获得锁的线程和调用await方法的线程存在本质上的不同。一旦一个线程调用await方法，它进入该条件的等待集。即使锁可用时，该线程也不能马上解除阻塞，要等到另一个线程调用同一条件上的signalAll方法。

```java
class Bank{
    private Lock bankLock = new Reentrantlock();
    private Condition sufficientFunds = bankLock.newCondition();
    public void transfer(){
        bankLock.lock();
        try{
            while(accounts[from]<amount) //注意不能使用if
                sufficientFunds.await();
            转移资金
            sufficientFunds.signalAll();
        }
    }
}

```

`singalAll()`重新激活因为这一条件而等待的所有线程。当这些线程从等待集当中移出时，它们将试图重新进入该对象。一旦锁成为可用的，它们中的某个将从await调用返回，重新获得该锁并从被阻塞的地方继续执行。此时，线程应该再次测试该条件。由于无法确保该条件被满足——`signalAll`方法仅仅是通知正在等待的线程此时有可能已经满足条件。

另一个方法signal，则是随机解除等待集中某个线程的阻塞状态。这比解除所有线程的阻塞更加有效，但也存在危险。如果随机选择的线程发现自己仍然不能运行，那么它再次被阻塞。如果没有其他线程再次调用signal，那么系统就死锁了。

await()方法也可以提供一个超时参数。

### synchronized

Java中的每一个对象都有一个内部锁。如果一个方法用synchronized关键字声明，那么对象的锁将保护整个方法。

内部对象锁只有一个相关条件。wait方法添加一个线程到等待集中，notifyAll/notify方法解除等待线程的阻塞状态。

将静态方法声明为synchronized也是合法的。如果调用这种方法，该方法获得相关的类对象的内部锁。

内部锁和条件存在一些局限：

* 不能中断一个正在试图获得锁的线程。
* 试图获得锁时不能设定超时。
* 每个锁仅有单一的条件，可能是不够的。

### 同步阻塞

```java
synchronized(obj){
    //获得了obj对象的锁，称为同步阻塞
}
```

有时程序员使用一个对象的锁来实现额外的原子操作，实际上称为客户端锁定（client-side locking）：

```java
public void transfer(Vector<Double> accounts){
    synchronized(accounts){
        accounts.set(from,1);
        accounts.set(to,2);
    }
}
```

通过以上方法保证两个set的原子性。但是它完全依赖于这样一个事实：Vector类对自己的所有可修改方法都使用内部锁。然而，这是真的吗？Vector类的文档没有给出这样的承诺。客户端锁定是非常脆弱的，通常不推荐使用。

### volatile

如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。假设对共享变量除了赋值之外并不完成其他操作，那么可以将这些共享变量声明为volatile。



sun： 

为了获得最佳速度，允许线程保存共享成员变量的私有拷贝，而且只当线程进入或者离开同步代码块时才与共享成员变量的原始值对比。这样当多个线程同时与某个对象交互时，就必须要注意到要让线程及时的得到共享成员变量的变化。
而volatile关键字就是提示VM：对于这个成员变量不能保存它的私有拷贝，而应直接与共享成员变量交互

### final与并发

将一个变量声明为final时，其他线程会在构造函数完成对该对象的构造之后才看到这个变量。如果不使用final，其它线程可能只是看到null。当然，对这个变量的操作并不是线程安全的。

### 读/写锁

`java.util.concurrent.locks`包定义了两个锁类，`ReentrantLock`类和`ReentrantReadWriteLock`类。如果很多线程从一个数据结构读取数据而很少线程修改其中数据的话，读写锁是十分有用的。

```java
private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
private Lock readLock = rwl.readLock(); //抽取读锁
private Lock writeLock = rwl.writeLock(); //抽取写锁
//对所有获取方法加读锁
public void getSomething(){
    readLock.lock();
    try{...}
    finally{readLock.unlock();}
}
//对所有修改方法加写锁
public void transfer(){
    writeLock.lock();
    try{...}
    finally{writeLock.unlock();}
}
```

读锁可以被多个读操作共用，但会排斥所有写操作。写锁，排斥所有其他的读操作和写操作。

### 阻塞队列

生产者线程向队列插入元素，消费者线程则取出它们。使用队列，可以安全地从一个线程向另一个线程传递数据。例如，考虑银行转账程序，转账线程将转账指令对象插入一个队列中，而不是直接访问银行对象。另一个线程从队列中取出指令执行转账。只有该线程可以访问该银行对象的内部。因此不需要同步。

当试图向队列添加元素而队列已满，或是想从队列移出元素而队列为空的时候，阻塞队列会将线程阻塞。

java.util.concurrent包提供了阻塞队列的几个变种：

* LinkedBlockingQueue：默认的容量是没有上边界的，但是，也可以选择指定最大容量。

* LinkedBlockingDeque：是一个双端的版本。

* ArrayBlockingQueue：在构造时需要指定容量，并且有一个可选的参数来指定是否需要公平性。

* PriorityBlockingQueue：是一个带优先级的队列，而不是先进先出队列。元素按照它们的优先级顺序被移出。

* DelayQueue：包含实现Delayed接口的对象。

  ```java
  //getDelay方法返回对象的残留延迟。负值表示延迟已经结束。元素只有在延迟用完的情况下才能从DelayQueue移除。
  //还必须实现compareTo方法。DelayQueue使用该方法对元素进行排序。
  interface Delayed extends Comparable<Delayed>{
      long getDelay(TimeUnit unit);
  }
  ```

### 线程安全的集合

java.util.concurrent包提供了映射、有序集和队列的高效实现：ConcurrentHashMap、ConcurrentSkipListMap、ConcurrentSkipListSet和ConcurrentLinkedQueue。

集合返回弱一致性（weakly consistent）的迭代器。这意味着迭代器不一定能反映出它们被构造之后的所有的修改，但是，它们不会将同一个值返回两次，也不会抛出Concurrent ModificationException异常。

### Callable与Future

Callable 和 Future接口的区别

1. Callable规定的方法是call()，而Runnable规定的方法是run()。
2. Callable的任务执行后可返回值，而Runnable的任务是不能返回值的。
3. call()方法可抛出异常，而run()方法是不能抛出异常的。
4. 运行Callable任务可拿到一个Future对象， Future表示异步计算的结果。
5. 它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。
6. 通过Future对象可了解任务执行情况，可取消任务的执行，还可获取任务执行的结果。
7. Callable是类似于Runnable的接口，实现Callable接口的类和实现Runnable的类都是可被其它线程执行的任务。

FutureTask包装器是一种非常便利的机制，可将Callable转换成Future和Runnable，它同时实现二者的接口。

```java
Callable<Integer> myComputation = ...;
FutureTask<Integer> task = new FutureTask<Integer>(myComputation);
Thread t = new Thread(task); //task既是Runnable
t.start();
Integer result = task.get(); //task也是Future
```

### 线程池

构建一个新的线程是有一定代价的，因为涉及与操作系统的交互。如果程序中创建了大量的生命期很短的线程，应该使用线程池（thread pool）。

一个线程池中包含许多准备运行的空闲线程。将Runnable对象交给线程池，就会有一个线程调用run方法。当run方法退出时，线程不会死亡，而是在池中准备为下一个请求提供服务。

另一个使用线程池的理由是减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。如果有一个会创建许多线程的算法，应该使用一个线程数“固定的”线程池以限制并发线程的总数。

执行器（Executor）类有许多静态工厂方法用来构建线程池。

* newCachedThreadPool：构建了一个线程池，对于每个任务，如果有空闲线程可用，立即让它执行任务，如果没有可用的空闲线程，则创建一个新线程。
* newFixedThreadPool：构建一个具有固定大小的线程池。如果提交的任务数多于空闲的线程数，那么把得不到服务的任务放置到队列中。当其他任务完成以后再运行它们。
* newSingleThreadExecutor：是一个退化了的大小为1的线程池：由一个线程执行提交的任务，一个接着一个。
* newScheduledThreadPool：
* newSingleThreadScheduledExecutor：

前3个方法返回实现了ExecutorService接口的ThreadPoolExecutor类的对象。

可用下面的submit方法之一将一个Runnable对象或Callable对象提交给ExecutorService，调用submit时，会得到一个Future对象，可用来查询该任务的状态。

```java
Future<?> submit(Runnable task)
Future<T> submit(Runnable task, T result)
Future<T> submit(Callable<T> task)
```

* `Future<?> submit(Runnable task)`

  这个submit方法返回一个奇怪样子的`Future<?>`。可以使用这样一个对象来调用isDone、cancel或isCancelled。但是，get方法在完成的时候只是简单地返回null。

* `Future<T> submit(Runnable task, T result)`

  这个版本的Submit也提交一个Runnable，并且Future的get方法在完成的时候返回指定的result对象。

* `Future<T> submit(Callable<T> task)`

  这个版本的Submit提交一个Callable，并且返回的Future对象将在计算结果准备好的时候得到它。

当用完一个线程池的时候，调用shutdown。该方法启动该池的关闭序列。被关闭的执行器不再接受新的任务。当所有任务都完成以后，线程池中的线程死亡。另一种方法是调用shutdownNow。该池取消尚未开始的所有任务并试图中断正在运行的线程。

后两个方法将返回实现了Scheduled-ExecutorService接口的对象。ScheduledExecutorService接口具有为预定执行或重复执行任务而设计的方法。

可以预定Runnable或Callable在初始的延迟之后只运行一次。也可以预定一个Runnable对象周期性地运行。

#### 重要参数

> 现有一个线程池，参数corePoolSize = 5，maximumPoolSize = 10，BlockingQueue阻塞队列长度为5，此时有4个任务同时进来，问：线程池会创建几条线程？
> 如果4个任务还没处理完，这时又同时进来2个任务，问：线程池又会创建几条线程还是不会创建？
> 如果前面6个任务还是没有处理完，这时又同时进来5个任务，问：线程池又会创建几条线程还是不会创建？

如果你此时一脸懵逼，请不要慌，问题不大。

创建线程池的构造方法的参数都有哪些？

要回答这个问题，我们需要从创建线程池的参数去找答案：

```java
java.util.concurrent.ThreadPoolExecutor#ThreadPoolExecutor：
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler) {
	if (corePoolSize < 0 || maximumPoolSize <= 0 || maximumPoolSize < corePoolSize || keepAliveTime < 0)
	        throw new IllegalArgumentException();
	if (workQueue == null || threadFactory == null || handler == null)
	        throw new NullPointerException();
	this.acc = System.getSecurityManager() == null ? null : AccessController.getContext();
	this.corePoolSize = corePoolSize;
	this.maximumPoolSize = maximumPoolSize;
	this.workQueue = workQueue;
	this.keepAliveTime = unit.toNanos(keepAliveTime);
	this.threadFactory = threadFactory;
	this.handler = handler;
}
```

创建线程池一共有7个参数，从源码可知，corePoolSize和maximumPoolSize都不能小于0，且核心线程数不能大于最大线程数。

下面我来解释一下这7个参数的用途：

- **corePoolSize**：线程池核心线程数量，**核心线程不会被回收，即使没有任务执行，也会保持空闲状态**。如果线程池中的线程少于此数目，则在执行任务时创建。
- **maximumPoolSize**：池允许最大的线程数，**当线程数量达到corePoolSize，且workQueue队列塞满任务了之后，继续创建线程**。
- **keepAliveTime**：超过corePoolSize之后的“临时线程”的存活时间。
- **unit**：keepAliveTime的单位。
- **workQueue**：**当前线程数超过corePoolSize时，新的任务会处在等待状态，并存在workQueue中**，BlockingQueue是一个先进先出的阻塞式队列实现，底层实现会涉及Java并发的AQS机制。
- **threadFactory**：创建线程的工厂类，通常我们会自定一个threadFactory设置线程的名称，这样我们就可以知道线程是由哪个工厂类创建的，可以快速定位。
- **handler**：线程池执行拒绝策略，当线数量达到maximumPoolSize大小，并且workQueue也已经塞满了任务的情况下，线程池会调用handler拒绝策略来处理请求。

系统默认的拒绝策略有以下几种：

- AbortPolicy：为线程池**默认**的拒绝策略，该策略直接抛异常处理。
- DiscardPolicy：直接抛弃不处理。
- DiscardOldestPolicy：丢弃队列中最老的任务。
- CallerRunsPolicy：将任务分配给当前执行execute方法线程来处理。

我们还可以自定义拒绝策略，只需要实现RejectedExecutionHandler接口即可，友好的拒绝策略实现有如下：

- 将数据保存到数据，待系统空闲时再进行处理
- 将数据用日志进行记录，后由人工处理

现在我们回到刚开始的问题就很好回答了：

> 线程池corePoolSize=5，线程初始化时不会自动创建线程，所以当有4个任务同时进来时，执行execute方法会新建【4】条线程来执行任务；
> 前面的4个任务都没完成，现在又进来2个队列，会新建【1】条线程来执行任务，这时poolSize=corePoolSize，还剩下1个任务，线程池会将剩下这个任务塞进阻塞队列中，等待空闲线程执行；
> 如果前面6个任务还是没有处理完，这时又同时进来了5个任务，此时还没有空闲线程来执行新来的任务，所以线程池继续将这5个任务塞进阻塞队列，但发现阻塞队列已经满了，核心线程也用完了，还剩下1个任务不知道如何是好，于是线程池只能创建【1】条“临时”线程来执行这个任务了；
> 这里创建的线程用“临时”来描述还是因为它们不会长期存在于线程池，它们的存活时间为keepAliveTime，此后线程池会维持最少corePoolSize数量的线程。

<img src="https://pic4.zhimg.com/v2-971a30193d9749df13b0811feb88fb47_b.jpg" alt="img" style="zoom:80%;" />

#### 为什么不建议使用Executors创建线程池？

JDK为我们提供了Executors线程池工具类，里面有默认的线程池创建策略，大概有以下几种：

- **FixedThreadPool**：线程池线程数量固定，即corePoolSize和maximumPoolSize数量一样。
- **SingleThreadPool**：单个线程的线程池。
- **CachedThreadPool**：初始核心线程数量为0，最大线程数量为Integer.MAX_VALUE，线程空闲时存活时间为60秒，并且它的阻塞队列为SynchronousQueue，它的初始长度为0，这会导致任务每次进来都会创建线程来执行，在线程空闲时，存活时间到了又会释放线程资源。
- **ScheduledThreadPool**：创建一个定长的线程池，而且支持定时的以及周期性的任务执行，类似于Timer。

用Executors工具类虽然很方便，我依然不推荐大家使用以上默认的线程池创建策略，阿里巴巴开发手册也是强制不允许使用Executors来创建线程池，我们从JDK源码中寻找一波答案：

```java
java.util.concurrent.Executors：
// FixedThreadPool
public static ExecutorService newFixedThreadPool(int nThreads) {
	return new ThreadPoolExecutor(nThreads, nThreads,
	                                  0L, TimeUnit.MILLISECONDS,
	                                  new LinkedBlockingQueue<Runnable>());
}
// SingleThreadPool
public static ExecutorService newSingleThreadExecutor() {
	return new FinalizableDelegatedExecutorService
	        (new ThreadPoolExecutor(1, 1,
	                                0L, TimeUnit.MILLISECONDS,
	                                new LinkedBlockingQueue<Runnable>()));
}
// CachedThreadPool
public static ExecutorService newCachedThreadPool() {
	// 允许创建线程数为Integer.MAX_VALUE
	return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
	                                  60L, TimeUnit.SECONDS,
	                                  new SynchronousQueue<Runnable>());
}
// ScheduledThreadPool
public ScheduledThreadPoolExecutor(int corePoolSize) {
	// 允许创建线程数为Integer.MAX_VALUE
	super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
	              new DelayedWorkQueue());
}
public LinkedBlockingQueue() {
	// 允许队列长度最大为Integer.MAX_VALUE
	this(Integer.MAX_VALUE);
}
```

从JDK源码可看出，Executors工具类无非是把一些特定参数进行了封装，并提供一些方法供我们调用而已，我们并不能灵活地填写参数，策略过于简单，不够友好。

CachedThreadPool和ScheduledThreadPool最大线程数为Integer.MAX_VALUE，如果线程无限地创建，会造成OOM异常。

LinkedBlockingQueue基于链表的FIFO队列，是无界的，默认大小是Integer.MAX_VALUE，因此FixedThreadPool和SingleThreadPool的阻塞队列长度为Integer.MAX_VALUE，如果此时队列被无限地堆积任务，会造成OOM异常。

## 乐观锁和悲观锁

两种锁各有优缺点，不可认为一种好于另一种。

**乐观锁适用于写比较少的情况下（多读场景）**，即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果是多写的情况，一般会经常产生冲突，这就会导致上层应用会不断的进行retry，这样反倒是降低了性能，所以**一般多写的场景下用悲观锁就比较合适。**

### 悲观锁

总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**）。

传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。

Java中`synchronized`和`ReentrantLock`等独占锁就是悲观锁思想的实现。

### 乐观锁

总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。

**乐观锁适用于多读的应用类型，这样可以提高吞吐量**，像数据库提供的类似于**write_condition机制**，其实都是提供的乐观锁。

在Java中`java.util.concurrent.atomic`包下面的原子变量类就是使用了乐观锁的一种实现方式**CAS**实现的。

缺点：

1. ABA 问题

   如果一个变量V初次读取的时候是A值，并且在准备赋值的时候检查到它仍然是A值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回A，那CAS操作就会误认为它从来没有被修改过。这个问题被称为CAS操作的 **"ABA"问题。**

   JDK 1.5 以后的 `AtomicStampedReference 类`就提供了此种能力，其中的 `compareAndSet 方法`就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

2. 循环时间长开销大

   自旋CAS（也就是不成功就一直循环执行直到成功）如果长时间不成功，会给CPU带来非常大的执行开销。

3. 只能保证一个共享变量的原子操作

   CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5开始，提供了`AtomicReference类`来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用`AtomicReference类`把多个共享变量合并成一个共享变量来操作。

### 乐观锁常见的两种实现方式

乐观锁一般会使用版本号机制或CAS算法实现

#### 版本号机制

一般是在数据表中加上一个数据版本号version字段，表示数据被修改的次数，当数据被修改时，version值会加一。当线程A要更新数据值时，在读取数据的同时也会读取version值，在提交更新时，若刚才读取到的version值为当前数据库中的version值相等时才更新，否则重试更新操作，直到更新成功。

#### CAS算法

即**compare and swap（比较与交换）**，是一种有名的**无锁算法**。

无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步。**CAS算法**涉及到三个操作数

- 需要读写的内存值 V
- 进行比较的值 A
- 拟写入的新值 B

当且仅当 V 的值等于 A时，CAS通过原子方式用新值B来更新V的值，否则不会执行任何操作（比较和替换是一个原子操作）。一般情况下是一个**自旋操作**，即**不断的重试**。

## 异常

在Java程序设计语言中，异常对象都是派生于Throwable类的一个实例。如果Java中内置的异常类不能够满足需求，用户可以创建自己的异常类。

![img](https://img-blog.csdnimg.cn/20190612234942627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phdmFfY3hycw==,size_16,color_FFFFFF,t_70)

需要注意的是，所有的异常都是由Throwable继承而来，但在下一层立即分解为两个分支：Error和Exception。

Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误。应用程序不应该抛出这种类型的对象。如果出现了这样的内部错误，除了通告给用户，并尽力使程序安全地终止之外，再也无能为力了。这种情况很少出现。

在设计Java程序时，需要关注Exception层次结构。这个层次结构又分解为两个分支：一个分支派生于RuntimeException；另一个分支包含其他异常。划分两个分支的规则是：由程序错误导致的异常属于RuntimeException；而程序本身没有问题，但由于像I/O错误这类问题导致的异常属于其他异常。

Java语言规范将派生于Error类或RuntimeException类的所有异常称为非受查（unchecked）异常，所有其他的异常称为受查（checked）异常。编译器将核查是否为所有的受查异常提供了异常处理器。

注释：如果熟悉标准C++类库中的异常层次结构，就一定会感到有些困惑。C++有两个基本的异常类，一个是runtime_error；另一个是logic_error。logic_error类相当于Java中的RuntimeException，它表示程序中的逻辑错误；runtime_error类是所有由于不可预测的原因所引发的异常的基类。它相当于Java中的非RuntimeException异常。

如果遇到了无法处理的情况，那么Java的方法可以抛出一个异常。例如，一段读取文件的代码知道有可能读取的文件不存在，或者内容为空，因此，试图处理文件信息的代码就需要通知编译器可能会抛出IOException类的异常。方法应该在其首部声明所有可能抛出的异常。这样可以从首部反映出这个方法可能抛出哪类受查异常。

在以下两种情况的时候，必须在方法中用throws子句声明异常：

1. 调用一个抛出受查异常的方法，例如，FileInputStream构造器。
2. 程序运行过程中发现错误，并且利用throw语句抛出一个受查异常。

但是，不需要声明Java的内部错误，即从Error继承的错误。任何程序代码都具有抛出那些异常的潜能，而我们对其没有任何控制能力。同样，也不应该声明从RuntimeException继承的那些非受查异常。

**除了声明异常之外，还可以捕获异常。这样会使异常不被抛到方法之外，也不需要throws规范。**

警告：如果在子类中覆盖了超类的一个方法，子类方法中声明的受查异常不能比超类方法中声明的异常更通用（也就是说，子类方法中可以抛出更特定的异常，或者根本不抛出任何异常）。特别需要说明的是，如果超类方法没有抛出任何受查异常，子类也不能抛出任何受查异常。

# 3 设计模式

## 单例模式

> 确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

```java
//是线程安全的。也被称为饿汉式单例。
public class Singleton{
    private static final Singleton singleton = new Singleton();
    private Singleton(){} //需要{}
    //注意是static方法
    public static Singleton getSingleton(){
        return singleton;
    }
    //类中其它方法尽量是static的
    public static void doSomething(){}
}
```

```java
//也是线程安全的。也被称为懒汉式单例。
public class Singleton{
    private static Singleton singleton = null;
    private Singleton(){} 
    //注意是增加的synchronized关键字
    synchronized public static Singleton getSingleton(){
        if(singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }
}
```

```java
//双重检查加锁
public class Singleton{
    private static violate Singleton instance = null;
    private Singleton(){}
    public static Singleton getInstance(){
        if(instance == null){
            synchronized(Singleton.class){
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

* **饿汉式单例**会消耗更多系统资源，因为即使没有创建一个实例也会在方法区的静态变量表里分配内存。

* **懒汉式单例**每次获取实例都需要进入同步，对性能影响较大。

* **双重检查加锁**既线程安全，又能够使性能不受到大的影响。

所谓双重检查加锁机制，指的是：并不是每次进入getInstance方法都需要同步，而是先不同步，进入方法过后，先检查实例是否存在，如果不存在才进入下面的同步块，这是第一重检查。进入同步块过后，再次检查实例是否存在，如果不存在，就在同步的情况下创建一个实例，这是第二重检查。这样一来，就只需要同步一次了，从而减少了多次在同步情况下进行判断所浪费的时间。

双重检查加锁机制的实现会使用一个关键字volatile，它的意思是：被volatile修饰的变量的值，将不会被本地线程缓存，编译器也不会进行指令重排。懒汉单例的final也能达到这样的效果，但是由于此处的变量需要后续进行更改故使用volatile关键字。

这种实现方式既可使实现线程安全的创建实例，又不会对性能造成太大的影响，**它只是在第一次创建实例的时候同步，以后就不需要同步了，从而加快运行速度**。

### 优点

* 减少内存开支。
* 降低系统开销。

### 缺点

* 一般没有接口，扩展很困难。因为接口对单例模式是没有任何意义的，它要求“自行实例化”，接口或抽象类是不可能被实例化的。
* 单例模式对测试是不利的。在并行开发环境中，如果单例模式没有完成，是不能进行测试的，没有接口也不能使用mock的方式虚拟一个对象。

### 使用场景

* 在整个项目中需要一个共享访问点或共享数据。
* 创建一个对象需要消耗的资源过多，如要访问IO和数据库等资源。

## 工厂模式

> 定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。

**抽象产品类**

```java
public abstract class Product{
    public abstract void doSomething();
}
```

**具体产品类**

```java
public class ConcreteProduct1 extends Product{
    public void doSomething(){}
}

public class ConcreteProduct2 extends Product{
    public void doSomething(){}
}
```

**抽象工厂类**

```java
public abstract class Creatror{
    public abstract <T extends Product> T createProduct(Class<T> c);
}
```

**具体工厂类**

```java
public ConcreteCreator extends Creator{
    public <T extends Product> T createProduct(Class<T> c){
        Product product = null;
        try{
            product = (Product)Class.forName(c.getName()).newInstance();
        }catch(Exception e){
            //异常处理
        }
        return (T)product;
    }
}
```

**场景类**

```java
public class client{
    public static void main(String[] args){
        Creator creator = new ConcreteCreator();
        Product product = creator.createProduct(ConcreteProduct.class);
    }
}
```

### 优点

* 对象创建是有条件约束的，如一个调用者需要一个具体的产品对象，只要知道这个产品的类名（或约束字符串）就可以了，不用知道创建对象的艰辛过程，降低模块间的耦合。
* 扩展性非常优秀。在增加产品类的情况下，只要适当地修改具体的工厂类或扩展一个工厂类，就可以完成“拥抱变化”。
* 屏蔽产品类。这一特点非常重要，产品类的实现如何变化，调用者都不需要关心，它只需要关心产品的接口，只要接口保持不变，系统中的上层模块就不要发生变化。

## 代理模式

> 为其他对象提供一种代理以控制对这个对象的访问。

**抽象主题类**

```java
public interface Subject{
    public void doSomething();
}
```

**真实主题类**

```java
public class RealSubject implements Subject{
    public void doSomething(){}
}
```

**代理类**

```java
public class Proxy implements Subject{
    private Subject subject = null;
    public Proxy(){
        this.subject = new Proxy();
    }
    public Proxy(Subject _subject){
        this.subject = _subject;
    }
    public void doSomething(){
        this.before();
        this.subject.doSomething();
        this.after();
    }
    public void before(){}
    public void after(){}
}
```

# 4.操作系统

## ==进程间通信==

### 匿名管道

调用pipe函数时在内核中开辟一块缓冲区用于通信,它有一个读端，一个写端。管道在用户程序看起来就像一个打开的文件,通过read或者write向这个文件读写数据，其实是在读写内核缓冲区。

**匿名管道的特点：**

1.两个进程通过一个管道只能实现单向通信，如果想双向通信必须再重新创建一个管道。

2.只能用于具有亲缘关系的进程间通信，例如父子，兄弟进程。

### 命名管道(FIFO)

上一种进程间通信的方式是匿名的，所以只能用于具有亲缘关系的进程间通信，命名管道的出现正好解决了这个问题。FIFO不同于管道之处在于它提供一个路径名与之关联，以FIFO的文件形式存储文件系统中。命名管道是一个设备文件，因此即使进程与创建FIFO的进程不存在亲缘关系，只要可以访问该路径，就能够通过FIFO相互通信。

**命名管道的特点：**

1.命名管道是一个存在于硬盘上的文件，而管道是存在于内存中的特殊文件。

2.命名管道可以用于任何两个进程之间的通信，不管这两个进程是不是父子进程，也不管这两个进程之间有没有关系。

### 消息队列

消息队列提供了一种从一个进程向另一个进程发送一个数据块的方法。

消息队列可以是一种消息链表。有足够的权限的线程可以往队列中放置消息，有足够读权限的线程可以从队列中取走消息。每个消息都是一个记录，它由发送者赋予一个优先级。在某个进程往一个队列写入消息之前，并不需要另外某个进程在该队列上等待消息的到达。这根管道和FIFO是相反的，对于后者来说，除非读出者已经存在，否则现有写入者是没有意义的。

**消息队列和管道的对比**

1.匿名管道是跟随进程的，消息队列是跟随内核的，也就是说进程结束之后，匿名管道就死了，但是消息队列还会存在（除非显示调用函数销毁）。

2.管道是文件，存放在磁盘上，访问速度慢，消息队列是数据结构，存放在内存，访问速度快。

3.管道是数据流式存取，消息队列是数据块式存取。

### 信号量

信号量的本质是一种数据操作锁，用来负责数据操作过程中的互斥，同步等功能。

信号量是用来管理临界资源的。它本身只是一种外部资源的标识，不具有数据交换功能，而是通过控制其他的通信资源实现进程间通信。 使用PV操作。

### 共享内存

共享内存就是分配一块能被其他进程访问的内存。

在使用共享内存区前，必须通过系统函数将其附加到进程的地址空间或说映射到进程空间。两个不同进程A、B共享内存的意思是，同一块物理内存被映射到 进程A、B各自的进程地址空间。进程A可以即时看到进程B对共享内存中数据的更新，反之亦然。由于多个进程共享同一块内存区域，必然需要某种同步机制，互斥锁和信号量都可以。

**共享内存的特点：**

共享内存是这进程间通信方式中效率最高的。但是因为共享内存没有提供相应的互斥机制，所以一般共享内存都和信号量配合起来使用。

消息队列，FIFO，管道的消息传递方式一般为 ：

1. 服务器获取输入的信息；
2. 通过管道，消息队列等写入数据至内存中，通常需要将该数据拷贝到内核中；
3. 客户从内核中将数据拷贝到自己的客户端进程中；
4. 然后再从进程中拷贝到输出文件；

上述过程通常要经过4次拷贝，才能完成文件的传递。

而共享内存只需要：

1. 输入内容到共享内存区
2. 从共享内存输出到文件

上述过程不涉及到内核的拷贝，这些进程间数据的传递就不再通过执行任何进入内核的系统调用来传递彼此的数据，节省了时间，所以共享内存是这五种进程间通信方式中效率最高的。

### 套接字

socket

## ==死锁==

### 四个必要条件

1. 互斥。在出现死锁的系统中，必须存在需要互斥使用的资源。
2. 占有等待。在出现死锁的系统中，一定有已分配到了某些资源且在等待另外资源的进程。
3. 非剥夺。在出现死锁的系统中，一定有不可剥夺使用的资源。
4. 循环等待。

## ==IO==

用户程序进行IO的读写，依赖于底层的IO读写，基本上会用到底层的read&write两大系统调用。在不同的操作系统中，IO读写的系统调用的名称可能不完全一样，但是基本功能是一样的。

这里涉及一个基础的知识：read系统调用，并不是直接从物理设备把数据读取到内存中；write系统调用，也不是直接把数据写入到物理设备。上层应用无论是调用操作系统的read，还是调用操作系统的write，都会涉及缓冲区。

具体来说，调用操作系统的read，是把数据**从内核缓冲区复制到进程缓冲区**；而write系统调用，是把数据**从进程缓冲区复制到内核缓冲区**。也就是说，**上层程序的IO操作，实际上不是物理设备级别的读写，而是缓存的复制**。read&write两大系统调用，都不负责数据在内核缓冲区和物理设备（如磁盘）之间的交换，这项底层的读写交换，是由操作系统内核（Kernel）来完成的。

### 同步和阻塞

阻塞IO，指的是**需要内核IO操作彻底完成后，才返回到用户空间执行用户的操作**。阻塞指的是用户空间程序的执行状态。传统的IO模型都是同步阻塞IO。在Java中，默认创建的socket都是阻塞的。

同步IO，是一种用户空间与内核空间的IO发起方式。同步IO是指**用户空间的线程是主动发起IO请求的一方，内核空间是被动接受方**。异步IO则反过来，是指系统内核是主动发起IO请求的一方，用户空间的线程是被动接受方。

### 同步阻塞IO（Blocking IO）

在Java应用程序进程中，默认情况下，所有的socket连接的IO操作都是同步阻塞IO。

在阻塞式IO模型中，Java应用程序从IO系统调用开始，直到系统调用返回，在这段时间内，Java进程是阻塞的。返回成功后，应用进程开始处理用户空间的缓存区数据。

举个例子，在Java中发起一个socket的read读操作的系统调用，流程大致如下：

1. 从Java启动IO读的read系统调用开始，用户线程就进入阻塞状态。
2. 当系统内核收到read系统调用，就开始准备数据。一开始，数据可能还没有到达内核缓冲区（例如，还没有收到一个完整的socket数据包），这个时候内核就要等待。
3. 内核一直等到完整的数据到达，就会将数据从内核缓冲区复制到用户缓冲区（用户空间的内存），然后内核返回结果（例如返回复制到用户缓冲区中的字节数）。
4. 直到内核返回后，用户线程才会解除阻塞的状态，重新运行起来。

总之，阻塞IO的特点是：在内核进行IO执行的两个阶段，用户线程都被阻塞了。

阻塞IO的优点是：

* 应用的程序开发非常简单。
* 在阻塞期间，用户线程基本不会占用CPU资源。

阻塞IO的缺点是：一般情况下，会为每个连接配备一个独立的线程；反过来说，就是一个线程维护一个连接的IO操作。在并发量小的情况下，这样做没有什么问题。但是，当在高并发的应用场景下，需要大量的线程来维护大量的网络连接，内存、线程切换开销会非常巨大。因此，基本上阻塞IO模型在高并发应用场景下是不可用的。

### 同步非阻塞IO（Non-blocking IO）

强调一下，这里所说的NIO（同步非阻塞IO）模型，并非Java的NIO（New IO）库。

在NIO模型中，应用程序一旦开始IO系统调用，会出现以下两种情况：

1. 在内核缓冲区中没有数据的情况下，系统调用会立即返回，返回一个调用失败的信息。
2. 在内核缓冲区中有数据的情况下，是阻塞的，直到数据从内核缓冲复制到用户进程缓冲。复制完成后，系统调用返回成功，应用进程开始处理用户空间的缓存数据。

举个例子。发起一个非阻塞socket的read读操作的系统调用，流程如下：

1. 在内核数据没有准备好的阶段，用户线程发起IO请求时，立即返回。所以，为了读取到最终的数据，用户线程需要不断地发起IO系统调用。
2. 内核数据到达后，用户线程发起系统调用，用户线程阻塞。内核开始复制数据，它会将数据从内核缓冲区复制到用户缓冲区（用户空间的内存），然后内核返回结果（例如返回复制到的用户缓冲区的字节数）。
3. 用户线程读到数据后，才会解除阻塞状态，重新运行起来。也就是说，用户进程需要经过多次的尝试，才能保证最终真正读到数据，而后继续执行。

同步非阻塞IO的特点：应用程序的线程需要不断地进行IO系统调用，轮询数据是否已经准备好，如果没有准备好，就继续轮询，直到完成IO系统调用为止。

同步非阻塞IO的优点：每次发起的IO系统调用，在内核等待数据过程中可以立即返回。用户线程不会阻塞，实时性较好。

同步非阻塞IO的缺点：不断地轮询内核，这将占用大量的CPU时间，效率低下。

总体来说，在高并发应用场景下，同步非阻塞IO也是不可用的。一般Web服务器不使用这种IO模型。这种IO模型一般很少直接使用，而是在其他IO模型中使用非阻塞IO这一特性。在Java的实际开发中，也不会涉及这种IO模型。

### IO多路复用（IO Multiplexing）(NIO)

即经典的Reactor反应器设计模式，有时也称为异步阻塞IO, Java中的Selector选择器和Linux中的epoll都是这种模型。

如何避免同步非阻塞IO模型中轮询等待的问题呢？这就是IO多路复用模型。

在IO多路复用模型中，引入了一种新的系统调用，查询IO的就绪状态。在Linux系统中，对应的系统调用为select/epoll系统调用。通过该系统调用，一个进程可以监视多个文件描述符，一旦某个描述符就绪（一般是内核缓冲区可读/可写），内核能够将就绪的状态返回给应用程序。随后，应用程序根据就绪的状态，进行相应的IO系统调用。

目前支持IO多路复用的系统调用，有select、epoll等等。select系统调用，几乎在所有的操作系统上都有支持，具有良好的跨平台特性。epoll是在Linux 2.6内核中提出的，是select系统调用的Linux增强版本。

在IO多路复用模型中通过select/epoll系统调用，单个应用程序的线程，可以不断地轮询成百上千的socket连接，当某个或者某些socket网络连接有IO就绪的状态，就返回对应的可以执行的读写操作。

举个例子来说明IO多路复用模型的流程。发起一个多路复用IO的read读操作的系统调用，流程如下：

1. 选择器注册。在这种模式中，首先，将需要read操作的目标socket网络连接，提前注册到select/epoll选择器中，Java中对应的选择器类是Selector类。然后，才可以开启整个IO多路复用模型的轮询流程。
2. 就绪状态的轮询。通过选择器的查询方法，查询注册过的所有socket连接的就绪状态。通过查询的系统调用，内核会返回一个就绪的socket列表。当任何一个注册过的socket中的数据准备好了，内核缓冲区有数据（就绪）了，内核就将该socket加入到就绪的列表中。当用户进程调用了select查询方法，那么整个线程会被阻塞掉。
3. 用户线程获得了就绪状态的列表后，根据其中的socket连接，发起read系统调用，用户线程阻塞。内核开始复制数据，将数据从内核缓冲区复制到用户缓冲区。
4. 复制完成后，内核返回结果，用户线程才会解除阻塞的状态，用户线程读取到了数据，继续执行。

IO多路复用模型的特点：IO多路复用模型的IO涉及两种系统调用，一种是select/epoll（就绪查询），另一种是IO操作。IO多路复用模型建立在操作系统的基础设施之上，即操作系统的内核必须能够提供多路分离的系统调用select/epoll。和NIO模型相似，多路复用IO也需要轮询。负责select/epoll状态查询调用的线程，需要不断地进行select/epoll轮询，查找出达到IO操作就绪的socket连接。IO多路复用模型与同步非阻塞IO模型是有密切关系的。对于注册在选择器上的每一个可以查询的socket连接，一般都设置成为同步非阻塞模型。

IO多路复用模型的优点：与一个线程维护一个连接的阻塞IO模式相比，使用select/epoll的最大优势在于，一个选择器查询线程可以同时处理成千上万个连接（Connection）。系统不必创建大量的线程，也不必维护这些线程，从而大大减小了系统的开销。

IO多路复用模型的缺点：本质上，select/epoll系统调用是阻塞式的，属于同步IO。都需要在读写事件就绪后，由系统调用本身负责进行读写，也就是说这个读写过程是阻塞的。如何彻底地解除线程的阻塞，就必须使用异步IO模型。

Java语言的NIO（New IO）技术，使用的就是IO多路复用模型。在Linux系统上，使用的是epoll系统调用。

### 异步IO（Asynchronous IO）

异步IO，指的是用户空间与内核空间的调用方式反过来。用户空间的线程变成被动接受者，而内核空间成了主动调用者。这有点类似于Java中比较典型的回调模式，用户空间的线程向内核空间注册了各种IO事件的回调函数，由内核去主动调用。

异步IO模型（Asynchronous IO，简称为AIO）。AIO的基本流程是：用户线程通过系统调用，向内核注册某个IO操作。内核在整个IO操作（包括数据准备、数据复制）完成后，通知用户程序，用户执行后续的业务操作。在异步IO模型中，在整个内核的数据处理过程中，包括内核将数据从网络物理设备（网卡）读取到内核缓冲区、将内核缓冲区的数据复制到用户缓冲区，用户程序都不需要阻塞。

举个例子。发起一个异步IO的read读操作的系统调用，流程如下：

1. 当用户线程发起了read系统调用，立刻就可以开始去做其他的事，用户线程不阻塞。
2. 内核就开始了IO的第一个阶段：准备数据。等到数据准备好了，内核就会将数据从内核缓冲区复制到用户缓冲区（用户空间的内存）。
3. 内核会给用户线程发送一个信号（Signal），或者回调用户线程注册的回调接口，告诉用户线程read操作完成了。
4. 用户线程读取用户缓冲区的数据，完成后续的业务操作。

异步IO模型的特点：在内核等待数据和复制数据的两个阶段，用户线程都不是阻塞的。用户线程需要接收内核的IO操作完成的事件，或者用户线程需要注册一个IO操作完成的回调函数。正因为如此，异步IO有的时候也被称为信号驱动IO。

异步IO异步模型的缺点：应用程序仅需要进行事件的注册与接收，其余的工作都留给了操作系统，也就是说，需要底层内核提供支持。理论上来说，异步IO是真正的异步输入输出，它的吞吐量高于IO多路复用模型的吞吐量。就目前而言，Windows系统下通过IOCP实现了真正的异步IO。而在Linux系统下，异步IO模型在2.6版本才引入，目前并不完善，其底层实现仍使用epoll，与IO多路复用相同，因此在性能上没有明显的优势。

大多数的高并发服务器端的程序，一般都是基于Linux系统的。因而，目前这类高并发网络应用程序的开发，大多采用IO多路复用模型。大名鼎鼎的Netty框架，使用的就是IO多路复用模型，而不是异步IO模型。

## ==SELECT POLL EPOLL==

### 从网卡接收数据说起

下图是一个典型的计算机结构图，计算机由CPU、存储器（内存）、网络接口等部件组成。了解epoll本质的**第一步**，要从**硬件**的角度看计算机怎样接收网络数据。

![img](https://pic2.zhimg.com/80/v2-e549406135abf440331de9dd8c3925e9_720w.jpg)

下图展示了网卡接收数据的过程。在①阶段，网卡收到网线传来的数据；经过②阶段的硬件电路的传输；最终将数据写入到内存中的某个地址上（③阶段）。这个过程涉及到DMA传输、IO通路选择等硬件有关的知识，但我们只需知道：**网卡会把接收到的数据写入内存。**

<img src="https://pic2.zhimg.com/80/v2-6827b63c9fb42823dcd1913ea5433b15_720w.jpg" alt="img" style="zoom:50%;" />

通过硬件传输，网卡接收的数据存放到内存中。操作系统就可以去读取它们。

### 如何知道接收了数据？

了解epoll本质的**第二步**，要从**CPU**的角度来看数据接收。要理解这个问题，要先了解一个概念——中断。

计算机执行程序时，会有优先级的需求。比如，当计算机收到断电信号时（电容可以保存少许电量，供CPU运行很短的一小段时间），它应立即去保存数据，保存数据的程序具有较高的优先级。

一般而言，由硬件产生的信号需要cpu立马做出回应（不然数据可能就丢失），所以它的优先级很高。cpu理应中断掉正在执行的程序，去做出响应；当cpu完成对硬件的响应后，再重新执行用户程序。中断的过程如下图，和函数调用差不多。只不过函数调用是事先定好位置，而中断的位置由“信号”决定。

![img](https://pic4.zhimg.com/80/v2-89a9490f1d5c316167ff4761184239f7_720w.jpg)

以键盘为例，当用户按下键盘某个按键时，键盘会给cpu的中断引脚发出一个高电平。cpu能够捕获这个信号，然后执行键盘中断程序。下图展示了各种硬件通过中断与cpu交互。

<img src="https://pic3.zhimg.com/80/v2-c756381c0f63f9104f9102d280759d22_720w.jpg" alt="img" style="zoom:80%;" />

现在可以回答本节提出的问题了：当网卡把数据写入到内存后，**网卡向cpu发出一个中断信号，操作系统便能得知有新数据到来**，再通过网卡**中断程序**去处理数据。

### 进程阻塞为什么不占用cpu资源？

了解epoll本质的**第三步**，要从**操作系统进程调度**的角度来看数据接收。阻塞是进程调度的关键一环，指的是进程在等待某事件（如接收到网络数据）发生之前的等待状态，recv、select和epoll都是阻塞方法。**了解“进程阻塞为什么不占用cpu资源？”，也就能够了解这一步**。

为简单起见，我们从普通的recv接收开始分析，先看看下面代码：

```java
//创建socket
int s = socket(AF_INET, SOCK_STREAM, 0);   
//绑定
bind(s, ...)
//监听
listen(s, ...)
//接受客户端连接
int c = accept(s, ...)
//接收客户端数据
recv(c, ...);
//将数据打印出来
printf(...)
```

这是一段最基础的网络编程代码，先新建socket对象，依次调用bind、listen、accept，最后调用recv接收数据。recv是个阻塞方法，当程序运行到recv时，它会一直等待，直到接收到数据才往下执行。

那么阻塞的原理是什么？

**工作队列**

操作系统为了支持多任务，实现了进程调度的功能，会把进程分为“运行”和“等待”等几种状态。运行状态是进程获得cpu使用权，正在执行代码的状态；等待状态是阻塞状态，比如上述程序运行到recv时，程序会从运行状态变为等待状态，接收到数据后又变回运行状态。操作系统会分时执行各个运行状态的进程，由于速度很快，看上去就像是同时执行多个任务。

下图中的计算机中运行着A、B、C三个进程，其中进程A执行着上述基础网络程序，一开始，这3个进程都被操作系统的工作队列所引用，处于运行状态，会分时执行。

![img](https://pic3.zhimg.com/80/v2-2f3b71710f1805669a780a2d634f0626_720w.jpg)

工作队列中有A、B和C三个进程

**等待队列**

当进程A执行到创建socket的语句时，操作系统会创建一个由文件系统管理的socket对象（如下图）。这个socket对象包含了发送缓冲区、接收缓冲区、等待队列等成员。等待队列是个非常重要的结构，它指向所有需要等待该socket事件的进程。

![img](https://pic3.zhimg.com/80/v2-7ce207c92c9dd7085fb7b823e2aa5872_720w.jpg)

当程序执行到recv时，操作系统会将进程A从工作队列移动到该socket的等待队列中（如下图）。由于工作队列只剩下了进程B和C，依据进程调度，cpu会轮流执行这两个进程的程序，不会执行进程A的程序。**所以进程A被阻塞，不会往下执行代码，也不会占用cpu资源**。

![img](https://pic1.zhimg.com/80/v2-1c7a96c8da16f123388e46f88772e6d8_720w.jpg)

ps：操作系统添加等待队列只是添加了对这个“等待中”进程的引用，以便在接收到数据时获取进程对象、将其唤醒，而非直接将进程管理纳入自己之下。上图为了方便说明，直接将进程挂到等待队列之下。

**唤醒进程**

当socket接收到数据后，操作系统将该socket等待队列上的进程重新放回到工作队列，该进程变成运行状态，继续执行代码。也由于socket的接收缓冲区已经有了数据，recv可以返回接收到的数据。

### 内核接收网络数据全过程

**这一步，贯穿网卡、中断、进程调度的知识，叙述阻塞recv下，内核接收数据全过程。**

如下图所示，进程在recv阻塞期间，计算机收到了对端传送的数据（步骤①）。数据经由网卡传送到内存（步骤②），然后网卡通过中断信号通知cpu有数据到达，cpu执行中断程序（步骤③）。此处的中断程序主要有两项功能，先将网络数据写入到对应socket的接收缓冲区里面（步骤④），再唤醒进程A（步骤⑤），重新将进程A放入工作队列中。

![img](https://pic4.zhimg.com/80/v2-696b131cae434f2a0b5ab4d6353864af_720w.jpg)

唤醒进程的过程如下图所示。

![img](https://pic3.zhimg.com/80/v2-3e1d0a82cdc86f03343994f48d938922_720w.jpg)

**以上是内核接收数据全过程**

这里留有两个思考题，大家先想一想。

其一，操作系统如何知道网络数据对应于哪个socket？

其二，如何同时监视多个socket的数据？

第一个问题：因为一个socket对应着一个端口号，而网络数据包中包含了ip和端口的信息，内核可以通过端口号找到对应的socket。当然，为了提高处理速度，操作系统会维护端口号到socket的索引结构，以快速读取。

第二个问题是**多路复用的重中之重，**是本文后半部分的重点！

### 同时监视多个socket的简单方法

服务端需要管理多个客户端连接，而recv只能监视单个socket，这种矛盾下，人们开始寻找监视多个socket的方法。epoll的要义是**高效**的监视多个socket。从历史发展角度看，必然先出现一种不太高效的方法，人们再加以改进。只有先理解了不太高效的方法，才能够理解epoll的本质。

假如能够预先传入一个socket列表，**如果列表中的socket都没有数据，挂起进程，直到有一个socket收到数据，唤醒进程**。这种方法很直接，也是select的设计思想。

为方便理解，我们先复习select的用法。在如下的代码中，先准备一个数组（下面代码中的fds），让fds存放着所有需要监视的socket。然后调用select，**如果fds中的所有socket都没有数据，select会阻塞，直到有一个socket接收到数据，select返回，唤醒进程**。用户可以遍历fds，通过FD_ISSET判断具体哪个socket收到数据，然后做出处理。

```c++
int s = socket(AF_INET, SOCK_STREAM, 0);  
bind(s, ...)
listen(s, ...)

int fds[] =  存放需要监听的socket

while(1){
    int n = select(..., fds, ...)
    for(int i=0; i < fds.count; i++){
        if(FD_ISSET(fds[i], ...)){
            //fds[i]的数据处理
        }
    }
}
```

**select的流程**

select的实现思路很直接。假如程序同时监视如下图的sock1、sock2和sock3三个socket，那么在调用select之后，操作系统把进程A分别加入这三个socket的等待队列中。

![img](https://pic4.zhimg.com/80/v2-0cccb4976f8f2c2f8107f2b3a5bc46b3_720w.jpg)

当任何一个socket收到数据后，中断程序将唤起进程。下图展示了sock2接收到了数据的处理流程。

> ps：recv和select的中断回调可以设置成不同的内容。

![img](https://pic1.zhimg.com/80/v2-85dba5430f3c439e4647ea4d97ba54fc_720w.jpg)

sock2接收到了数据，中断程序唤起进程A

所谓唤起进程，就是将进程从所有的等待队列中移除，加入到工作队列里面。如下图所示。

![img](https://pic4.zhimg.com/80/v2-a86b203b8d955466fff34211d965d9eb_720w.jpg)

将进程A从所有等待队列中移除，再加入到工作队列里面

经由这些步骤，当进程A被唤醒后，它知道至少有一个socket接收了数据。程序只需遍历一遍socket列表，就可以得到就绪的socket。

这种简单方式**行之有效**，在几乎所有操作系统都有对应的实现。

**但是简单的方法往往有缺点，主要是：**

其一，每次调用select都需要将进程加入到所有监视socket的等待队列，每次唤醒都需要从每个队列中移除。这里涉及了两次遍历，而且每次都要将整个fds列表传递给内核，有一定的开销。正是因为遍历操作开销大，出于效率的考量，才会规定select的最大监视数量，默认只能监视1024个socket。

其二，进程被唤醒后，程序并不知道哪些socket收到数据，还需要遍历一次。

那么，有没有减少遍历的方法？有没有保存就绪socket的方法？这两个问题便是epoll技术要解决的。

### epoll的设计思路

epoll是在select出现N多年后才被发明的，是select和poll的增强版本。epoll通过以下一些措施来改进效率。

**措施一：功能分离**

select低效的原因之一是将“维护等待队列”和“阻塞进程”两个步骤合二为一。如下图所示，每次调用select都需要这两步操作，然而大多数应用场景中，需要监视的socket相对固定，并不需要每次都修改。epoll将这两个操作分开，先用epoll_ctl维护等待队列，再调用epoll_wait阻塞进程。显而易见的，效率就能得到提升。

![img](https://pic2.zhimg.com/80/v2-5ce040484bbe61df5b484730c4cf56cd_720w.jpg)

相比select，epoll拆分了功能

为方便理解后续的内容，我们先复习下epoll的用法。如下的代码中，先用epoll_create创建一个epoll对象epfd，再通过epoll_ctl将需要监视的socket添加到epfd中，最后调用epoll_wait等待数据。

```c++
int s = socket(AF_INET, SOCK_STREAM, 0);   
bind(s, ...)
listen(s, ...)

int epfd = epoll_create(...);
epoll_ctl(epfd, ...); //将所有需要监听的socket添加到epfd中

while(1){
    int n = epoll_wait(...)
    for(接收到数据的socket){
        //处理
    }
}
```

功能分离，使得epoll有了优化的可能。

**措施二：就绪列表**

select低效的另一个原因在于程序不知道哪些socket收到数据，只能一个个遍历。如果内核维护一个“就绪列表”，引用收到数据的socket，就能避免遍历。如下图所示，计算机共有三个socket，收到数据的sock2和sock3被rdlist（就绪列表）所引用。当进程被唤醒后，只要获取rdlist的内容，就能够知道哪些socket收到数据。

![img](https://pic4.zhimg.com/80/v2-5c552b74772d8dbc7287864999e32c4f_720w.jpg)

### epoll的原理和流程

本节会以示例和图表来讲解epoll的原理和流程。

**创建epoll对象**

如下图所示，当某个进程调用epoll_create方法时，内核会创建一个eventpoll对象（也就是程序中epfd所代表的对象）。eventpoll对象也是文件系统中的一员，和socket一样，它也会有等待队列。

![img](https://pic4.zhimg.com/80/v2-e3467895734a9d97f0af3c7bf875aaeb_720w.jpg)

创建一个代表该epoll的eventpoll对象是必须的，因为内核要维护“就绪列表”等数据，“就绪列表”可以作为eventpoll的成员。

**维护监视列表**

创建epoll对象后，可以用epoll_ctl添加或删除所要监听的socket。以添加socket为例，如下图，如果通过epoll_ctl添加sock1、sock2和sock3的监视，内核会将eventpoll添加到这三个socket的等待队列中。

![img](https://pic2.zhimg.com/80/v2-b49bb08a6a1b7159073b71c4d6591185_720w.jpg)

当socket收到数据后，中断程序会操作eventpoll对象，而不是直接操作进程。

**接收数据**

当socket收到数据后，中断程序会给eventpoll的“就绪列表”添加socket引用。如下图展示的是sock2和sock3收到数据后，中断程序让rdlist引用这两个socket。

![img](https://pic1.zhimg.com/80/v2-18b89b221d5db3b5456ab6a0f6dc5784_720w.jpg)

eventpoll对象相当于是socket和进程之间的中介，socket的数据接收并不直接影响进程，而是通过改变eventpoll的就绪列表来改变进程状态。

当程序执行到epoll_wait时，如果rdlist已经引用了socket，那么epoll_wait直接返回，如果rdlist为空，阻塞进程。

**阻塞和唤醒进程**

假设计算机中正在运行进程A和进程B，在某时刻进程A运行到了epoll_wait语句。如下图所示，内核会将进程A放入eventpoll的等待队列中，阻塞进程。

![img](https://pic1.zhimg.com/80/v2-90632d0dc3ded7f91379b848ab53974c_720w.jpg)

当socket接收到数据，中断程序一方面修改rdlist，另一方面唤醒eventpoll等待队列中的进程，进程A再次进入运行状态（如下图）。也因为rdlist的存在，进程A可以知道哪些socket发生了变化。

![img](https://pic4.zhimg.com/80/v2-40bd5825e27cf49b7fd9a59dfcbe4d6f_720w.jpg)

### epoll的实现细节

至此，相信读者对epoll的本质已经有一定的了解。但我们还留有一个问题，**eventpoll的数据结构**是什么样子？

再留两个问题，**就绪队列**应该应使用什么数据结构？eventpoll应使用什么数据结构来管理通过epoll_ctl添加或删除的socket？

如下图所示，eventpoll包含了lock、mtx、wq（等待队列）、rdlist等成员。rdlist和rbr是我们所关心的。

![img](https://pic4.zhimg.com/80/v2-e63254878f67751dcc07a25b93f974bb_720w.jpg)



**就绪列表的数据结构**

就绪列表引用着就绪的socket，所以它应能够快速的插入数据。

程序可能随时调用epoll_ctl添加监视socket，也可能随时删除。当删除时，若该socket已经存放在就绪列表中，它也应该被移除。

所以就绪列表应是一种能够快速插入和删除的数据结构。双向链表就是这样一种数据结构，epoll使用双向链表来实现就绪队列（对应上图的rdllist）。

**索引结构**

既然epoll将“维护监视队列”和“进程阻塞”分离，也意味着需要有个数据结构来保存监视的socket。至少要方便的添加和移除，还要便于搜索，以避免重复添加。红黑树是一种自平衡二叉查找树，搜索、插入和删除时间复杂度都是O(log(N))，效率较好。epoll使用了红黑树作为索引结构（对应上图的rbr）。

### 结论

epoll在select和poll（poll和select基本一样，有少量改进）的基础引入了eventpoll作为中间层，使用了先进的数据结构，是一种高效的多路复用技术。

![img](https://pic2.zhimg.com/80/v2-14e0536d872474b0851b62572b732e39_720w.jpg)

## ==JAVA NIO==

Java NIO由以下三个核心组件组成：· Channel（通道）· Buffer（缓冲区）· Selector（选择器）

### Channel

在OIO中，同一个网络连接会关联到两个流：一个输入流（Input Stream），另一个输出流（Output Stream）。通过这两个流，不断地进行输入和输出的操作。

在NIO中，同一个网络连接使用一个通道表示，所有的NIO的IO操作都是从通道开始的。一个通道类似于OIO中的两个流的结合体，既可以从通道读取，也可以向通道写入。

![image-20201211164947026](syd基础知识.assets/image-20201211164947026.png)

这里不对纷繁复杂的Java NIO通道类型进行过多的描述，仅仅聚焦于介绍其中最为重要的四种Channel（通道）实现：FileChannel、SocketChannel、ServerSocketChannel、DatagramChannel。对于以上四种通道，说明如下：

1. FileChannel文件通道，用于文件的数据读写。
2. SocketChannel套接字通道，用于Socket套接字TCP连接的数据读写。
3. ServerSocketChannel服务器嵌套字通道（或服务器监听通道），允许我们监听TCP连接请求，为每个监听到的请求，创建一个SocketChannel套接字通道。
4. DatagramChannel数据报通道，用于UDP协议的数据读写。

### Selector

一个网络连接，操作系统底层使用一个文件描述符来表示。

Selector在Java应用层面，实现对多个文件描述符的监视。

![image-20201211165213369](syd基础知识.assets/image-20201211165213369.png)

它是一个IO事件的查询器。通过选择器，一个线程可以查询多个通道的IO事件的就绪状态。实现IO多路复用，从具体的开发层面来说，

- 首先把通道注册到选择器中
- 然后通过选择器内部的机制，可以查询（select）这些注册的通道是否有已经就绪的IO事件（例如可读、可写、网络连接完成等）。

- 我们可以很简单地使用一个线程，通过选择器去管理多个通道。

这样系统不必为每一个网络连接（文件描述符）创建进程/线程，从而大大减小了系统的开销。

### Buffer

通道的读取，就是将数据从通道读取到缓冲区中；通道的写入，就是将数据从缓冲区中写入到通道中。缓冲区的使用，是面向流的OIO所没有的，也是NIO非阻塞的重要前提和基础之一。

==NIO的Buffer（缓冲区）本质上是一个内存块，既可以写入数据，也可以从中读取数据==

NIO的Buffer类，是一个抽象类，位于java.nio包中，其内部是一个内存块（数组）。NIO的Buffer与普通的内存块（Java数组）不同的是：NIO Buffer对象，提供了一组更加有效的方法，用来进行写入和读取的交替访问。需要强调的是：Buffer类是一个非线程安全类。

Buffer类是一个抽象类，对应于Java的主要数据类型，在NIO中有8种缓冲区类，分别如下：ByteBuffer、CharBuffer、DoubleBuffer、FloatBuffer、IntBuffer、LongBuffer、ShortBuffer、MappedByteBuffer。前7种Buffer类型，覆盖了能在IO中传输的所有的Java基本数据类型。第8种类型MappedByteBuffer是专门用于内存映射的一种ByteBuffer类型。

使用Java NIO Buffer类的基本步骤如下：

1. 使用创建子类实例对象的allocate()方法，创建一个Buffer类的实例对象。
2. 调用put方法，将数据写入到缓冲区中。
3. 写入完成后，在开始读取数据前，调用Buffer.flip()方法，将缓冲区转换为读模式。
4. 调用get方法，从缓冲区中读取数据。
5. 读取完成后，调用Buffer.clear() 或Buffer.compact()方法，将缓冲区转换为写入模式。

# 5.MySQl

## 数据库范式

满足高级依赖就意味着满足了低级依赖。

1. 第一范式（1NF）：属性不可拆分或无重复的列。

2. 第二范式（2NF）：就是有一个唯一主键，并且非主属性对候选键是完全依赖。

   例如，在选课关系表(学号，课程号，成绩，学分)，关键字为组合关键字(学号，课程号)，但由于非主属性学分仅依赖于课程号，对关键字(学号，课程号)只是部分依赖，而不是完全依赖，因此此种方式会导致数据冗余以及更新异常等问题，解决办法是将其分为两个关系模式：学生表(学号，课程号，分数)和课程表(课程号，学分)，新关系通过学生表中的外关键字课程号联系，在需要时进行连接。

3. 第三范式（3NF）：消除传递依赖。

   简而言之，第三范式（3NF）要求一个数据库表中不包含已在其它表中已包含的非主属性信息。例如，存在一个部门信息表，其中每个部门有部门编号（dept_id）、部门名称、部门简介等信息。那么在的员工信息表中列出部门编号后就不能再将部门名称、部门简介等与部门有关的信息再加入员工信息表中。如果不存在部门信息表，则根据第三范式（3NF）也应该构建它，否则就会有大量的数据冗余。简而言之，第三范式就是属性不依赖于其它非主属性。

   总结一下：两个表相关联，一个表只能有另一个表的一个依赖。

4. 第四范式（4NF）：一个表的主键只对应一个多值。

   例如，职工表(职工编号，职工孩子姓名，职工选修课程)，在这个表中，同一个职工可能会有多个职工孩子姓名，同样，同一个职工也可能会有多个职工选修课程，即这里存在着多值事实，不符合第四范式。如果要符合第四范式，只需要将上表分为两个表，使它们只有一个多值事实，例如职工表一(职工编号，职工孩子姓名)，职工表二(职工编号，职工选修课程)，两个表都只有一个多值事实，所以符合第四范式。

## 关系型和NoSQL数据库

### 关系型数据库

关系型数据库最典型的数据结构是表，由二维表及其之间的联系所组成的一个数据组织。
优点：

* 易于维护：都是使用表结构，格式一致；
* 使用方便：SQL语言通用，可用于复杂查询；
* 复杂操作：支持SQL，可用于一个表以及多个表之间非常复杂的查询。

缺点：

* 固定的表结构，灵活度稍欠；

### 非关系型数据库

非关系型数据库严格上不是一种数据库，应该是一种数据结构化存储方法的集合，可以是文档或者键值对等。
优点：

* 格式灵活：存储数据的格式可以是key,value形式、文档形式、图片形式等等，文档形式、图片形式等等。

缺点：

* 不提供sql支持；
* 不易于维护。

## 排序检索数据

```sql
SELECT name
FROM students
ORDER BY student_id DESC;
```

DESC关键字只应用到直接位于其前面的列名。在多个列上降序排序 如果想在多个列上进行降序排序，必须对每个列指定DESC关键字。

## 通配符匹配

%表示任何字符出现任意次数，_只匹配单个字符而不是多个字符。

```sql
SELECT id
FROM students
WHERE students_name LIKE '%Alex_';
```

## 正则表达式匹配

```sql
SELECT id
FROM students
WHERE students_name REGEXP 'Alex';
```

## 分组

```sql
SELECT name
FROM students
GROUP BY class_num
HAVING COUNT(*)>30
ORDER BY id;
```

## 联结

### 内部联结

内部联结基于两个表之间的相等测试，这种联结也称为等值联结。

```sql
SELECT s.student_name,c.class_name
FROM student as s INNER JOIN class as c
ON s.class_id = c.class_id;
```

### 自联结

一个表，自己和自己联结。

```sql
SELECT s1.student_name
FROM student AS s1, student AS s2
WHERE s1.id = s2.id
AND s2.teacher_name = 'xiaohua';
```

通常自联结的速度会比子查询快很多。

### 外部联结

外部联结包含了那些在相关表中没有关联行的行。这种类型的联结称为外部联结。

```sql
SELECT c.customer_id, o.orders_num
FROM customers AS c LEFT JOIN orders AS o
ON c.customer_id = o.customer_id;
```

## 插入数据

```sql
INSERT INTO students(
    student_name,
    id,
    teacher_name,
    math_score
)
VALUES(
    'liming',
    1,
    'xiaohua',
    100
);
```

## 更新数据

```
UPDATE student
SET math_score = 100
WHERE student_name = 'liming';
```

## 删除数据

```
DELETE FROM student
WHERE student_name = 'liming';
```

## 创建表

```sql
CREATE TABLE students
(
    student_id int NOT NULL AUTO_INCREMENT,
    student_name char(50) NOT NULL,
    teachr_name char(50) NULL,
    PRIMARY KEY (student_id),
    FOREIGN KEY (teacher_name) REFERENCES teachers (teacher_name)
)ENGINE=InnoDB;
```

## 事务

```sql
START TRANSACTION;
SAVEPOINT delete1;
DELETE FROM student WHERE student_name = 'liming';
ROLLBACK TO delete1;
COMMIT;
```

### ACID特性

* 原子性（atomicity）：原子性指整个数据库事务是不可分割的工作单位。只有使事务中所有的数据库操作都执行成功，才算整个事务成功。事务中任何一个SQL语句执行失败，已经执行成功的SQL语句也必须撤销，数据库状态应该退回到执行事务前的状态。**通过UNDO LOG实现。**
* 一致性（consistency）：一致性指事务将数据库从一种状态转变为下一种一致的状态。在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。例如，在表中有一个字段为姓名，为唯一约束，即在表中姓名不能重复。如果一个事务对姓名字段进行了修改，但是在事务提交或事务操作发生回滚后，表中的姓名变得非唯一了，这就破坏了事务的一致性要求，即事务将数据库从一种状态变为了一种不一致的状态。因此，事务是一致性的单位，如果事务中某个动作失败了，系统可以自动撤销事务——返回初始化的状态。
* 隔离性（isolation）：隔离性还有其他的称呼，如并发控制（concurrency control）、可串行化（serializability）、锁（locking）等。事务的隔离性要求每个读写事务的对象对其他事务的操作对象能相互分离，即该事务提交前对其他事务都不可见，通常这使用锁来实现。当前数据库系统中都提供了一种粒度锁（granular lock）的策略，允许事务仅锁住一个实体对象的子集，以此来提高事务之间的并发度。**通过REDO LOG实现。**
* 持久性（durability）：事务一旦提交，其结果就是永久性的。即使发生宕机等故障，数据库也能将数据恢复。

### 四种隔离级别

SQL标准定义了4类隔离级别，包括了一些具体规则，用来限定事务内外的哪些改变是可见的，哪些是不可见的。

低级别的隔离级一般支持更高的并发处理，并拥有更低的系统开销。

* Read Uncommitted（读取未提交内容）

  在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）。

  

* Read Committed（读取提交内容）

  ==Oracle、SqlServer（默认都是PC，Read Committed），MySQL默认（RR，可重复读级别）==足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。

  

* Repeatable Read（可重读）

  这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。

  简单的说，幻读指在一个事务内读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户第二次使用相同的查询语句再读取该范围的数据行时，会发现有新的“幻影” 行——结果与第一次查询不同。换句话说，一个事务里的两个相同条件的查询查到的结果是不一致的。InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。

* Serializable（可串行化）

  这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。


### UNDO LOG与REDO LOG

innodb事务日志包括redo log和undo log。在概念上，innodb通过**force log at commit**机制实现事务的持久性，即在事务提交的时候，必须先将该事务的所有事务日志写入到磁盘上的redo log file和undo log file中进行持久化。

* redo log

  用于记录事务执行后的状态，提供前滚操作。通常是物理日志。

  redo log又称重做日志文件，记录的是数据修改之后的值，不管事务是否提交都会记录下来。它用来恢复提交后的物理数据页，且只能恢复到最后一次提交的位置。

  在实例和介质失败时，redo log文件就能派上用场，如数据库掉电，InnoDB存储引擎会使用redo log恢复到掉电前的时刻，以此来保证数据的完整性。

  在一条更新语句进行执行的时候，InnoDB引擎会把更新记录写到redo log日志中，然后更新内存，此时算是语句执行完了，然后在空闲的时候或者是按照设定的更新策略将redo log中的内容更新到磁盘中。

  作用：

  确保事务的持久性。防止在发生故障的时间点，尚有脏页未写入磁盘，在重启mysql服务的时候，根据redo log进行重做，从而达到事务的持久性这一特性。

* undo log

  回滚日志，用于记录事务开始前的状态，提供回滚操作。一般是逻辑日志，根据每行记录进行记录。

  作用：

  保存了事务发生之前的数据的一个版本，可以用于回滚，同时可以提供多版本并发控制下的读（MVCC），也即非锁定读。

## ==索引==

索引是一种特殊的文件，它们包含着对数据表里所有记录的引用指针。
更通俗的说，数据库索引好比是一本书前面的目录，能加快数据库的查询速度。
一般数据库默认都会为主键生成索引。
索引分为聚簇索引和非聚簇索引两种，聚簇索引是按照数据存放的物理位置为顺序的。因此遍历快而修改慢。
聚簇索引能提高多行检索的速度，而非聚簇索引对于单行的检索很快。

### 聚集索引和非聚集索引

索引在数据库中的作用类似于目录在书籍中的作用，用来提高查找信息的速度。使用索引查找数据，无需对整表进行扫描，可以快速找到所需数据。

**聚集索引就是存放的物理顺序和列中的顺序一样,一般设置主键索引就为聚集索引。**

一个没加主键的表，它的数据**无序**的放置在磁盘存储器上，一行一行的排列的很整齐。如果给表上了主键，那么表在磁盘上的存储结构就由整齐排列的结构转变成了**树状结构**，也就是**平衡树结构**，换句话说，就是整个表就变成了一个索引，也就是所谓的**聚集索引**。 这就是为什么一个表只能有一个主键， 一个表只能有一个聚集索引，因为主键的作用就是把表的数据格式转换成索引（平衡树）的格式放置。

![img](https://pic2.zhimg.com/v2-11ad4e1d08351fed1bff6a9232c0b261_b.jpg)

 上图就是带有主键的表（聚集索引）的结构图。其中树的所有结点（底部除外）的数据都是由主键字段中的数据构成，也就是通常我们指定主键的id字段。最下面部分是真正表中的数据。 假如我们执行一个SQL语句：

```text
select * from table where id = 1256
```

 首先根据索引定位到1256这个值所在的叶结点，然后再通过叶结点取到id等于1256的数据行。 这里不讲解平衡树的运行细节， 但是从上图能看出，树一共有三层， 从根节点至叶节点只需要经过三次查找就能得到结果。如下图

![img](https://pic4.zhimg.com/v2-79cbb0688cb16b7242c012aa919eaf5b_b.jpg)

然而， 事物都是有两面的， 索引能让数据库查询数据的速度上升， 而使写入数据的速度下降，原因很简单的，**因为平衡树这个结构必须一直维持在一个正确的状态， 增删改数据都会改变平衡树各节点中的索引数据内容，破坏树结构， 因此，在每次数据改变时， DBMS必须去重新梳理树（索引）的结构以确保它的正确**，这会带来不小的性能开销，也就是为什么索引会给查询以外的操作带来副作用的原因。







讲完聚集索引 ， 接下来聊一下非聚集索引， 也就是我们平时经常提起和使用的常规索引。

非聚集索引和聚集索引一样， 同样是采用平衡树作为索引的数据结构。索引树结构中各节点的值来自于表中的索引字段， 假如给user表的name字段加上索引 ， 那么索引就是由name字段中的值构成，在数据改变时， DBMS需要一直维护索引结构的正确性。如果给表中多个字段加上索引 ， 那么就会出现多个独立的索引结构，**每个索引（非聚集索引）互相之间不存在关联**。 如下图

![img](https://pic4.zhimg.com/v2-9c21eca4e9f4e776e472523ace8379ab_b.jpg)

**每次给字段建一个新索引， 字段中的数据就会被复制一份出来， 用于生成索引。 因此， 给表添加索引，会增加表的体积， 占用磁盘存储空间。**

**非聚集索引和聚集索引的区别在于：**

通过聚集索引可以一次查到需要查找的数据， 而通过非聚集索引第一次只能查到记录对应的主键值 ， 再使用主键的值通过聚集索引查找到需要的数据。

聚集索引一张表只能有一个，而非聚集索引一张表可以有多个。



#### ==总结==
InnoDB：

- 聚簇索引：
  - 主键：B+树结构，叶子节点存数据页（每个主键对应的记录）
  - 优点
    - 数据访问更快。聚触1次查找到，非聚触要2次
    - 排序查找和范围查找快。聚簇索引对于主键的排序查找和范围查找速度非常快
  - 缺点
    - **更新主键的代价很高**，因为将会导致被更新的行移动。
    - **插入速度严重依赖于插入顺序**，按照主键的**顺序插入**是最快的方式，否则将会出现页分裂

- 非聚触索引：
  - 其他索引：B+树结构，叶子节点存（主键+其他索引内容）。<br />查找：二级索引（非聚簇索引）访问需要两次索引查找



MyISAM：

- 主键：B+树结构（和InnoDB结构不同），叶子结点存（行数据地址值），不唯一属性的不能用聚触索引
- 聚触和非聚触：结构都一样，只是聚触的索引不能建立在非唯一的字段上。



### 普通、唯一、联合

**普通**

这是最基本的索引，它没有任何限制。

```sql
CREATE INDEX name ON students(student_name(50));
```



唯一索引

唯一的索引意味着两个行不能拥有相同的索引值。但允许有空值（注意和主键不同）

```sql
CREATE UNIQUE INDEX id ON students(student_id(10));
```



联合索引

联合索引是指对表上的多个列进行索引。

```sql
CREATE INDEX name_id ON students(student_name,student_id);
```

注意最左前缀。

### 覆盖索引、最左前缀原则、索引下推

```sql
mysql> 
create table T (
ID int primary key,
k int NOT NULL DEFAULT 0, 
s varchar(16) NOT NULL DEFAULT '',
index k(k)) engine=InnoDB;

insert into T values(100,1, 'aa'),(200,2,'bb'),(300,3,'cc'),(500,5,'ee'),(600,6,'ff'),(700,7,'gg');
```

执行 ==select * from T where k between 3 and 5==，需要执行几次树的搜索操作，会扫描多少行？

![img](https:////upload-images.jianshu.io/upload_images/16701032-87dce0dee11dc2bf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1142/format/webp)

这条SQL查询语句的执行流程：

- 1.在k索引树上找到k=3的记录，取得 ID = 300；
- 2.再到ID索引树查到ID=300对应的R3；
- 3.在k索引树取下一个值k=5，取得ID=500；
- 4.再回到ID索引树查到ID=500对应的R4；
- 5.在k索引树取下一个值k=6，不满足条件，循环结束。

在这个过程中，==回到主键索引树搜索的过程，我们称为回表==。可以看到，这个查询过程读了k索引树的3条记录（步骤1、3和5），回表了两次（步骤2和4）。



在这个例子中，由于查询结果所需要的数据只在主键索引上有，所以不得不回表。

那么，有没有可能经过索引优化，避免回表过程呢？



#### 覆盖索引

如果执行的语句是select ID from T where k between 3 and 5，这时只需要查ID的值，而ID的值已经在k索引树上了，因此可以直接提供查询结果，不需要回表。也就是说，在这个查询里面，==索引k已经“覆盖了”我们的查询需求，我们称为覆盖索引==。

**由于覆盖索引可以减少树的搜索次数，显著提升查询性能，所以使用覆盖索引是一个常用的性能优化手段。**

需要注意的是，在引擎内部使用覆盖索引在索引k上其实读了三个记录，R3~R5（对应的索引k上的记录项），但是对于MySQL的Server层来说，它就是找引擎拿到了两条记录，因此MySQL认为扫描行数是2。

基于上面覆盖索引的说明，我们来讨论一个问题：在一个市民信息表上，是否有必要将身份证号和名字建立联合索引？

假设这个市民表的定义是这样的：

```go
CREATE TABLE `tuser` (
  `id` int(11) NOT NULL,
  `id_card` varchar(32) DEFAULT NULL,
  `name` varchar(32) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `ismale` tinyint(1) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `id_card` (`id_card`),
  KEY `name_age` (`name`,`age`)
) ENGINE=InnoDB
```

我们知道，身份证号是市民的唯一标识。也就是说，如果有根据身份证号查询市民信息的需求，我们只要在身份证号字段上建立索引就够了。而再建立一个（身份证号、姓名）的联合索引，是不是浪费空间？

如果现在有一个高频请求，要根据市民的身份证号查询他的姓名，这个联合索引就有意义了。它可以在这个高频请求上用到覆盖索引，不再需要回表查整行记录，减少语句的执行时间。

当然，索引字段的维护总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要权衡考虑了。这正是业务DBA，或者称为业务数据架构师的工作。

#### 最左前缀原则

看到这里你一定有一个疑问，如果为每一种查询都设计一个索引，索引是不是太多了。如果我现在要按照市民的身份证号去查他的家庭地址呢？虽然这个查询需求在业务中出现的概率不高，但总不能让它走全表扫描吧？反过来说，==单独为一个不频繁的请求创建一个（身份证号，地址）的索引又感觉有点浪费。应该怎么做呢？==

B+树这种索引结构，可以利用索引的“最左前缀”，来定位记录。

为了直观地说明这个概念，我们用（name，age）这个联合索引来分析。

![img](https:////upload-images.jianshu.io/upload_images/16701032-1dd09c02ccd95166.png?imageMogr2/auto-orient/strip|imageView2/2/w/1142/format/webp)

可以看到，索引项是按照索引定义里面出现的字段顺序排序的。

当你的逻辑需求是查到所有名字是“张三”的人时，可以快速定位到ID4，然后向后遍历得到所有需要的结果。

如果你要查的是所有名字第一个字是“张”的人，你的SQL语句的条件是"where name like ‘张%’"。这时，你也能够用上这个索引，查找到第一个符合条件的记录是ID3，然后向后遍历，直到不满足条件为止。

可以看到，不只是索引的全部定义，只要满足最左前缀，就可以利用索引来加速检索。这个最左前缀可以是联合索引的最左N个字段，也可以是字符串索引的最左M个字符。

基于上面对最左前缀索引的说明，我们来讨论一个问题：**在建立联合索引的时候，如何安排索引内的字段顺序。**

这里的评估标准是，索引的复用能力。因为可以支持最左前缀，所以当已经有了(a,b)这个联合索引后，一般就不需要单独在a上建立索引了。==因第一原则是，如果通过调整顺序，可以少维护一个索引，那么这个顺序往往就是需要优先考虑采用的。==

所以现在你知道了，这段开头的问题里，我们要为高频请求创建(身份证号，姓名）这个联合索引，并用这个索引支持“根据身份证号查询地址”的需求。

那么，如果既有联合查询，又有基于a、b各自的查询呢？查询条件里面只有b的语句，是无法使用(a,b)这个联合索引的，这时候你不得不维护另外一个索引，也就是说你需要同时维护(a,b)、(b) 这两个索引。

这时候，**我们要考虑的原则就是空间了。**比如上面这个市民表的情况，name字段是比age字段大的 ，那我就建议你创建一个（name,age)的联合索引和一个(age)的单字段索引。

#### 索引下推

以市民表的联合索引（name, age）为例。如果现在有一个需求：检索出表中“名字第一个字是张，而且年龄是10岁的所有男孩”。那么，SQL语句是这么写的：

```csharp
mysql> select * from tuser where name like '张%' and age=10 and ismale=1;
```

你已经知道了前缀索引规则，所以这个语句在搜索索引树的时候，只能用 “张”，找到第一个满足条件的记录ID3。当然，这还不错，总比全表扫描要好。

然后呢？

当然是判断其他条件是否满足。

在MySQL 5.6之前，只能从ID3开始一个个回表。到主键索引上找出数据行，再对比字段值。

而MySQL 5.6 引入的索引下推优化（index condition pushdown)， 可以在索引遍历过程中，对索引中包含的字段先做判断，直接过滤掉不满足条件的记录，减少回表次数。

- 无索引下推执行流程图

  ![img](https:////upload-images.jianshu.io/upload_images/16701032-463c8ea7f3684669.png?imageMogr2/auto-orient/strip|imageView2/2/w/1142/format/webp)

- 索引下推执行流程

  ![img](https:////upload-images.jianshu.io/upload_images/16701032-4e53371c7c59c392.png?imageMogr2/auto-orient/strip|imageView2/2/w/1142/format/webp)

这两个图里面，每一个虚线箭头表示回表一次。

第一个图中，在(name,age)索引里面我特意去掉了age的值，这个过程InnoDB并不会去看age的值，只是按顺序把“name第一个字是’张’”的记录一条条取出来回表。因此，需要回表4次。

他们的区别是，InnoDB在(name,age)索引内部就判断了age是否等于10，对于不等于10的记录，直接判断并跳过。在这个例子中，只需要对ID4、ID5这两条记录回表取数据判断，就只需要回表2次。

### 为什么Mysql索引采用B+树

#### 二叉查找树

一个二叉查找树是由n个节点随机构成，所以，对于某些情况，二叉查找树会退化成一个有n个节点的线性链。显然这个二叉树的查询效率就很低，因此若想最大性能的构造一个二叉查找树，需要这个二叉树是平衡的，从而引出了一个新的定义-平衡二叉树AVL。

#### AVL树

AVL树是带有平衡条件的二叉查找树，一般是用平衡因子差值判断是否平衡并通过旋转来实现平衡，左右子树树高不超过1，和红黑树相比，它是严格的平衡二叉树，平衡条件必须满足所有节点的左右子树高度差不超过1。不管我们是执行插入还是删除操作，只要不满足上面的条件，就要通过旋转来保持平衡，而旋转是非常耗时的。由此我们可以知道**AVL树适合用于插入删除次数比较少，但查找多的情况**。 

由于维护这种高度平衡所付出的代价比从中获得的效率收益还大，故而实际的应用不多，更多的地方是用追求局部而不是非常严格整体平衡的红黑树。当然，如果应用场景中对插入删除不频繁，只是对查找要求较高，那么AVL还是较优于红黑树。

#### 红黑树

一种二叉查找树，但在每个节点增加一个存储位表示节点的颜色，可以是red或black。通过对任何一条从根到叶子的路径上各个节点着色的方式的限制，红黑树确保没有一条路径会比其它路径长出两倍。它是一种弱平衡二叉树(由于是若平衡，可以推出，相同的节点情况下，AVL树的高度低于红黑树)，相对于要求严格的AVL树来说，它的旋转次数变少，所以对于搜索、插入、删除操作多的情况下，我们就用红黑树。

广泛用于C++的STL中，Map和Set都是用红黑树实现的； 



为什么用B+ 而不用 红黑树呢？sun：http://www.cyc2018.xyz/%E6%95%B0%E6%8D%AE%E5%BA%93/MySQL.html#b-tree-%E5%8E%9F%E7%90%86

- B+ 树有更低的树高。红黑树的出度更小，所以书高更高，
- 磁盘访问原理
- 磁盘预读特性



#### B树

我们在MySQL中的数据一般是放在磁盘中的，读取数据的时候肯定会有访问磁盘的操作，磁盘中有两个机械运动的部分，分别是盘片旋转和磁臂移动。盘片旋转就是我们市面上所提到的多少转每分钟，而磁盘移动则是在盘片旋转到指定位置以后，移动磁臂后开始进行数据的读写。那么这就存在一个定位到磁盘中的块的过程，而定位是磁盘的存取中花费时间比较大的一块，毕竟机械运动花费的时候要远远大于电子运动的时间。当大规模数据存储到磁盘中的时候，显然定位是一个非常花费时间的过程，但是我们可以通过B树进行优化，提高磁盘读取时定位的效率。

为什么B类树可以进行优化呢？我们可以根据B类树的特点，构造一个多阶的B类树，然后在尽量多的在结点上存储相关的信息，保证层数尽量的少，以便后面我们可以更快的找到信息，磁盘的I/O操作也少一些，而且B类树是平衡树，每个结点到叶子结点的高度都是相同，这也保证了每个查询是稳定的。

总的来说，B/B+树是为了磁盘或其它存储设备而设计的一种平衡多路查找树(相对于二叉，B树每个内节点有多个分支)，与红黑树相比，在相同的的节点的情况下，一颗B/B+树的高度远远小于红黑树的高度(在下面B/B+树的性能分析中会提到)。B/B+树上操作的时间通常由存取磁盘的时间和CPU计算时间这两部分构成，而CPU的速度非常快，所以B树的操作效率取决于访问磁盘的次数，关键字总数相同的情况下B树的高度越小，磁盘I/O所花的时间越少。

**B树的性质：**

1. 定义任意非叶子结点最多只有M个儿子，且M>2； 
2. 根结点的儿子数为[2, M]； 
3. 除根结点以外的非叶子结点的儿子数为[M/2, M]； 
4. 每个结点存放至少M/2-1（取上整）和至多M-1个关键字；（至少2个关键字） 
5. 非叶子结点的关键字个数=指向儿子的指针个数-1； 
6. 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] < K[i+1]； 
7. 非叶子结点的指针：P[1], P[2], …, P[M]；其中P[1]指向关键字小于K[1]的子树，P[M]指向关键字大于K[M-1]的子树，其它P[i]指向关键字属于(K[i-1], K[i])的子树； 
8. 所有叶子结点位于同一层； 

#### B+树

B+树是应文件系统所需而产生的一种B树的变形树（文件的目录一级一级索引，只有最底层的叶子节点（文件）保存数据）非叶子节点只保存索引，不保存实际的数据，数据都保存在叶子节点中，这不就是文件系统文件的查找吗?

我们就举个文件查找的例子：有3个文件夹a、b、c， a包含b，b包含c，一个文件yang.c，a、b、c就是索引（存储在非叶子节点）， a、b、c只是要找到的yang.c的key，而实际的数据yang.c存储在叶子节点上。

所有的非叶子节点都可以看成索引部分！

B+树的性质(下面提到的都是和B树不相同的性质)：

1. 非叶子节点的子树指针与关键字个数相同； 
2. 非叶子节点的子树指针p[i],指向关键字值属于[k[i],k[i+1]]的子树.(B树是开区间,也就是说B树不允许关键字重复,B+树允许重复)； 
3. 为所有叶子节点增加一个链指针； 
4. 所有关键字都在叶子节点出现(稠密索引). (且链表中的关键字恰好是有序的)； 
5. 非叶子节点相当于是叶子节点的索引(稀疏索引),叶子节点相当于是存储(关键字)数据的数据层； 
6. 更适合于文件系统；

![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZWD664KnCQ2x68C8NUSn9ickla6KbCCFu39obZuGEooFPtVsap3picXKYLMLh3ibpeZ7b8lia1SvDN4w/640?tp=webp&wxfrom=5&wx_lazy=1)

为什么说B+树比B树更适合数据库索引？

1. B+树的磁盘读写代价更低：

   B+树的非叶子节点不含有指向关键字具体信息的指针，因此其内部节点相对B树更小。如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。

2. B+树的查询效率更加稳定：

   任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

3. 方便扫库：

   由于B+树的数据都存储在叶子结点中，分支结点均为索引，扫库只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以B+树更加适合在区间查询的情况，所以通常B+树用于数据库索引。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作或者说效率太低。

#### 总结

查找数据，最简单的方式是顺序查找。但是对于几十万上百万，甚至上亿的数据库查询就很慢了。

所以要对查找的方式进行优化，二叉树可以把速度提升到O(log(n,2))，查询的瓶颈在于树的深度，最坏的情况要查找到二叉树的最深层，由于，每查找深一层，就要访问更深一层的索引文件。在多达数G的索引文件中，这将是很大的开销。所以，尽量把数据结构设计的更为‘矮胖’一点就可以减少访问的层数。在众多的解决方案中，B/B+树很好的适合。

相比B树，B+树的父节点也必须存在于子节点中，是其中最大或者最小元素，B+树的节点只存储索引key值，具体信息的地址存在于叶子节点的地址中。这就使以页为单位的索引中可以存放更多的节点。减少更多的I/O支出。因此，B+树成为了数据库比较优秀的数据结构，MySQL中MyIsAM和InnoDB都是采用的B+树结构。不同的是前者是非聚集索引，后者主键是聚集索引，所谓聚集索引是物理地址连续存放的索引，在取区间的时候，查找速度非常快，但同样的，插入的速度也会受到影响而降低。聚集索引的物理位置使用链表来进行存储。

## 数据库锁

![img](https://pic2.zhimg.com/v2-fab8c4a1871e59e11567f4a7407495e1_b.jpg)

### 锁的分类

- **按锁的粒度划分**（即，每次上锁的对象是表，行还是页）：表级锁，行级锁，页级锁
- **按锁的级别划分**：共享锁、排他锁
- **按加锁方式分**：自动锁（存储引擎自行根据需要施加的锁）、显式锁（用户手动请求的锁）
- **按操作划分**：DML锁（对数据进行操作的锁)、DDL锁（对表结构进行变更的锁）
- **最后按使用方式划分**：悲观锁、乐观锁



### 锁粒度划分

#### 行级锁

行级锁是MySQL中锁定粒度最细的一种锁，表示只针对当前操作的行进行加锁。行级锁能大大减少数据库操作的冲突。其加锁粒度最小，但加锁的开销也最大。行级锁分为共享锁 和 排他锁。

特点：==开销大，加锁慢==；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。

#### 表级锁

表级锁是MySQL中锁定粒度最大的一种锁，表示对当前操作的整张表加锁，它实现简单，资源消耗较少，被大部分MySQL引擎支持。最常使用的MYISAM与INNODB都支持表级锁定。表级锁定分为表共享读锁（共享锁）与表独占写锁（排他锁）。

特点：开销小，加锁快；不会出现死锁；锁定粒度大，发出锁冲突的概率最高，并发度最低。

#### 页级锁

页级锁是MySQL中锁定粒度介于行级锁和表级锁中间的一种锁。表级锁速度快，但冲突多，行级冲突少，但速度慢。所以取了折衷的页级，**一次锁定相邻的一组记录**。==BDB支持页级锁==

特点：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般

#### MySQL常用存储引擎的锁机制

* MyISAM和MEMORY采用表级锁
* BDB采用页面锁或表级锁，默认为页面锁

* InnoDB支持行级锁和表级锁，**默认为行级锁**

InnoDB行锁是通过给索引上的索引项加锁来实现的，InnoDB这种行锁实现特点意味着：**只有通过索引条件检索数据，InnoDB才使用行级锁，否则，InnoDB将使用表锁！**

在实际应用中，要特别注意InnoDB行锁的这一特性，不然的话，可能导致大量的锁冲突，从而影响并发性能。

**行级锁都是基于索引的，如果一条SQL语句用不到索引是不会使用行级锁的，会使用表级锁。**行级锁的缺点是：由于需要请求大量的锁资源，所以速度慢，内存消耗大。

InnoDB引擎默认的修改数据语句：update,delete,insert都会自动给涉及到的数据加上排他锁。

select语句默认不会加任何锁类型，==如果加排他锁可以使用select …for update语句，加共享锁可以使用select … lock in share mode语句==

所以加过排他锁的数据行在其他事务种是不能修改数据的，也不能通过for update和lock in share mode锁的方式查询数据，但可以直接通过select …from…查询数据，因为普通查询没有任何锁机制。

##### ==猜想==

> 事务A对行a加了X锁，事务B读a，能够读出a的变化，就是能读出脏数据，uncommitted read：
>
> 不能读出a的变化，但能读出，
>
> 

### 行级锁与死锁

MyISAM中是不会产生死锁的，因为MyISAM总是一次性获得所需的全部锁，==要么全部满足，要么全部等待。而在InnoDB中，锁是逐步获得的，就造成了死锁的可能。==

在MySQL中，行级锁并不是直接锁记录，而是锁索引。索引分为主键索引和非主键索引两种，如果一条sql语句操作了主键索引，MySQL就会锁定这条主键索引；如果一条语句操作了非主键索引，MySQL会先锁定该非主键索引，再锁定相关的主键索引。在UPDATE、DELETE操作时，MySQL不仅锁定WHERE条件扫描过的所有索引记录，而且会锁定相邻的键值，即所谓的next-key locking。

> ?：什么意思



**当两个事务同时执行，一个锁住了主键索引，在等待其他相关索引。另一个锁定了非主键索引，在等待主键索引。这样就会发生死锁。**

发生死锁后，InnoDB一般都可以检测到，并使一个事务释放锁回退，另一个获取锁完成事务。

### 共享锁与排它锁

#### 共享锁（Share Lock）

共享锁又称读锁，是读取操作创建的锁。其他用户可以并发读取数据，但任何事务都不能对数据进行修改（获取数据上的排他锁），直到已释放所有共享锁。

如果事务T对数据A加上共享锁后，则其他事务只能对A再加共享锁，不能加排他锁。获准共享锁的事务只能读数据，不能修改数据。

用法 SELECT ... LOCK IN SHARE MODE;

在查询语句后面增加LOCK IN SHARE MODE，Mysql会对查询结果中的==每行都加共享锁==，当没有其他线程对查询结果集中的任何一行使用排他锁时，可以成功申请共享锁，否则会被阻塞。其他线程也可以读取使用了共享锁的表，而且这些线程读取的是同一个版本的数据。

#### 排它锁（eXclusive Lock）

排他锁又称写锁，如果事务T对数据A加上排他锁后，则其他事务不能再对A加任任何类型的封锁。获准排他锁的事务既能读数据，又能修改数据。

用法 SELECT ... FOR UPDATE;

在查询语句后面增加FOR UPDATE，Mysql会对查询结果中的==每行都加排他锁==，当没有其他线程对查询结果集中的任何一行使用排他锁时，可以成功申请排他锁，否则会被阻塞。

### 乐观锁和悲观锁

#### 乐观锁（Optimistic Lock）

假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。

**相对于悲观锁，在对数据库进行处理的时候，乐观锁并不会使用数据库提供的锁机制。一般的实现乐观锁的方式就是记录数据版本。**

数据版本,为数据增加的一个版本标识。当读取数据时，将版本标识的值一同读出，数据每更新一次，同时对版本标识进行更新。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的版本标识进行比对，如果数据库表当前版本号与第一次取出来的版本标识值相等，则予以更新，否则认为是过期数据。

实现数据版本有两种方式，第一种是使用版本号，第二种是使用时间戳。

##### 使用版本号实现乐观锁

使用版本号时，可以在数据初始化时指定一个版本号，每次对数据的更新操作都对版本号执行+1操作。并判断当前版本号是不是该数据的最新的版本号。

```text
1.查询出商品信息
select (status,status,version) from t_goods where id=#{id}
2.根据商品信息生成订单
3.修改商品status为2
update t_goods
set status=2,version=version+1
where id=#{id} and version=#{version};
```

乐观并发控制相信事务之间的数据竞争(data race)的概率是比较小的，==因此尽可能做下去，直到提交的时候才去锁定==，所以不会产生任何锁和死锁。但如果直接简单这么做，还是有可能会遇到不可预期的结果，例如两个事务都读取了数据库的某一行，经过修改以后写回数据库，这时就遇到了问题。

==疑问？什么问题==

> **注意：乐观锁的更新操作，最好用主键或者唯一索引来更新,这样是行锁，否则更新时会锁表**



#### 悲观锁（Pessimistic Lock）

在整个数据处理过程中，将数据处于锁定状态。**悲观锁的实现，往往依靠数据库提供的锁机制** （也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）

- 在对任意记录进行修改前，先尝试为该记录加上排他锁（exclusive locking）。
- 如果加锁失败，说明该记录正在被修改，那么当前查询可能要等待或者抛出异常（具体响应方式由开发者根据实际需要决定）
- 如果成功加锁，那么就可以对记录做修改，事务完成后就会解锁了。
- 其间如果有其他对该记录做修改或加排他锁的操作，都会等待我们解锁或直接抛出异常。

#### MySQL InnoDB 中使用悲观锁

**要使用悲观锁，我们必须关闭mysql数据库的自动提交属性，因为MySQL默认使用autocommit模式，也就是说，当你执行一个更新操作后，==MySQL会立刻将结果进行提交。set autocommit=0;==**

```text
// 0.开始事务
begin;

// 1.查询出商品信息
select status from t_goods where id=1 for update;

// 2.根据商品信息生成订单
insert into t_orders (id,goods_id) values (null,1);

// 3.修改商品status为2
update t_goods set status=2;

// 4.提交事务
commit;
```

上面的查询语句中，我们使用了select…for update的方式，这样就通过开启排他锁的方式实现了悲观锁。此时在t_goods表中，id为1的 那条数据就被我们锁定了，其它的事务必须等本次事务提交之后才能执行。这样我们可以保证当前的数据不会被其它事务修改。



### ==浅析MySQL二段锁??==

链接:https://blog.csdn.net/weixin_33889245/article/details/88926438?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4.control

### [多版本并发控制](https://blog.csdn.net/Waves___/article/details/105295060)

参考文档：[MySQL中MVCC的正确打开方式（源码佐证）](https://blog.csdn.net/Waves___/article/details/105295060)

- MVCC 中事务的修改操作（DELETE、INSERT、UPDATE）会为数据行新增一个版本快照。

#### **隐藏字段**

​    InnoDB存储引擎在每行数据的后面添加了三个隐藏字段：

   1. **DB_TRX_ID**(6字节)：表示最近一次对本记录行作修改（insert | update）的事务ID。至于delete操作，InnoDB认为是一个update操作，不过会更新一个另外的删除位，将行表示为deleted。并非真正删除。

   2. **DB_ROLL_PTR**(7字节)：回滚指针，指向当前记录行的undo log信息

   3. **DB_ROW_ID**(6字节)：随着新行插入而单调递增的行ID。理解：当表没有主键或唯一非空索引时，innodb就会使用这个行ID自动产生聚簇索引。如果表有主键或唯一非空索引，聚簇索引就不会包含这个行ID了。**这个DB_ROW_ID跟MVCC关系不大**。

#### **Read View 结构（重点）**

​    其实Read View（读视图），跟快照、snapshot是一个概念。

**① low_limit_id：目前出现过的最大的事务ID+1，即下一个将被分配的事务ID**

**② up_limit_id：\*活跃事务列表trx_ids中最小的事务ID，如果trx_ids为空，则up_limit_id 为 low_limit_id**

 **③ trx_ids：Read View创建时  其他未提交的活跃事务ID列表**

 ***④ creator_trx_id：当前创建事务的ID，是一个递增的编号***



#### **Undo log**    

在InnoDB里，undo log分为如下两类：
    ①insert undo log : 事务对insert新记录时产生的undo log, 只在事务回滚时需要, 并且在事务提交后就可以立即丢弃。
    ②update undo log : 事务对记录进行delete和update操作时产生的undo log，不仅在事务回滚时需要，快照读也需要，只有当数据库所使用的快照中不涉及该日志记录，对应的回滚日志才会被purge线程删除



#### 可见性比较算法

1. 如果 trx_id < up_limit_id, 那么表明“最新修改该行的事务”在“当前事务”创建快照之前就提交了，所以该记录行的值对当前事务是可见的。跳到步骤5。

   2. 如果 trx_id >= low_limit_id, 那么表明“最新修改该行的事务”在“当前事务”创建快照之后才修改该行，所以该记录行的值对当前事务不可见。跳到步骤4。

3. 如果 up_limit_id <= trx_id < low_limit_id, 表明“最新修改该行的事务”在“当前事务”创建快照的时候可能处于“活动状态”或者“已提交状态”；所以就要对活跃事务列表trx_ids进行查找（源码中是用的二分查找，因为是有序的）：
   1. 如果在活跃事务列表trx_ids中能找到 id 为 trx_id 的事务，表明①在“当前事务”创建快照前，“该记录行的值”被“id为trx_id的事务”修改了，但没有提交；或者②在“当前事务”创建快照后，“该记录行的值”被“id为trx_id的事务”修改了（不管有无提交）；这些情况下，这个记录行的值对当前事务都是不可见的，跳到步骤4；
   2. 在活跃事务列表中找不到，则表明“id为trx_id的事务”在修改“该记录行的值”后，在“当前事务”创建快照前就已经提交了，所以记录行对当前事务可见，跳到步骤5。

   4. 在该记录行的 DB_ROLL_PTR 指针所指向的undo log回滚段中，取出最新的的旧事务号DB_TRX_ID, 将它赋给trx_id，然后跳到步骤1重新开始判断。

   5. 将该可见行的值返回。

![img](syd基础知识.assets/20200413204733606.png)



###  ==Next-Key Locks？？==



### ？？关系数据库设计理论



### ？？ER图





# 6.JVM

## 运行时数据区

方法区、堆、虚拟机栈、本地方法栈、程序计数器。其中，方法区和堆是为所有线程所共享的，其它三个是线程隔离的。

### 程序计数器

程序计数器是一块较小的内存空间，它可以看作是当前线程所执行的字节码的行号指示器。在虚拟机的概念模型里，字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

由于Java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器（对于多核处理器来说是一个内核）都只会执行一条线程中的指令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各条线程之间计数器互不影响，独立存储。

如果线程正在执行的是一个Java方法，这个计数器记录的是正在执行的虚拟机字节码指令的地址；如果正在执行的是Native方法，这个计数器值则为空（Undefined）。

此内存区域是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。

### Java虚拟机栈

Java虚拟机栈的生命周期与线程相同。虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。局部变量表存放了编译期可知的各种基本数据类型、对象引用和returnAddress类型。

局部变量表所需的内存空间在编译期间完成分配，当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。

如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常。

### 本地方法栈

本地方法栈与虚拟机栈所发挥的作用是非常相似的，它们之间的区别不过是虚拟机栈为虚拟机执行Java方法服务，而本地方法栈则为虚拟机使用到的Native方法服务。本地方法栈区域也会抛出StackOverflowError异常。

### Java堆

对于大多数应用来说，Java堆是Java虚拟机所管理的内存中最大的一块。Java堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。

Java堆是垃圾收集器管理的主要区域。从内存回收的角度来看，由于现在收集器基本都采用分代收集算法，所以Java堆中还可以细分为：新生代和老年代。

根据Java虚拟机规范的规定，Java堆可以处于物理上不连续的内存空间中，只要逻辑上是连续的即可，就像我们的磁盘空间一样。在实现时，既可以实现成固定大小的，也可以是可扩展的，不过当前主流的虚拟机都是按照可扩展来实现的。如果在堆中没有内存完成实例分配，并且堆也无法再扩展时，将会抛出OutOfMemoryError异常。

### 方法区

方法区与Java堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

Java虚拟机规范对方法区的限制非常宽松，除了和Java堆一样不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾收集。相对而言，垃圾收集行为在这个区域是比较少出现的，但并非数据进入了方法区就如永久代的名字一样“永久”存在了。这区域的内存回收目标主要是针对常量池的回收和对类型的卸载，一般来说，这个区域的回收“成绩”比较难以令人满意，尤其是类型的卸载，条件相当苛刻，但是这部分区域的回收确实是必要的。

根据Java虚拟机规范的规定，当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常。

#### 运行时常量池

运行时常量池是方法区的一部分。Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。当常量池无法再申请到内存时会抛出OutOfMemoryError异常。

### 直接内存

直接内存并不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范中定义的内存区域。但是这部分内存也被频繁地使用，而且也可能导致OutOfMemoryError异常出现，所以我们放到这里一起讲解。在JDK 1.4中新加入了NIO（New Input/Output）类，引入了一种基于通道（Channel）与缓冲区（Buffer）的I/O方式，它可以使用Native函数库直接分配堆外内存，然后通过一个存储在Java堆中的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在Java堆和Native堆中来回复制数据。显然，本机直接内存的分配不会受到Java堆大小的限制，但是，既然是内存，肯定还是会受到本机总内存（包括RAM以及SWAP区或者分页文件）大小以及处理器寻址空间的限制。服务器管理员在配置虚拟机参数时，会根据实际内存设置-Xmx等参数信息，但经常忽略直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统级的限制），从而导致动态扩展时出现OutOfMemoryError异常。

## 垃圾判定算法

### 引用计数法

给对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻计数器为0的对象就是不可能再被使用的。

引用计数算法的实现简单，判定效率也很高，在大部分情况下它都是一个不错的算法。但是，主流的Java虚拟机里面没有选用引用计数算法来管理内存，其中最主要的原因是它很难解决对象之间相互循环引用的问题。

### 可达性分析算法

这个算法的基本思路就是通过一系列的称为“GC Roots”的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链，当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的。

在Java语言中，可作为GC Roots的对象包括下面几种：

* 虚拟机栈（栈帧中的本地变量表）中引用的对象。
* 方法区中类静态属性引用的对象。
* 方法区中常量引用的对象。
* 本地方法栈中JNI（即一般说的Native方法）引用的对象。

即使在可达性分析算法中不可达的对象，也并非是“非死不可”的，这时候它们暂时处于“缓刑”阶段，要真正宣告一个对象死亡，至少要经历两次标记过程：如果对象在进行可达性分析后发现没有与GC Roots相连接的引用链，那它将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行finalize()方法。当对象没有覆盖finalize()方法，或者finalize()方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行”。

如果这个对象被判定为有必要执行finalize()方法，那么这个对象将会放置在一个叫做F-Queue的队列之中，并在稍后由一个由虚拟机自动建立的、低优先级的Finalizer线程去执行它。这里所谓的“执行”是指虚拟机会触发这个方法，但并不承诺会等待它运行结束，这样做的原因是，如果一个对象在finalize()方法中执行缓慢，或者发生了死循环（更极端的情况），将很可能会导致F-Queue队列中其他对象永久处于等待，甚至导致整个内存回收系统崩溃。finalize()方法是对象逃脱死亡命运的最后一次机会，稍后GC将对F-Queue中的对象进行第二次小规模的标记，如果对象要在finalize()中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可，譬如把自己（this关键字）赋值给某个类变量或者对象的成员变量，那在第二次标记时它将被移除出“即将回收”的集合；如果对象这时候还没有逃脱，那基本上它就真的被回收了。

## 垃圾收集算法

### 标记-清除算法

最基础的收集算法是“标记-清除”算法，如同它的名字一样，算法分为“标记”和“清除”两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收所有被标记的对象。

它的主要不足有两个：一个是效率问题，标记和清除两个过程的效率都不高；另一个是空间问题，标记清除之后会产生大量不连续的内存碎片，空间碎片太多可能会导致以后在程序运行过程中需要分配较大对象时，无法找到足够的连续内存而不得不提前触发另一次垃圾收集动作。

### 复制算法

为了解决效率问题，一种称为“复制”的收集算法出现了，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。这样使得每次都是对整个半区进行内存回收，内存分配时也就不用考虑内存碎片等复杂情况，只要移动堆顶指针，按顺序分配内存即可，实现简单，运行高效。只是这种算法的代价是将内存缩小为了原来的一半，未免太高了一点。

现在的商业虚拟机都采用这种收集算法来回收新生代，IBM公司的专门研究表明，新生代中的对象98%是“朝生夕死”的，所以并不需要按照1∶1的比例来划分内存空间，而是将内存分为一块较大的Eden空间和两块较小的Survivor空间，每次使用Eden和其中一块Survivor。当回收时，将Eden和Survivor中还存活着的对象一次性地复制到另外一块Survivor空间上，最后清理掉Eden和刚才用过的Survivor空间。HotSpot虚拟机默认Eden和Survivor的大小比例是8∶1，也就是每次新生代中可用内存空间为整个新生代容量的90% （80%+10%），只有10%的内存会被“浪费”。当然，98%的对象可回收只是一般场景下的数据，我们没有办法保证每次回收都只有不多于10%的对象存活，当Survivor空间不够用时，需要依赖其他内存（这里指老年代）进行分配担保。

### 标记-整理算法

复制收集算法在对象存活率较高时就要进行较多的复制操作，效率将会变低。更关键的是，如果不想浪费50%的空间，就需要有额外的空间进行分配担保，以应对被使用的内存中所有对象都100%存活的极端情况，所以在老年代一般不能直接选用这种算法。

根据老年代的特点，有人提出了另外一种“标记-整理”（Mark-Compact）算法，标记过程仍然与“标记-清除”算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。

## 垃圾收集器

![img](http://img.blog.csdn.net/20170102225015393)

### Serial收集器

这个收集器是一个单线程的收集器，但它的“单线程”的意义并不仅仅说明它只会使用一个CPU或一条收集线程去完成垃圾收集工作，更重要的是在它进行垃圾收集时，必须暂停其他所有的工作线程，直到它收集结束。

实际上到现在为止，它依然是虚拟机运行在Client模式下的默认新生代收集器。它也有着优于其他收集器的地方：简单而高效（与其他收集器的单线程比），对于限定单个CPU的环境来说，Serial收集器由于没有线程交互的开销，专心做垃圾收集自然可以获得最高的单线程收集效率。在用户的桌面应用场景中，分配给虚拟机管理的内存一般来说不会很大，收集几十兆甚至一两百兆的新生代，停顿时间完全可以控制在几十毫秒最多一百多毫秒以内，只要不是频繁发生，这点停顿是可以接受的。所以，Serial收集器对于运行在Client模式下的虚拟机来说是一个很好的选择。

### ParNew收集器

ParNew收集器其实就是Serial收集器的多线程版本。

ParNew收集器除了多线程收集之外，其他与Serial收集器相比并没有太多创新之处，但它却是许多运行在Server模式下的虚拟机中首选的新生代收集器，其中有一个与性能无关但很重要的原因是，除了Serial收集器外，目前只有它能与CMS收集器配合工作。

### Parallel Scavenge收集器

Parallel Scavenge收集器是一个新生代收集器，它也是使用复制算法的收集器，又是并行的多线程收集器。

Parallel Scavenge收集器的特点是它的关注点与其他收集器不同，其它收集器的关注点是尽可能地缩短垃圾收集时用户线程的停顿时间，而Parallel Scavenge收集器的目标则是达到一个可控制的吞吐量。所谓吞吐量就是CPU用于运行用户代码的时间与CPU总消耗时间的比值。

### Serial Old收集器

Serial Old是Serial收集器的老年代版本，它同样是一个单线程收集器，使用“标记-整理”算法。这个收集器的主要意义也是在于给Client模式下的虚拟机使用。

### Parallel Old收集器

Parallel Old是Parallel Scavenge收集器的老年代版本，使用多线程和“标记-整理”算法。在注重吞吐量以及CPU资源敏感的场合，都可以优先考虑Parallel Scavenge加Parallel Old收集器。

### CMS收集器

CMS（Concurrent Mark Sweep）收集器是一种以获取最短回收停顿时间为目标的收集器。目前很大一部分的Java应用集中在互联网站或者B/S系统的服务端上，这类应用尤其重视服务的响应速度，希望系统停顿时间最短，以给用户带来较好的体验。CMS收集器就非常符合这类应用的需求。

CMS收集器是基于“标记—清除”算法实现的，它的运作过程相对于前面几种收集器来说更复杂一些，整个过程分为4个步骤，包括：

1. 初始标记：标记一下GC Roots能直接关联到的对象。

2. 并发标记：进行GCRoots Tracing的过程。

3. 重新标记：为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录。

4. 并发清除

其中，初始标记、重新标记这两个步骤仍然需要“Stop The World”。初始标记仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，并发标记阶段就是进行GCRoots Tracing的过程，而重新标记阶段则是为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，这个阶段的停顿时间一般会比初始标记阶段稍长一些，但远比并发标记的时间短。

由于整个过程中耗时最长的并发标记和并发清除过程收集器线程都可以与用户线程一起工作，所以，从总体上来说，CMS收集器的内存回收过程是与用户线程一起并发执行的。

## 类加载机制

### 字节码

在聊 Java 类加载机制之前，需要先了解一下 Java 字节码，因为它和类加载机制息息相关。

计算机只认识 0 和 1，所以任何语言编写的程序都需要编译成机器码才能被计算机理解，然后执行，Java 也不例外。

Java 在诞生的时候喊出了一个非常牛逼的口号：“Write Once, Run Anywhere”，为了达成这个目的，Sun 公司发布了许多可以在不同平台（Windows、Linux）上运行的 Java 虚拟机（JVM）——负责载入和执行 Java 编译后的字节码。



![img](https://pic4.zhimg.com/v2-c263eb51a16b1286dd859940e5763a33_b.jpg)



到底 Java 字节码是什么样子，我们借助一段简单的代码来看一看。

源码如下：

```text
package com.cmower.java_demo;

public class Test {

    public static void main(String[] args) {
        System.out.println("沉默王二");
    }

}
```

代码编译通过后，通过 `xxd Test.class` 命令查看一下这个字节码文件。

```text
xxd Test.class
00000000: cafe babe 0000 0034 0022 0700 0201 0019  .......4."......
00000010: 636f 6d2f 636d 6f77 6572 2f6a 6176 615f  com/cmower/java_
00000020: 6465 6d6f 2f54 6573 7407 0004 0100 106a  demo/Test......j
00000030: 6176 612f 6c61 6e67 2f4f 626a 6563 7401  ava/lang/Object.
00000040: 0006 3c69 6e69 743e 0100 0328 2956 0100  ..<init>...()V..
00000050: 0443 6f64 650a 0003 0009 0c00 0500 0601  .Code...........
00000060: 000f 4c69 6e65 4e75 6d62 6572 5461 626c  ..LineNumberTabl
```

感觉有点懵逼，对不对？

懵就对了。

这段字节码中的 `cafe babe` 被称为“魔数”，是 JVM 识别 .class 文件的标志。文件格式的定制者可以自由选择魔数值（只要没用过），比如说 .png 文件的魔数是 `8950 4e47`。

至于其他内容嘛，可以选择忘记了。

### 类加载过程

了解了 Java 字节码后，我们来聊聊 Java 的类加载过程。

Java 的类加载过程可以分为 5 个阶段：**载入、验证、准备、解析和初始化**。这 5 个阶段一般是顺序发生的，但在动态绑定的情况下，解析阶段发生在初始化阶段之后。

1. Loading（载入）

   JVM 在该阶段的主要目的是将字节码从不同的数据源（可能是 class 文件、也可能是 jar 包，甚至网络）转化为二进制字节流加载到内存中，并生成一个代表该类的 `java.lang.Class` 对象。

2. Verification（验证）

   JVM 会在该阶段对二进制字节流进行校验，只有符合 JVM 字节码规范的才能被 JVM 正确执行。该阶段是保证 JVM 安全的重要屏障，下面是一些主要的检查。

   * 确保二进制字节流格式符合预期（比如说是否以 `cafe bene` 开头）。
   * 是否所有方法都遵守访问控制关键字的限定。
   * 方法调用的参数个数和类型是否正确。
   * 确保变量在使用之前被正确初始化了。
   * 检查变量是否被赋予恰当类型的值。

3.  Preparation（准备）

   JVM 会在该阶段对类变量（也称为静态变量，`static` 关键字修饰的）分配内存并初始化（对应数据类型的默认初始值，如 0、0L、null、false 等）。

   也就是说，假如有这样一段代码：

   ```java
   public String chenmo = "沉默";
   public static String wanger = "王二";
   public static final String cmower = "沉默王二";
   ```

   chenmo 不会被分配内存，而 wanger 会；但 wanger 的初始值不是“王二”而是 `null`。

   需要注意的是，`static final` 修饰的变量被称作为常量，和类变量不同。常量一旦赋值就不会改变了，所以 cmower 在准备阶段的值为“沉默王二”而不是 `null`。

4. Resolution（解析）

   该阶段将常量池中的符号引用转化为直接引用。

   what？符号引用，直接引用？

   **符号引用**以一组符号（任何形式的字面量，只要在使用时能够无歧义的定位到目标即可）来描述所引用的目标。

   在编译时，Java 类并不知道所引用的类的实际地址，因此只能使用符号引用来代替。比如 `com.Wanger` 类引用了 `com.Chenmo` 类，编译时 Wanger 类并不知道 Chenmo 类的实际内存地址，因此只能使用符号 `com.Chenmo`。

   **直接引用**通过对符号引用进行解析，找到引用的实际内存地址。

5. Initialization（初始化）

   该阶段是类加载过程的最后一步。在准备阶段，类变量已经被赋过默认初始值，而在初始化阶段，类变量将被赋值为代码期望赋的值。换句话说，初始化阶段是执行类构造器方法的过程。

   oh，no，上面这段话说得很抽象，不好理解，对不对，我来举个例子。

   ```java
   String cmower = new String("沉默王二");
   ```

   上面这段代码使用了 `new` 关键字来实例化一个字符串对象，那么这时候，就会调用 String 类的构造方法对 cmower 进行实例化。

### 类加载器

聊完类加载过程，就不得不聊聊类加载器。

一般来说，Java 程序员并不需要直接同类加载器进行交互。JVM 默认的行为就已经足够满足大多数情况的需求了。不过，如果遇到了需要和类加载器进行交互的情况，而对类加载器的机制又不是很了解的话，就不得不花大量的时间去调试 
`ClassNotFoundException` 和 `NoClassDefFoundError` 等异常。

对于任意一个类，都需要由它的类加载器和这个类本身一同确定其在 JVM 中的唯一性。也就是说，如果两个类的加载器不同，即使两个类来源于同一个字节码文件，那这两个类就必定不相等（比如两个类的 Class 对象不 `equals`）。

站在程序员的角度来看，Java 类加载器可以分为三种。

1. 启动类加载器（Bootstrap Class-Loader），加载 `jre/lib` 包下面的 jar 文件，比如说常见的 rt.jar。

2. 扩展类加载器（Extension or Ext Class-Loader），加载 `jre/lib/ext` 包下面的 jar 文件。

3. 应用类加载器（Application or App Clas-Loader），根据程序的类路径（classpath）来加载 Java 类。

来来来，通过一段简单的代码了解下。

```java
public class Test {
    public static void main(String[] args) {
        ClassLoader loader = Test.class.getClassLoader();
        while (loader != null) {
            System.out.println(loader.toString());
            loader = loader.getParent();
        }
    }
}
```

每个 Java 类都维护着一个指向定义它的类加载器的引用，通过 `类名.class.getClassLoader()` 可以获取到此引用；然后通过 `loader.getParent()` 可以获取类加载器的上层类加载器。

这段代码的输出结果如下：

```text
sun.misc.Launcher$AppClassLoader@73d16e93
sun.misc.Launcher$ExtClassLoader@15db9742
```

第一行输出为 Test 的类加载器，即应用类加载器，它是 `sun.misc.Launcher$AppClassLoader` 类的实例；第二行输出为扩展类加载器，是 `sun.misc.Launcher$ExtClassLoader` 类的实例。那启动类加载器呢？

按理说，扩展类加载器的上层类加载器是启动类加载器，但在我这个版本的 JDK 中， 扩展类加载器的 `getParent()` 返回 `null`。所以没有输出。

### 双亲委派模型

如果以上三种类加载器不能满足要求的话，程序员还可以自定义类加载器（继承 `java.lang.ClassLoader` 类），它们之间的层级关系如下图所示。

<img src="https://pic1.zhimg.com/v2-ff2dae59f9bf4563436a7a564b41ee1c_b.jpg" alt="img" style="zoom:50%;" />



这种层次关系被称作为**双亲委派模型**：如果一个类加载器收到了加载类的请求，它会先把请求委托给上层加载器去完成，上层加载器又会委托上上层加载器，一直到最顶层的类加载器；如果上层加载器无法完成类的加载工作时，当前类加载器才会尝试自己去加载这个类。

PS：双亲委派模型突然让我联想到朱元璋同志，这个同志当上了皇帝之后连宰相都不要了，所有的事情都亲力亲为，只有自己没精力没时间做的事才交给大臣们去干。

使用双亲委派模型有一个很明显的好处，那就是 Java 类随着它的类加载器一起具备了一种带有优先级的层次关系，这对于保证 Java 程序的稳定运作很重要。

上文中曾提到，如果两个类的加载器不同，即使两个类来源于同一个字节码文件，那这两个类就必定不相等——双亲委派模型能够保证同一个类最终会被特定的类加载器加载。

# 7.计算机网络

## 五层结构

* 物理层：单位是电平信号。主要功能是电平解码和时空复用。
* 数据链路层：单位是帧。主要功能是以太网和MAC层。
* 网络层：单位是IP数据包。主要功能是ARP地址解析，子网划分，ICMP网管，网关协议和NAT。
* 传输层：单位是UCP数据报和TCP数据段。
* 应用层：单位是报文。

## TCP协议

提供可靠控制、流量控制、拥塞控制和连接管理的传输层协议。

### 报文字段

1. 源端口和目的端口

2. **序号(seq)**

   占4字节。序号范围是[0, 2^32 - 1]，共2^32（即4 294 967 296）个序号。序号增加到2^32- 1后，下一个序号就又回到0。也就是说，序号使用mod 232运算。

   TCP是面向字节流的。在一个TCP连接中传送的字节流中的每一个字节都按顺序编号。整个要传送的字节流的起始序号必须在连接建立时设置。首部中的序号字段值则指的是本报文段所发送的数据的第一个字节的序号。例如，一报文段的序号字段值是301，而携带的数据共有100字节。这就表明：本报文段的数据的第一个字节的序号是301，最后一个字节的序号是400。显然，下一个报文段（如果还有的话）的数据序号应当从401开始，即下一个报文段的序号字段值应为401。

3. **确认号 (ack)**

   占4字节，是期望收到对方下一个报文段的第一个数据字节的序号。例如，B正确收到了A发送过来的一个报文段，其序号字段值是501，而数据长度是200字节（序号501～700），这表明B正确收到了A发送的到序号700为止的数据。因此，B期望收到A的下一个数据序号是701，于是B在发送给A的确认报文段中把确认号置为701。总之，应当记住：若确认号 = N，则表明：到序号N - 1为止的所有数据都已正确收到。由于序号字段有32位长，可对4 GB（即4千兆字节）的数据进行编号。在一般情况下可保证当序号重复使用时，旧序号的数据早已通过网络到达终点了。

4. 数据偏移

   占4位，它指出TCP报文段的数据起始处距离TCP报文段的起始处有多远。这个字段实际上是指出TCP报文段的首部长度。由于首部中还有长度不确定的选项字段，因此数据偏移字段是必要的。但应注意，“数据偏移”的单位是32位字（即以4字节长的字为计算单位）。由于4位二进制数能够表示的最大十进制数字是15，因此数据偏移的最大值是60字节，这也是TCP首部的最大长度（即选项长度不能超过40字节）。

5. 保留

   占6位，保留为今后使用，但目前应置为0。

6. 紧急URG (URGent) 

   当URG = 1时，表明紧急指针字段有效。它告诉系统此报文段中有紧急数据，应尽快传送(相当于高优先级的数据)，而不要按原来的排队顺序来传送。

7. **确认ACK (ACKnowlegment)** 

   仅当ACK = 1时确认号字段(ack)才有效。当ACK = 0时，确认号无效。TCP规定，在连接建立后所有传送的报文段都必须把ACK置1。

8. 推送 PSH (PuSH)

   当两个应用进程进行交互式的通信时，有时在一端的应用进程希望在键入一个命令后立即就能够收到对方的响应。在这种情况下，TCP就可以使用推送(push)操作。这时，发送方TCP把PSH置1，并立即创建一个报文段发送出去。接收方TCP收到PSH = 1的报文段，就尽快地（即“推送”向前）交付接收应用进程，而不再等到整个缓存都填满了后再向上交付。虽然应用程序可以选择推送操作，但推送操作还很少使用。

9. 复位RST (ReSeT)

   当RST = 1时，表明TCP连接中出现严重差错（如由于主机崩溃或其他原因），必须释放连接，然后再重新建立运输连接。RST置1还用来拒绝一个非法的报文段或拒绝打开一个连接。RST也可称为重建位或重置位。

10. **同步SYN (SYNchronization)** 

    在连接建立时用来同步序号。当SYN = 1而ACK = 0时，表明这是一个连接请求报文段。对方若同意建立连接，则应在响应的报文段中使SYN = 1和ACK =1。因此，**SYN置为1就表示这是一个连接请求或连接接受报文**。

11. **终止FIN**

    用来释放一个连接。**当FIN = 1时，表明此报文段的发送方的数据已发送完毕，并要求释放运输连接。**

12. 窗口

    占2字节。窗口值是[0, 2^16 - 1]之间的整数。窗口指的是发送本报文段的一方的接收窗口（而不是自己的发送窗口）。窗口值告诉对方：从本报文段首部中的确认号算起，接收方目前允许对方发送的数据量。之所以要有这个限制，是因为接收方的数据缓存空间是有限的。总之，窗口值作为接收方让发送方设置其发送窗口的依据。窗口值是经常在动态变化着。

13. 检验和

    占2字节。检验和字段检验的范围包括首部和数据这两部分。

14. 紧急指针

    占2字节。紧急指针仅在URG = 1时才有意义，它指出本报文段中的紧急数据的字节数（紧急数据结束后就是普通数据）。

15. 选项

    长度可变，最长可达40字节。当没有使用“选项”时，TCP的首部长度是20字节。TCP最初只规定了一种选项，即最大报文段长度 MSS。后续增加了时间戳选项。

#### MSS 最大报文段长度

MSS是每一个TCP报文段中的数据字段的最大长度。数据字段加上TCP首部才等于整个的TCP报文段。所以MSS并不是整个TCP报文段的最大长度，而是“TCP报文段长度减去TCP首部长度”。

为什么要规定一个最大报文段长度MSS呢？这并不是考虑接收方的接收缓存可能放不下TCP报文段中的数据。实际上，MSS与接收窗口值没有关系。我们知道，TCP报文段的数据部分，至少要加上40字节的首部，才能组装成一个IP数据报。若选择较小的MSS长度，网络的利用率就降低。设想在极端的情况下，当TCP报文段只含有1字节的数据时，在IP层传输的数据报的开销至少有40字节(包括TCP报文段的首部和IP数据报的首部)。这样，对网络的利用率就不会超过1/41。到了数据链路层还要加上一些开销。但反过来，若TCP报文段非常长，那么在IP层传输时就有可能要分解成多个短数据报片。在终点要把收到的各个短数据报片装配成原来的TCP报文段。当传输出错时还要进行重传。这些也都会使开销增大。

因此，MSS应尽可能大些，只要在IP层传输时不需要再分片就行。由于IP数据报所经历的路径是动态变化的，因此在这条路径上确定的不需要分片的MSS，如果改走另一条路径就可能需要进行分片。因此最佳的MSS是很难确定的。在连接建立的过程中，双方都把自己能够支持的MSS写入这一字段，以后就按照这个数值传送数据，两个传送方向可以有不同的MSS值。若主机未填写这一项，则MSS的默认值是536字节长。因此，所有在因特网上的主机都应能接受的报文段长度是536 + 20= 556字节。

#### 时间戳

占10字节，其中最主要的字段为**时间戳值字段**（4字节）和**时间戳回送回答字段**（4字节）。

时间戳选项有以下两个功能：

1. 用来计算往返时间RTT。发送方在发送报文段时把当前时钟的时间值放入时间戳字段，接收方在确认该报文段时把时间戳字段值复制到时间戳回送回答字段。因此，发送方在收到确认报文后，可以准确地计算出RTT来。
2. 用于处理TCP序号超过2^32的情况，这又称为防止序号绕回 PAWS (Protect AgainstWrapped Sequence numbers)。我们知道，序号只有32位，而每增加2^32个序号就会重复使用原来用过的序号。当使用高速网络时，在一次TCP连接的数据传送中序号很可能会被重复使用。例如，若用1Gb/s的速率发送报文段，则不到35秒钟数据字节的序号就会重复。为了使接收方能够把新的报文段和迟到很久的报文段区分开，可以在报文段中加上这种时间戳。

### TCP和UDP的区别

1. TCP面向连接，UDP是无连接的；
2. TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付；Tcp通过校验和，重传控制，序号标识，滑动窗口、确认应答实现可靠传输。如丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。
3. UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。
4. 每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
5. TCP对系统资源要求较多，UDP对系统资源要求较少。

### 可靠控制

保证TCP的可靠性，通过**首部检验和、序列号、确认应答机制和超时重传机制**。

### 流量控制

保证TCP的流量控制，通过**停止等待、后退N帧和选择重传算法**。本质都是滑动窗口算法。

#### 停止等待算法

相当于发送窗口和接收窗口都为1的选择重传算法。

“停止等待”就是每发送完一个分组就停止发送，等待对方的确认。在收到确认后再发送下一个分组。

无差错时一切照常，出现差错时有以下考虑：

1. 接收方B在接受数据包M1时检测出了差错，就丢弃M1，其他什么也不做（不通知A收到有差错的分组）。
2. 也可能是M1在传输过程中丢失了，这时B当然什么都不知道。

在这两种情况下，B都不会发送任何信息。

可靠传输协议是这样设计的：A只要超过了一段时间仍然没有收到确认，就认为刚才发送的分组丢失了，因而重传前面发送过的分组。这就叫做**超时重传**。要实现超时重传，就要在每发送完一个分组设置一个超时计时器。如果在超时计时器到期之前收到了对方的确认，就撤销已设置的超时计时器。

这里应注意以下三点:

1. A在发送完一个分组后，必须暂时保留已发送的分组的副本（为发生超时重传时使用）。只有在收到相应的确认后才能清除暂时保留的分组副本。
2. 分组和确认分组都必须进行编号。这样才能明确是哪一个发送出去的分组收到了确认，而哪一个分组还没有收到确认。
3. 超时计时器设置的重传时间应当比数据在分组传输的平均往返时间更长一些。

确认丢失和确认迟到是另一种情况。B所发送的对M1的确认丢失了。A在设定的超时重传时间内没有收到确认，但并无法知道是自己发送的分组出错、丢失，或者是B发送的确认丢失了。因此A在超时计时器到期后就要重传M1。现在应注意B的动作。假定B又收到了重传的分组M1。这时应采取两个行动。

第一，丢弃这个重复的分组M1，不向上层交付。第二，向 A 发送确认。不能认为已经发送过确认就不再发送，因为A之所以重传M1就表示A没有收到对M1的确认。

这种可靠传输协议常称为**自动重传请求ARQ (Automatic Repeat reQuest)**。意思是重传的请求是自动进行的。接收方不需要请求发送方重传某个出错的分组。

#### 后退N帧算法

相当于接收窗口为1的选择重传算法。

在发送完一个数据报后，不用停下来等待确认，而是可以连续发送多个数据报。

如果前一个帧在超时时间内未得到确认，就认为丢失或被破坏，需要重发出错帧及其后面的所有数据帧。这样有可能有把正确的数据帧重传一遍，降低了传送效率。

#### 选择重传算法

也称滑动窗口算法。

连续ARQ协议规定，发送方每收到一个确认，就把发送窗口向前滑动一个分组的位置。

接收方一般都是采用累积确认的方式。这就是说，接收方不必对收到的分组逐个发送确认，而是在收到几个分组后，对按序到达的最后一个分组发送确认，这就表示：到这个分组为止的所有分组都已正确收到了。

累积确认有优点也有缺点。

优点是：容易实现，即使确认丢失也不必重传。

但缺点是不能向发送方反映出接收方已经正确收到的所有分组的信息。

### 拥塞控制

#### 慢开始与快恢复算法

发送方维持一个叫做拥塞窗口cwnd的状态变量。拥塞窗口的大小取决于网络的拥塞程度，并且动态地在变化。发送方让自己的发送窗口等于拥塞窗口，另外考虑到接受方的接收能力，发送窗口可能小于拥塞窗口。

慢开始算法的思路就是，不要一开始就发送大量的数据，先探测一下网络的拥塞程度，也就是说由小到大逐渐增加拥塞窗口的大小。

实时拥塞窗口大小是以字节为单位的。如下图：

![img](https://img-blog.csdn.net/20130801220358468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2ljb2ZpZWxk/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

一次传输轮次之后拥塞窗口就加倍。这就是乘法增长，和后面的拥塞避免算法的加法增长比较。

为了防止cwnd增长过大引起网络拥塞，还需设置一个慢开始门限ssthresh状态变量。ssthresh的用法如下：

* 当cwnd<ssthresh时，使用慢开始算法。
* 当cwnd>=ssthresh时，改用拥塞避免算法。

拥塞避免算法让拥塞窗口缓慢增长，即每经过一个往返时间RTT就把发送方的拥塞窗口cwnd加1，而不是加倍。这样拥塞窗口按线性规律缓慢增长。

无论是在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞（其根据就是没有收到确认，虽然没有收到确认可能是其他原因的分组丢失，但是因为无法判定，所以都当做拥塞来处理），就把慢开始门限设置为出现拥塞时的发送窗口大小的一半。然后把拥塞窗口设置为1，执行慢开始算法。如下图：

![img](https://img-blog.csdn.net/20130801220438375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2ljb2ZpZWxk/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

快恢复算法，在出现拥塞之后将拥塞窗口设置为出现拥塞时拥塞窗口的一半，而不是直接降为1.

![img](https://img-blog.csdn.net/20130801220615250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2ljb2ZpZWxk/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 快重传

快重传要求接收方在收到一个失序的报文段后就立即发出重复确认（为的是使发送方及早知道有报文段没有到达对方）而不要等到自己发送数据时捎带确认。快重传算法规定，发送方只要一连收到三个重复确认就应当立即重传对方尚未收到的报文段，而不必继续等待设置的重传计时器时间到期。

## 三次握手

三次握手其实就是指建立一个TCP连接时，需要客户端和服务器总共发送3个包。进行三次握手的主要作用就是为了确认双方的接收能力和发送能力是否正常、指定自己的初始化序列号为后面的可靠性传送做准备。实质上其实就是连接服务器指定端口，建立TCP连接，并同步连接双方的序列号和确认号，交换TCP窗口大小信息。

* 刚开始：客户端处于 Closed 的状态，服务端处于 Listen 状态。

* 第一次握手：客户端给服务端发一个 SYN 报文，并指明客户端的初始化序列号 ISN。此时客户端处于 SYN_SEND 状态。首部的同步位SYN=1，初始序号seq=x，SYN=1的报文段不能携带数据，但要消耗掉一个序号。
* 第二次握手：服务器收到客户端的 SYN 报文之后，会以自己的 SYN 报文作为应答，并且也是指定了自己的初始化序列号 ISN(s)。同时会把客户端的 ISN + 1 作为ACK 的值，表示自己已经收到了客户端的 SYN，此时服务器处于 SYN_REVD 的状态。在确认报文段中SYN=1，ACK=1，确认号ack=x+1，初始序号seq=y。
* 第三次握手：客户端收到 SYN 报文之后，会发送一个 ACK 报文，当然，也是一样把服务器的 ISN + 1 作为 ACK 的值，表示已经收到了服务端的 SYN 报文，此时客户端处于 ESTABLISHED 状态。服务器收到 ACK 报文之后，也处于 ESTABLISHED 状态，此时，双方已建立起了连接。确认报文段ACK=1，确认号ack=y+1，序号seq=x+1（初始为seq=x，第二个报文段所以要+1），ACK报文段可以携带数据，不携带数据则不消耗序号。

发送第一个SYN的一端将执行主动打开（active open），接收这个SYN并发回下一个SYN的另一端执行被动打开（passive open）。

在socket编程中，客户端执行connect()时，将触发三次握手。

![img](https://pic3.zhimg.com/v2-2a54823bd63e16674874aa46a67c6c72_b.jpg)

### 为什么需要三次握手

弄清这个问题，我们需要先弄明白三次握手的目的是什么，能不能只用两次握手来达到同样的目的。

第一次握手：客户端发送网络包，服务端收到了。这样服务端就能得出结论：客户端的发送能力、服务端的接收能力是正常的。

第二次握手：服务端发包，客户端收到了。这样客户端就能得出结论：服务端的接收、发送能力，客户端的接收、发送能力是正常的。不过此时服务器并不能确认客户端的接收能力是否正常。

第三次握手：客户端发包，服务端收到了。这样服务端就能得出结论：客户端的接收、发送能力正常，服务器自己的发送、接收能力也正常。

因此，需要三次握手才能确认双方的接收与发送能力是否正常。

试想如果是用两次握手，则会出现下面这种情况：

如客户端发出连接请求，但因连接请求报文丢失而未收到确认，于是客户端再重传一次连接请求。后来收到了确认，建立了连接。数据传输完毕后，就释放了连接，客户端共发出了两个连接请求报文段，其中第一个丢失，第二个到达了服务端，但是第一个丢失的报文段只是在某些网络结点长时间滞留了，延误到连接释放以后的某个时间才到达服务端，此时服务端误认为客户端又发出一次新的连接请求，于是就向客户端发出确认报文段，同意建立连接，不采用三次握手，只要服务端发出确认，就建立新的连接了，此时客户端忽略服务端发来的确认，也不发送数据，则服务端一致等待客户端发送数据，浪费资源。

### 半连接队列

服务器第一次收到客户端的 SYN 之后，就会处于 SYN_RCVD 状态，此时双方还没有完全建立其连接，服务器会把此种状态下请求连接放在一个队列里，我们把这种队列称之为半连接队列。

当然还有一个全连接队列，就是已经完成三次握手，建立起连接的就会放在全连接队列中。如果队列满了就有可能会出现丢包现象。

这里在补充一点关于SYN-ACK 重传次数的问题：

服务器发送完SYN-ACK包，如果未收到客户确认包，服务器进行首次重传，等待一段时间仍未收到客户确认包，进行第二次重传。如果重传次数超过系统规定的最大重传次数，系统将该连接信息从半连接队列中删除。每次重传等待的时间不一定相同，一般会是指数增长，例如间隔时间为 1s，2s，4s，8s…

### ISN(Initial Sequence Number)

当一端为建立连接而发送它的SYN时，它为连接选择一个初始序号。ISN随时间而变化，因此每个连接都将具有不同的ISN。ISN可以看作是一个32比特的计数器，每4ms加1 。这样选择序号的目的在于防止在网络中被延迟的分组在以后又被传送，而导致某个连接的一方对它做错误的解释。

三次握手的其中一个重要功能是客户端和服务端交换 ISN(Initial Sequence Number)，以便让对方知道接下来接收数据的时候如何按序列号组装数据。如果 ISN 是固定的，攻击者很容易猜出后续的确认号，因此 ISN 是动态生成的。

### 三次握手过程中可以携带数据吗？

其实第三次握手的时候，是可以携带数据的。但是，第一次、第二次握手不可以携带数据

为什么这样呢?大家可以想一个问题，假如第一次握手可以携带数据的话，如果有人要恶意攻击服务器，那他每次都在第一次握手中的 SYN 报文中放入大量的数据。因为攻击者根本就不理服务器的接收、发送能力是否正常，然后疯狂着重复发 SYN 报文的话，这会让服务器花费很多时间、内存空间来接收这些报文。

也就是说，第一次握手不可以放数据，其中一个简单的原因就是会让服务器更加容易受到攻击了。而对于第三次的话，此时客户端已经处于 ESTABLISHED 状态。对于客户端来说，他已经建立起连接了，并且也已经知道服务器的接收、发送能力是正常的了，所以能携带数据也没啥毛病。

### SYN攻击

服务器端的资源分配是在二次握手时分配的，而客户端的资源是在完成三次握手时分配的，所以服务器容易受到SYN洪泛攻击。

SYN攻击就是Client在短时间内伪造大量不存在的IP地址，并向Server不断地发送SYN包，Server则回复确认包，并等待Client确认，由于源地址不存在，因此Server需要不断重发直至超时，这些伪造的SYN包将长时间占用未连接队列，导致正常的SYN请求因为队列满而被丢弃，从而引起网络拥塞甚至系统瘫痪。SYN 攻击是一种典型的 DoS/DDoS 攻击。

检测 SYN 攻击非常的方便，当你在服务器上看到大量的半连接状态时，特别是源IP地址是随机的，基本上可以断定这是一次SYN攻击。

常见的防御 SYN 攻击的方法有如下几种：

- 缩短超时（SYN Timeout）时间
- 增加最大半连接数
- 过滤网关防护
- SYN cookies技术

## 四次挥手

建立一个连接需要三次握手，而终止一个连接要经过四次挥手。这是由TCP的半关闭（half-close）造成的。所谓的半关闭，其实就是TCP提供了连接的一端在结束它的发送后还能接收来自另一端数据的能力。

* 刚开始：双方都处于 ESTABLISHED 状态，假如是客户端先发起关闭请求。

* 第一次挥手：客户端发送一个 FIN 报文，报文中会指定一个序列号。此时客户端处于 FIN_WAIT1 状态。即发出连接释放报文段（FIN=1，序号seq=u），并停止再发送数据，主动关闭TCP连接，进入FIN_WAIT1（终止等待1）状态，等待服务端的确认。
* 第二次挥手：服务端收到 FIN 之后，会发送 ACK 报文，且把客户端的序列号值 +1 作为 ACK 报文的序列号值，表明已经收到客户端的报文了，此时服务端处于 CLOSE_WAIT 状态。即服务端收到连接释放报文段后即发出确认报文段（ACK=1，确认号ack=u+1，序号seq=v），服务端进入CLOSE_WAIT（关闭等待）状态，此时的TCP处于半关闭状态，客户端到服务端的连接释放。客户端收到服务端的确认后，进入FIN_WAIT2（终止等待2）状态，等待服务端发出的连接释放报文段。
* 第三次挥手：如果服务端也想断开连接了，和客户端的第一次挥手一样，发给 FIN 报文，且指定一个序列号。此时服务端处于 LAST_ACK 的状态。即服务端没有要向客户端发出的数据，服务端发出连接释放报文段（FIN=1，ACK=1，序号seq=w，确认号ack=u+1），服务端进入LAST_ACK（最后确认）状态，等待客户端的确认。
* 第四次挥手：客户端收到 FIN 之后，一样发送一个 ACK 报文作为应答，且把服务端的序列号值 +1 作为自己 ACK 报文的序列号值，此时客户端处于 TIME_WAIT 状态。需要过一阵子以确保服务端收到自己的 ACK 报文之后才会进入 CLOSED 状态，服务端收到 ACK 报文之后，就处于关闭连接了，处于 CLOSED 状态。即客户端收到服务端的连接释放报文段后，对此发出确认报文段（ACK=1，seq=u+1，ack=w+1），客户端进入TIME_WAIT（时间等待）状态。此时TCP未释放掉，需要经过时间等待计时器设置的时间2MSL后，客户端才进入CLOSED状态。

收到一个FIN只意味着在这一方向上没有数据流动。客户端执行主动关闭并进入TIME_WAIT是正常的，服务端通常执行被动关闭，不会进入TIME_WAIT状态。

在socket编程中，任何一方执行close()操作即可产生挥手操作。

![img](https://pic2.zhimg.com/v2-c7d4b5aca66560365593f57385ce9fa9_b.jpg)

### 为什么需要四次挥手？

握手时，当服务端收到客户端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。

但是关闭连接时，当服务端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉客户端，“你发的FIN报文我收到了”。只有等到我服务端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四次挥手。

### 2MSL等待状态

MSL是Maximum Segment Lifetime的英文缩写，可译为“最长报文段寿命”，它是任何报文在网络上存在的最长时间，超过这个时间报文将被丢弃。

TIME_WAIT状态也成为2MSL等待状态。每个具体TCP实现必须选择一个报文段最大生存时间MSL（Maximum Segment Lifetime），它是任何报文段被丢弃前在网络内的最长时间。这个时间是有限的，因为TCP报文段以IP数据报在网络内传输，而IP数据报则有限制其生存时间的TTL字段。

对一个具体实现所给定的MSL值，处理的原则是：当TCP执行一个主动关闭，并发回最后一个ACK，该连接必须在TIME_WAIT状态停留的时间为2倍的MSL。这样可让TCP再次发送最后的ACK以防这个ACK丢失（另一端超时并重发最后的FIN）。

这种2MSL等待的另一个结果是这个TCP连接在2MSL等待期间，定义这个连接的插口（客户的IP地址和端口号，服务器的IP地址和端口号）不能再被使用。这个连接只能在2MSL结束后才能再被使用。

为了保证客户端发送的最后一个ACK报文段能够到达服务器。因为这个ACK有可能丢失，从而导致处在LAST-ACK状态的服务器收不到对FIN-ACK的确认报文。服务器会超时重传这个FIN-ACK，接着客户端再重传一次确认，重新启动时间等待计时器。最后客户端和服务器都能正常的关闭。假设客户端不等待2MSL，而是在发送完ACK之后直接释放关闭，一但这个ACK丢失的话，服务器就无法正常的进入关闭连接状态。

两个理由：

1. 保证客户端发送的最后一个ACK报文段能够到达服务端。

   这个ACK报文段有可能丢失，使得处于LAST-ACK状态的B收不到对已发送的FIN+ACK报文段的确认，服务端超时重传FIN+ACK报文段，而客户端能在2MSL时间内收到这个重传的FIN+ACK报文段，接着客户端重传一次确认，重新启动2MSL计时器，最后客户端和服务端都进入到CLOSED状态，若客户端在TIME-WAIT状态不等待一段时间，而是发送完ACK报文段后立即释放连接，则无法收到服务端重传的FIN+ACK报文段，所以不会再发送一次确认报文段，则服务端无法正常进入到CLOSED状态。

2. 防止“已失效的连接请求报文段”出现在本连接中。

   客户端在发送完最后一个ACK报文段后，再经过2MSL，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失，使下一个新的连接中不会出现这种旧的连接请求报文段。

理论上，四个报文都发送完毕，就可以直接进入CLOSE状态了，但是可能网络是不可靠的，有可能最后一个ACK丢失。所以TIME_WAIT状态就是用来重发可能丢失的ACK报文。

## COOKIE、SESSION、TOKEN

很久很久以前，Web 基本上就是文档的浏览而已， 既然是浏览，作为服务器， 不需要记录谁在某一段时间里都浏览了什么文档，每次请求都是一个新的HTTP协议， 就是请求加响应， 尤其是我不用记住是谁刚刚发了HTTP请求， 每个请求对我来说都是全新的。

但是随着交互式Web应用的兴起，像在线购物网站，需要登录的网站等等，马上就面临一个问题，那就是要管理会话，必须记住哪些人登录系统， 哪些人往自己的购物车中放商品， 也就是说我必须把每个人区分开，这就是一个不小的挑战，因为HTTP请求是无状态的，所以想出的办法就是给大家发一个会话标识(session id), 说白了就是一个随机的字串，每个人收到的都不一样， 每次大家向我发起HTTP请求的时候，把这个字符串给一并捎过来， 这样我就能区分开谁是谁了。

这样每个人只需要保存自己的session id，而服务器要保存所有人的session id ！ 如果访问服务器多了， 就得由成千上万，甚至几十万个。这对服务器说是一个巨大的开销 ， 严重的限制了服务器扩展能力， 比如说我用两个机器组成了一个集群， 小F通过机器A登录了系统， 那session id会保存在机器A上， 假设小F的下一次请求被转发到机器B怎么办？ 机器B可没有小F的 session id啊。有时候会采用一点小伎俩： **session sticky** ， 就是让小F的请求一直粘连在机器A上， 但是这也不管用， 要是机器A挂掉了， 还得转到机器B去。

那只好做session 的复制了， 把session id 在两个机器之间搬来搬去， 快累死了。

![img](https://pic1.zhimg.com/v2-0820e192d7fed102203f131e5189cfb4_b.jpg)

后来有个叫Memcached的支了招： 把session id 集中存储到一个地方， 所有的机器都来访问这个地方的数据， 这样一来，就不用复制了， 但是增加了单点失败的可能性， 要是那个负责session 的机器挂了， 所有人都得重新登录一遍， 估计得被人骂死。

![img](https://pic1.zhimg.com/v2-81c6a9f3edade4b8e276d182fb335704_b.jpg)

也尝试把这个单点的机器也搞出集群，增加可靠性， 但不管如何， 这小小的session 对我来说是一个沉重的负担。

于是有人就一直在思考， 我为什么要保存这可恶的session呢， 只让每个客户端去保存该多好？

可是**如果不保存这些session id , 怎么验证客户端发给我的session id 的确是我生成的呢？** 如果不去验证，我们都不知道他们是不是合法登录的用户， 那些不怀好意的家伙们就可以伪造session id , 为所欲为了。

嗯，对了，**关键点就是验证** ！

比如说， 小F已经登录了系统， 我给他发一个令牌(token)， 里边包含了小F的 user id， 下一次小F 再次通过Http 请求访问我的时候， 把这个token 通过Http header 带过来不就可以了。

不过这和session id没有本质区别啊， 任何人都可以可以伪造， 所以我得想点儿办法， 让别人伪造不了。

那就对数据做一个签名吧， 比如说我用HMAC-SHA256 算法，加上一个只有我才知道的密钥， 对数据做一个签名， 把这个签名和数据一起作为token ， 由于密钥别人不知道， 就无法伪造token了。

![img](https://pic4.zhimg.com/v2-0d0a12e065ec8a09137d01192cce875f_b.jpg)

这个token 我不保存， 当小F把这个token 给我发过来的时候，我再用同样的HMAC-SHA256 算法和同样的密钥，对数据再计算一次签名， 和token 中的签名做个比较， 如果相同， 我就知道小F已经登录过了，并且可以直接取到小F的user id , 如果不相同， 数据部分肯定被人篡改过， 我就告诉发送者： 对不起，没有认证。

![img](https://pic3.zhimg.com/v2-ed6c8498fd74cc60f16f897922ee040e_b.jpg)

Token 中的数据是明文保存的， 还是可以被别人看到的， 所以我不能在其中保存像密码这样的敏感信息。

当然， 如果一个人的token 被别人偷走了， 那我也没办法， 我也会认为小偷就是合法用户， 这其实和一个人的session id 被别人偷走是一样的。

这样一来， 我就不保存session id 了， 我只是生成token , 然后验证token ， 我用我的CPU计算时间换取了我的session 存储空间 ！

解除了session id这个负担， 可以说是无事一身轻， 我的机器集群现在可以轻松地做水平扩展， 用户访问量增大， 直接加机器就行。 这种无状态的感觉实在是太好了！

### Cookie

cookie 是一个非常具体的东西，指的就是浏览器里面能永久存储的一种数据，仅仅是浏览器实现的一种数据存储功能。

cookie由服务器生成，发送给浏览器，浏览器把cookie以<k,v>形式保存到某个目录下的文本文件内，下一次请求同一网站时会把该cookie发送给服务器。由于cookie是存在客户端上的，所以浏览器加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间，所以每个域的cookie数量是有限的。

### Session

session 从字面上讲，就是会话。这个就类似于你和一个人交谈，你怎么知道当前和你交谈的是张三而不是李四呢？对方肯定有某种特征（长相等）表明他就是张三。

session 也是类似的道理，服务器要知道当前发请求给自己的是谁。为了做这种区分，服务器就要给每个客户端分配不同的“身份标识”，然后客户端每次向服务器发请求的时候，都带上这个“身份标识”，服务器就知道这个请求来自于谁了。至于客户端怎么保存这个“身份标识”，可以有很多种方式，对于浏览器客户端，大家都默认采用 cookie 的方式。

服务器使用session把用户的信息临时保存在了服务器上，用户离开网站后session会被销毁。这种用户信息存储方式相对cookie来说更安全，可是session有一个缺陷：如果web服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失。

使用session的问题：

1. **资源消耗：**每次认证用户发起请求时，服务器需要去创建一个记录来存储信息。当越来越多的用户发请求时，内存的开销也会不断增加。
2. **可扩展性：**在服务端的内存中使用Session存储登录信息，伴随而来的是可扩展性问题。
3. **CORS(跨域资源共享)：**当我们需要让数据跨多台移动设备上使用时，跨域资源的共享会是一个让人头疼的问题。在使用Ajax抓取另一个域的资源，就可以会出现禁止请求的情况。
4. **CSRF(跨站请求伪造)：**用户在访问银行网站时，他们很容易受到跨站请求伪造的攻击，并且能够被利用其访问其他的网站。

在这些问题中，可扩展行是最突出的。因此我们有必要去寻求一种更有行之有效的方法。

### Token

在Web领域基于Token的身份验证随处可见。在大多数使用Web API的互联网公司中，tokens 是多用户下处理认证的最佳方式。

使用基于Token的身份验证的优点：

1. 无状态、可扩展
2. 支持移动设备
3. 跨程序调用
4. 安全

基于Token的身份验证是无状态的，我们不将用户信息存在服务器或Session中。

基于Token的身份验证的过程如下:

1. 用户通过用户名和密码发送请求。
2. 程序验证。
3. 程序返回一个签名的token 给客户端。
4. 客户端储存token,并且每次用于每次发送请求。
5. 服务端验证token并返回数据。

每一次请求都需要token。token应该在HTTP的头部发送从而保证了Http请求无状态。我们同样通过设置服务器属性Access-Control-Allow-Origin:* ，让服务器能接受到来自所有域的请求。

需要注意的是，在ACAO头部标明(designating)*时，不得带有像HTTP认证，客户端SSL证书和cookies的证书。