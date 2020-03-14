代码规范来源：阿里巴巴JAVA开发手册
# (一) 命名规约

1.**【强制】**代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。
反例： _name / __name / $Object / name_ / name$ / Object$
2.**【强制】**代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。
说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式
也要避免采用。
反例： DaZhePromotion [打折] / getPingfenByName()  [评分] / int某变量 = 3
正例： alibaba / taobao / youku / hangzhou等国际通用的名称，可视同英文。
3.**【强制】**类名使用UpperCamelCase风格，必须遵从驼峰形式，但以下情形例外：（领域模型
的相关命名）DO / BO / DTO / VO等。
正例：MarcoPolo / UserDO / XmlService / TcpUdpDeal /   TaPromotion
反例：macroPolo / UserDo / XMLService / TCPUDPDeal /   TAPromotion
4.**【强制】**方法名、参数名、成员变量、局部变量都统一使用lowerCamelCase风格，必须遵从
驼峰形式。
正例： localValue / getHttpMessage() /  inputUserId
5.**【强制】**常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
正例： MAX_STOCK_COUNT
反例： MAX_COUNT
6.**【强制】**抽象类命名使用Abstract或Base开头；异常类命名使用Exception结尾；测试类
命名以它要测试的类的名称开始，以Test结尾。
7.**【强制】**中括号是数组类型的一部分，数组定义如下：String[]   args;
反例：请勿使用String  args[]的方式来定义。
8.**【强制】**POJO类中布尔类型的变量，都不要加is，否则部分框架解析会引起序列化错误。
反例：定义为基本数据类型boolean isSuccess；的属性，它的方法也是isSuccess()，RPC
框架在反向解析的时候，“以为”对应的属性名称是success，导致属性获取不到，进而抛出异
常。
9.**【强制】**包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用
单数形式，但是类名如果有复数含义，类名可以使用复数形式。
正例：应用工具类包名为com.alibaba.open.util、类名为MessageUtils（此规则参考
spring的框架结构）
10.**【强制】**杜绝完全不规范的缩写，避免望文不知义。
反例： AbstractClass“缩写”命名成AbsClass；condition“缩写”命名成 condi，此类
随意缩写严重降低了代码的可阅读性。
11.**【强制】**如果使用到了设计模式，建议在类名中体现出具体模式。
说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计思想。
正例：public class OrderFactory;
public class LoginProxy;
public class ResourceObserver;
12.**【强制】**接口类中的方法和属性不要加任何修饰符号（public也不要加），保持代码的简洁
性，并加上有效的Javadoc注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是
与接口方法相关，并且是整个应用的基础常量。
正例：接口方法签名：void f();
接口基础常量表示：String COMPANY = "alibaba";
反例：接口方法定义：public abstract void f();
说明：JDK8中接口允许有默认实现，那么这个default方法，是对所有实现类都有价值的默
认实现。
13.**【强制】**接口和实现类的命名有两套规则：
1）**【强制】**对于Service和DAO类，基于SOA的理念，暴露出来的服务一定是接口，内部
的实现类用Impl的后缀与接口区别。
正例：CacheServiceImpl实现CacheService接口。
2）**【强制】**如果是形容能力的接口名称，取对应的形容词做接口名（通常是–able的形式）。
正例：AbstractTranslator实现 Translatable。

# (二) 常量定义

1.**【强制】**不允许出现任何魔法值（即未经定义的常量）直接出现在代码中。
```
反例： String key="Id#taobao_"+tradeId；
cache.put(key,  value);
```
2.**【强制】**long或者Long初始赋值时，必须使用大写的L，不能是小写的l，小写容易跟数字1混淆，造成误解。
> 说明：Long a = 2l;写的是数字的21，还是Long型的2?

3.**【推荐】**不要使用一个常量类维护所有常量，应该按常量功能进行归类，分开维护。如：缓存相关的常量放在类：CacheConsts下；系统配置相关的常量放在类：ConfigConsts下。
> 说明：大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。

4.*【推荐】*常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包内共享常量、类内共享常量。
* 跨应用共享常量：放置在二方库中，通常是client.jar中的constant目录下。
* 应用内共享常量：放置在一方库的modules中的constant目录下。
	```
	反例：易懂变量也要统一定义成应用内共享常量，两位攻城师在两个类中分别定义了表示“是”的变量：
	类A中：public  static final String YES = "yes";
	类B中：public  static final String YES = "y";
	A.YES.equals(B.YES)，预期是true，但实际返回为false，导致产生线上问题。
	```
* 子工程内部共享常量：即在当前子工程的constant目录下。
* 包内共享常量：即在当前包下单独的constant目录下。
* 类内共享常量：直接在类内部private  static  final定义。
5.*【推荐】*如果变量值仅在一个范围内变化用Enum类。如果还带有名称之外的延伸属性，必须使用Enum类，下面正例中的数字就是延伸信息，表示星期几。


# (三) 格式规约

1.**【强制】**大括号的使用约定。如果是大括号内为空，则简洁地写成{}即可，不需要换行；如果是非空代码块则：
* 左大括号前不换行。
* 左大括号后换行。
* 右大括号前换行。
* 右大括号后还有else等代码则不换行；表示终止右大括号后必须换行。

2.**【强制】**左括号和后一个字符之间不出现空格；同样，右括号和前一个字符之间也不出现空格。详见第5条下方正例提示。
3.**【强制】**if/for/while/switch/do等保留字与左右括号之间都必须加空格。
4.**【强制】**任何运算符左右必须加一个空格。
> 说明：运算符包括赋值运算符=、逻辑运算符&&、加减乘除符号、三目运行符等。

5.**【强制】**缩进采用4个空格，禁止使用tab字符。
> 说明：如果使用tab缩进，必须设置1个tab为4个空格。IDEA设置tab为4个空格时，请勿勾选Use tab character；而在eclipse中，必须勾选insert   spaces for tabs。

```
正例：（涉及1-5点）
public static void main(String args[]) {
	//缩进4个空格
	String say = "hello";
	//运算符的左右必须有一个空格
	int flag = 0;
	//关键词if与括号之间必须有一个空格，括号内的f与左括号，0与右括号不需要空格
	if (flag == 0) {
		System.out.println(say);
	}
	//左大括号前加空格且不换行；左大括号后换行
	if (flag == 1) {
		System.out.println("world");
		//右大括号前换行，右大括号后有else，不用换行
	} else {
		System.out.println("ok");
		//在右大括号后直接结束，则必须换行
	}
}
```
6.**【强制】**单行字符数限制不超过  120个，超出需要换行，换行时遵循如下原则：
* 第二行相对第一行缩进   4个空格，从第三行开始，不再继续缩进，参考示例。
* 运算符与下文一起换行。
* 方法调用的点符号与下文一起换行。
* 在多个参数超长，逗号后进行换行。
* 在括号前不要换行，见反例。
```
正例：
StringBuffer sb = new StringBuffer();
//超过120个字符的情况下，换行缩进4个空格，并且方法前的点符号一起换行
sb.append("zi").append("xin")...
	.append("huang")...
	.append("huang")...
	.append("huang");
反例：
StringBuffer sb = new StringBuffer();
//超过120个字符的情况下，不要在括号前换行
sb.append("zi").append("xin")...append
	("huang");
//参数很多的方法调用可能超过120个字符，不要在逗号前换行
method(args1, args2, args3, ...
	, argsX);
```
7.**【强制】**方法参数在定义和传入时，多个参数逗号后边必须加空格。
```
正例：下例中实参的"a",后边必须要有一个空格。
method("a", "b", "c");
```
8.**【强制】**IDE的text   file encoding设置为UTF-8;  IDE中文件的换行符使用Unix格式，不要使用windows格式。
9.*【推荐】*没有必要增加若干空格来使某一行的字符与上一行的相应字符对齐。
```
正例：
int a = 3;
long b = 4L;
float c = 5F;
StringBuffer sb = new StringBuffer();
说明：增加sb这个变量，如果需要对齐，则给a、b、c都要增加几个空格，在变量比较多的
```
10.*【推荐】*方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。
> 说明：没有必要插入多行空格进行隔开。

# (四) OOP规约
1.**【强制】**避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成本，直接用类名来访问即可。

2.**【强制】**所有的覆写方法，必须加@Override注解。
```
反例：getObject()与get0bject()的问题。一个是字母的O，一个是数字的0，加@Override可以准确判断是否覆盖成功。另外，如果在抽象类中对方法签名进行修改，其实现类会马上编译报错。
```
3.**【强制】**相同参数类型，相同业务含义，才可以使用Java的可变参数，避免使用Object。
> 说明：可变参数必须放置在参数列表的最后。（提倡同学们尽量不用可变参数编程）

```
正例：public User getUsers(String type, Integer... ids)
```
4.**【强制】**对外暴露的接口签名，原则上不允许修改方法签名，避免对接口调用方产生影响。接口过时必须加@Deprecated注解，并清晰地说明采用的新接口或者新服务是什么。

5.**【强制】**不能使用过时的类或方法。
>说明：java.net.URLDecoder中的方法decode(StringencodeStr)这个方法已经过时，应该使用双参数decode(String source, String encode)。接口提供方既然明确是过时接口，那么有义务同时提供新的接口；作为调用方来说，有义务去考证过时方法的新实现是什么。

6.**【强制】**Object的equals方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals。
```
正例： "test".equals(object);
反例： object.equals("test");
说明：推荐使用java.util.Objects#equals（JDK7引入的工具类）
```
7.**【强制】**所有的相同类型的包装类对象之间值的比较，全部使用equals方法比较。
> 说明：对于Integer  var=?在-128至127之间的赋值，Integer对象是在IntegerCache.cache产生，会复用已有对象，这个区间内的Integer值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用equals方法进行判断。

8.**【强制】**关于基本数据类型与包装数据类型的使用标准如下：
* 所有的POJO类属性必须使用包装数据类型。
* RPC方法的返回值和参数必须使用包装数据类型。
* 所有的局部变量【推荐】使用基本数据类型。

> 说明：POJO类属性没有初值是提醒使用者在需要使用时，必须自己显式地进行赋值，任何NPE问题，或者入库检查，都由使用者来保证。

```
正例：数据库的查询结果可能是null，因为自动拆箱，用基本数据类型接收有NPE风险。
反例：比如显示成交总额涨跌情况，即正负x%，x为基本数据类型，调用的RPC服务，调用不成功时，返回的是默认值，页面显示：0%，这是不合理的，应该显示成中划线-。所以包装数据类型的null值，能够表示额外的信息，如：远程调用失败，异常退出。
```
9.**【强制】**定义DO/DTO/VO等POJO类时，不要设定任何属性默认值。
```
反例：POJO类的gmtCreate默认值为new   Date();但是这个属性在数据提取时并没有置入具体值，在更新其它字段时又附带更新了此字段，导致创建时间被修改成当前时间。
```
10.**【强制】**序列化类新增属性时，请不要修改serialVersionUID字段，避免反序列失败；如果完全不兼容升级，避免反序列化混乱，那么请修改serialVersionUID值。
> 说明：注意serialVersionUID不一致会抛出序列化运行时异常。

11.**【强制】**构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在init方法中。

12.**【强制】**POJO类必须写toString方法。使用IDE的中工具：source>   generate toString时，如果继承了另一个POJO类，注意在前面加一下super.toString。
> 说明：在方法执行抛出异常时，可以直接调用POJO的toString()方法打印其属性值，便于排查问题。

13.*【推荐】*使用索引访问用String的split方法得到的数组时，需做最后一个分隔符后有无内容的检查，否则会有抛IndexOutOfBoundsException的风险。
> 说明：
String str = "a,b,c,,";
String[] ary = str.split(",");
//预期大于3，结果是3
System.out.println(ary.length);

14.*【推荐】*当一个类有多个构造方法，或者多个同名方法，这些方法应该按顺序放置在一起，便于阅读。

15.*【推荐】*类内方法定义顺序依次是：公有方法或保护方法  >私有方法  > getter/setter方法。
> 说明：公有方法是类的调用者和维护者最关心的方法，首屏展示最好；保护方法虽然只是子类关心，也可能是“模板设计模式”下的核心方法；而私有方法外部一般不需要特别关心，是一个黑盒实现；因为方法信息价值较低，所有Service和DAO的getter/setter方法放在类体最后。

16.*【推荐】*setter方法中，参数名称与类成员变量名称一致，this.成员名=参数名。在getter/setter方法中，尽量不要增加业务逻辑，增加排查问题的难度。
```
反例：
public Integer getData(){
	if(true)  {
		return data + 100;
	} else {
		return data - 100;
	}
}
```

17.*【推荐】*循环体内，字符串的联接方式，使用StringBuilder的append方法进行扩展。
```
反例：
String str = "start";
for(int i=0; i<100; i++){
	str = str + "hello";
}
说明：反编译出的字节码文件显示每次循环都会new出一个StringBuilder对象，然后进行append操作，最后通过toString方法返回String对象，造成内存资源浪费。
```

18.*【推荐】*final可提高程序响应效率，声明成final的情况：
* 不需要重新赋值的变量，包括类属性、局部变量。
* 对象参数前加final，表示不允许修改引用的指向。
* 类方法确定不允许被重写。

19.*【推荐】*慎用Object的clone方法来拷贝对象。
> 说明：对象的clone方法默认是浅拷贝，若想实现深拷贝需要重写clone方法实现属性对象的拷贝。

20.*【推荐】*类成员与方法访问控制从严：
* 如果不允许外部直接通过new来创建对象，那么构造方法必须是private。
* 工具类不允许有public或default构造方法。
* 类非static成员变量并且与子类共享，必须是protected。
* 类非static成员变量并且仅在本类使用，必须是private。
* 类static成员变量如果仅在本类使用，必须是private。
* 若是static成员变量，必须考虑是否为final。
* 类成员方法只供类内部调用，必须是private。
* 类成员方法只对继承类公开，那么限制为protected。

> 说明：任何类、方法、参数、变量，严控访问范围。过宽泛的访问范围，不利于模块解耦。
