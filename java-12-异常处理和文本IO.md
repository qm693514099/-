# 第12章：异常处理和文本I/O

## 异常处理
* 不应该由方法来终止程序——应该由调用者决定是否可以终止一个程序
* 方法应当如何通知他的调用者一个异常产生了呢？   **Java**可以让方法抛出一个异常，该异常可以被调用者捕获和处理。
* throw new ArithmetricException("**Divisor cannot be zero**");   加粗部分为 Detail message（描述异常的消息），无实际功能,因为此类异常在定义时便只有一个参数String。
* 异常（Exception）
* 调用方法的语句包含在一个try块和一个catch块中。try块中的代码包含了正常情况下执行的代码。异常被catch所捕获。catch中的代码被执行以处理异常。
* 异常就是一个从异常类所创建的对象。异常的根类是java.lang.Throwable.所有的Java异常类都直接或者间接地继承自Throwable。***可以通过继承Exception或者Exception的子类来创建自己的异常类***
* 分为免检异常和**必检异常**（也称**可查异常**），必检异常是说编译器会强制程序员检查并通过catch块处理他们，或者在方法头进行声明。
* 捕获异常即try-catch语句   
* 声明异常 `puvlic void method2() throws Exception{ any code  }`   其中的throw**s**即用于声明异常，声明异常来表明方法**可能会**抛出的异常。如果方法可能会抛出多个异常，就可以在关键字throws后添加一个用逗号分隔的异常列表  
`public void myMethod() throws Exception1,Exception2,Exception3,....,ExceptionN` 
* **如果父类中的方法没有声明异常，那么在子类重写时就不能声明异常**
* 抛出异常 `throw new Exception（）`     其中throw即用于抛出异常,抛出异常有两种表达形式：
```
 IllegalArgumentException ex = 
  new IllegalArgumentException("Wrong Argument");
 throw ex;
 
```
 `throw new IllegalArgumentException("Wrong Argument");`
 * ***JavaAPI中的每个异常类至少有两个构造方法： 一个无参构造方法和一个带有可以描述这个异常的String参数的构造方法***
 * 捕获异常  利用**调用栈**   一直向上查，**上层方法引用下层方法**，如果最终异常类型没有被捕获，则程序终止。
 * 各种异常类可以从一个共同的父类中派生。如果一个catch块可以捕获一个父类的异常对象，它就能捕获那个父类的所有子类的异常对象。只是判断应先判断是否是子类专属的异常类型。
 * 从JDK7开始一个catch就可以捕获多种异常
 ```
 catch (Exception1|Exception2|Exception3|...|Exceptionk ex){
      some code for handling these Exception
 }
 ```
* 1.try-catch:可以对异常进行处理，提高软件的健壮性  2.JVM：运行错误则直接崩溃
* 异常处理器使用System.out.println(ex)打印一个有关异常的短消息ex.toString().
* 如果try模块中发生异常，try模块剩余代码将不会被执行；如果catch模块中发生了异常，catch模块剩余代码将不会被执行，而是跳过catch模块去执行后面的部分。
* **finally语句** 在任何情况下，finally块中的代码都会执行，不论try块中死否出现异常或者被捕获。***即使在到达finally块之前有一个return语句，finally块还是会执行***   ***使用finally块时可以略去catch块***。
* 在代码中，应该在什么时候使用try-catch语句呢？ 当必须处理不可预料的错误状况时应该使用它。不要使用try-catch语句做简单的逻辑测试，不要用于处理简单可预料的情况。
* **重新抛出异常**如果异常处理器并不能处理一个异常，或者只是简单地希望它的调用者注意到该异常，Java允许该异常处理器重新抛出异常。 如下面的语句throw ex重新抛出异常给调用者，以便调用者的其他处理器获得处理异常ex的机会。
```
try{
 statements;
 }
 catch (TheException ex){
  perform operations before exists;
  throw ex;
 }
 ```
 *  **链式异常**：与另一个异常一起抛出一个异常，构成了链式异常。上面的代码块例子**只是**重新抛出了最初的异常。链式异常是将新异常（带有附加信息）和最初异常一起抛出。
 * 程序闪退，就是异常处理做的不好。
 
## File类
### 使用PrintWritter进行写文件
* 凡是设计IO的都要进行强制进行异常声明  public static void main(String[] args) throws java.io.IOException {。
* 绝对文件名：包含路径在内  平常所说文件名皆为相对文件名。
* Windows中目录分隔符为“\”,而在java中分隔符用“\\”,而在创建过程中new file("image/us.gif"),时使用的“/”分隔符。
* 在创建文件过程中，尽量不要使用绝对文件名作为创建文件的名字，因为这样违背了JVM的各种平台的可适用性，这样**只能在windows上运行**。
* 构建一个File实例并不会在机器上创建一个文件，不管文件是否存在，都可以创建任意文件名的File实例。可以调用File实例判断文件是否存在。
* 必须使用close（）方法关闭文件。如果没有调用该方法，数据就不能正确的保存在文件中。
```
File file = new File("Score.txt");
		if(file.exists()){
			System.out.println("File is already exists");
			System.exit(1);
		}
		
		PrintWriter output = new PrintWriter(file);
		
		output.print("John T Smith");
		output.println(90);
		output.print("Eric K Jones ");
		output.println(85);
		
		output.close();
```
* 对比：
```
import java.util.Scanner;
Scanner input = new Scanner(System.in);
int x = input.nextInt();
```
```
import java.io.PrintWriter;
import java.io.File;
File file = new File("scores.txt");
PrintWriter output = new PrintWriter(filename);
output.print("xxx");
output.close();
```
* 利用try-with-resource可以自动关闭文件，省略了file.close();
```
//运用close（）
PrintWriter output = new PrintWriter(file);		
output.print("John T Smith");	
output.close();
```
* 没有必要关闭输入文件，但是这是一种释放被文件占用的资源的好办法。

```
运用try（声明和创建资源）{
  使用资源处理文件
 }
try(PrintWriter output = new PrintWriter(file);){
	output.print("lalla");
}
```
### 利用Scanner进行读文件
* 对比：
```
//从键盘读取
Scanner input = new Scanner(System.in);
```
```
//从文件读取
Scanner input = new Scanner(new File(filename));
```
```
//从网页上读取
Scanner input = new Scanner(url.openStream());
```
* delimiter分隔符
* next()方法读取一个由分隔符分隔的字符串，但是nextLine（）读取的是一个以换行符结束的行。
* 不应该在一个基于标记的输入之后再使用一个基于行的输入。如果输入34，再回车，再输入567，则intValue为34，line只得到了一个回车符。
```
Scanner input = new Scanner(System.in);
int intValue = input.nextInt();
String line = input.nextLine();
```
* 下面程序注意创建新的文本文档的代码是PrintWriter output = new PrintWriter(targetFile);
* 还有一点非常重要，就是对String s1 = input.nextLine();
String s2 = s1.replaceAll(args[2], args[3]);两行代码的理解，s1读取源文件中的一行，replaceAll(args[2], args[3])表示将源文件的当前读取行的args[2]子字符串被替换为args[3]新的字符串为s2.若s1中无args[2]子字符串，则为将源文件一模一样的复制到目标文件。
* 下面问题还得出一件事：如args[n]数组中的任何一个元素都可以表示文件。.
```
//ReplaceText问了方宇聪海
import java.io.*;
import java.util.*;
public class ReplaceText {

	public static void main(String[] args) throws Exception{
		// TODO Auto-generated method stub
		if(args.length != 4){
			System.out.println("Usage: java ReplaceText sourceFile targetFile oldStr newStr");
			System.exit(1);
		}
		
		File sourceFile = new File(args[0]);
		if(!sourceFile.exists()){
			System.out.println("Sourcefile " + args[0] + "already exists");
			System.exit(2);
		}
		
		File targetFile = new File(args[1]);
		if(targetFile.exists()){
			System.out.println("targetfile " + args[1] + "already exists");
			System.exit(3);
		}
		
		try(
				Scanner input = new Scanner(sourceFile);
				PrintWriter output = new PrintWriter(targetFile);
		)
	{
			while(input.hasNext()){
				String s1 = input.nextLine();
				String s2 = s1.replaceAll(args[2], args[3]);
				output.println(s2);
			}
		}
	}

}
```
#### 从Web上读取数据
* 需要使用java.net.URL构造方法
* 输入URL必须输入http:**//**(双斜杠)
* 网页上读入  Scanner input = new Scanner(url.openStream());
* 可能发生的两种异常：1.MalformedURLException（URL 协议格式或者路径错误  或者解析错误） 2.IOException



 
 
 ### HOW2J.com  异常
 * try-catch异常捕捉机制提高了程序的健壮性，以前所见过的程序闪退即是这方面做的不好。
 * 步骤二：罗列到目前为止所接触过的所有异常(面试常问)： 数组超限，输入类型不匹配，待打开的文件不存在，做除法除数为零。
 1. OutOfIndexException 数组下标越界异常
 2. OutOfMemoryError  内存不足（非必检）—— 默认情况下java程序启动最大可以用16M的内存。
 3. ClassCastException 类型转换异常
 4. ArithmeticException 除数为零（非必检）
 5. NullPointerException 空指针异常（非必检）
 6. ArrayIndexOutOfBoundsException 下标越界异常（非必检）
 6. MalformedURLException（URL 协议格式或者路径错误  或者解析错误）
 * try块里面都是可能会出错，需要加强健壮性的代码，通过catch捕捉。
 * **instanceof**是和**catch (FileNotFoundException | ParseException e)** 这类在一个catch中捕捉多种异常使用的，见代码如下

```
//未使用instanceof
catch (FileNotFoundException e) {
            System.out.println("d:/LOL.exe不存在");
            e.printStackTrace();
        } catch (ParseException e) {
            System.out.println("日期格式解析错误");
            e.printStackTrace();
```

```
//使用instanceof
 catch (FileNotFoundException | ParseException e) {
            if (e instanceof FileNotFoundException)
                System.out.println("d:/LOL.exe不存在");
            if (e instanceof ParseException)
                System.out.println("日期格式解析错误");
            e.printStackTrace();
 ```
 * throws与throw这两个关键字接近，不过意义不一样，有如下区别：
>> 1. throws 出现在方法声明上，而throw通常都出现在方法体内。
>> 2. throws 表示出现异常的一种可能性，并不一定会发生这些异常；throw则是抛出了异常，执行throw则一定抛出了某个异常对象。
* finally 常常用作数据库链接的关闭工作，保证了无论是否链接的上，都会最终将数据库链接进行关闭。
* throws将本地（method1（））产生的异常抛出去，哪个方法（method2（））来调用哪个方法负责处理异常。若有其他方法（main）调用处理异常的方法，则不会报异常，因为method2（）已经将异常“消化掉了”。
* 运行时异常 RuntimeException，能否被捕捉？  可以，即使无try——catch也可捕捉此异常。
* 错误Error，能否被捕捉？  可以，即使无try——catch也可捕捉此异常。 
* **面试常问**：运行时异常与非运行时异常的区别？      运行时异常即使无try——catch语句也可被捕捉（**免检异常**），但是非运行时异常有的必须要加上try——catch语句才可以捕捉异常。
* 免检异常包括：RuntimeException和Error——————其余为必检异常
```
public class Hero {
	public String name;
	protected int hp;
	
	public void attackHero(Hero h)throws EnemyHeroIsDeadException{
		if(h.hp == 0){
			throw new EnemyHeroIsDeadException (h.name + "已经挂了，不需要再释放技能了");
		}
	}
	public String toString(){
		return name;
	}
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Hero garen = new Hero();
		garen.hp = 616;
		garen.name = "盖伦";
		
		Hero teemo = new Hero();
		teemo.hp = 0;
		teemo.name = "提莫";
		
		try{
			garen.attackHero(teemo);
		}
		catch(EnemyHeroIsDeadException e){
			System.out.println("异常的具体原因： "+ e.getMessage());
			e.printStackTrace();
		}
	}
	class EnemyHeroIsDeadException extends Exception{
		public EnemyHeroIsDeadException(){
			super();
		}
		public EnemyHeroIsDeadException(String msg){
			super(msg);
		}
	}

}
```
* 上面的super（msg）是因为：可能方法是一层层调用的，上层方法可能想知道下层异常的具体信息，就用super(msg);
 
 
 
 
 
 
 
 
 
 
 
 
