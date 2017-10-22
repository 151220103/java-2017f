
## Error Handling with Exceptions

#### "Badly formed code will not be run."

---

## Outline

<br/>
- Concepts
- Java Exception Mechanism
- Exception Guidelines
- Design by Contract vs. Exceptions 

---

## Software Reliability

<br/>
- Correctness 正确性
  + 依据规约，完成任务
- Robustness 鲁棒性
  + 异常情况，合理反应
- Integrity  完整性
  + 非法访问或修改，合理反应

---

## Design by Contract

<br/>

- 方法学层面的思想：以尽可能小的代价开发出可靠性出众的软件

- 可以贯穿于软件创建的全过程，从分析到设计，从文档到调试，甚至可以渗透到项目管理中


<br/>
> <small>Bertrand Meyer：DbC是构建面向对象软件系统方法的核心。</small>
<br/>
> <small>James McKim：“只要你会写程序，你就会写契约。”</small>

---

## Contract

<br/>
- Every software element is intended to satisfy a certain <font color=red>goal</font>, for the benefit of other software elements (and ultimately of human users). 
- This goal is the element’s <font color=red>contract</font>.  契约
  + The contract of any software element should be
<font color=red>Explicit</font>. 显式
  + Part of the software element itself.

---

## A view of software construction

<br/>
- Constructing systems as structured collections of cooperating software elements — <font color=red>suppliers</font> and <font color=red>clients</font> — cooperating on the basis of clear definitions of <font color=red>obligations</font> and <font color=red>benefits</font>.		
- These definitions are the contracts.

---

## 葫芦娃的契约

<br/>

|   | 义务| 权益|
| - | - | - |
| Client  | 不能混进蛇精蝎子精  | 由小到大给葫芦娃们排序 |
| Supplier| 帮葫芦娃们由小到大排序| 只排葫芦娃 |


---

## Properties of Contracts

<br/>
- Binds two parties (or more): supplier, client. 绑定双方或多方
- Is explicit (written). 显式的
- Specifies mutual obligations and benefits. 规定相互的义务和权益
- Usually maps obligation for one of the parties into benefit for the other, and conversely. 一方的义务对应另一方的权益，反之亦然

---

## Contracts

<br/>
- 契约就是<font color=red>“规范和检查”</font>！
  + Precondition：针对method，它规定了在调用该方法之前必须为真的条件
  + Postcondition：针对method，它规定了方法顺利执行完毕之后必须为真的条件
  + Invariant：针对整个类，它规定了该类任何实例调用任何方法都必须为真的条件

---

## 再看葫芦娃的契约

<br/>

|   | 义务| 权益 |
| - |-| -|
| Client   | "Satisfy Preconditon": 只排葫芦娃   | "From Postcondition": 葫芦娃们由小到大给排好序 |
| Supplier | "Satisfy Postconditon": 葫芦娃们由小到大排好序| "From Precondition":只排葫芦娃          |


---

## Exception Handling and DbC

<br/>
- Exceptions are about dealing with things going wrong at runtime.

- DbC is about statically defining the conditions under which code is supposed to operate. 

---

## What are Exceptions?

- Many “exceptional” things can happen during the running of a program, e.g.:
  + <font color=green>User mis-types input</font>
  + <font color=green>Web page not available</font>
  + <font color=green>File not found</font>
  + <font color=yellow>Array index out of bounds</font>
  + <font color=yellow>Method called on a null object</font>
  + <font color=pink>Out of memory</font>
  + <font color=pink>Bug in the actual language implementation</font>

<span style="color:red">Exceptions are unexpected conditions in programs.</span><!-- .element: class="fragment" -->

---

## What do we want of exceptions?

- Ideally, a language (and its implementation) should:

  +  <font color=red>Restrict</font> the set of possible exceptions to “reasonable” ones.
  + Indicate <font color=red>where</font> they happened, and <font color=red>distinguish</font> between them.
  + Allow exceptions to be dealt with in a <font color=red>different</font> place in the code from where they occur.
  + so we <font color=yellow>throw</font> exceptions where they <font color=red>occur</font>, and <font color=yellow>catch</font> them where we want to <font color=red>deal with</font> them.

<span style="color:#0099ff">Ideally, we don't want non-fatal exceptions to be thrown too far — this breaks up the modularity of the program and makes it hard to reason about.</span><!-- .element: class="fragment" -->

---

## Exceptions in Java

- If a thrown exception is <font color=red>not</font> caught, it <font color=red>propagates out</font> to the caller and so on until <font color=yellow>**main**</font>. 

- If it is <font color=red>never</font> caught, it <font color=red>terminates</font> the program.

- If a method can generate (checked) exceptions but does <font color=red>not</font> handle them, it has to explicitly <font color=red>declare</font> that it throws them so that clients know what to expect.

---

## Exceptions in Java

<br/>
- In Java, the basic exception handling construct is to:
  + <font color=red>try</font>: a block of code which normally executes ok
  + <font color=red>catch</font>: any exceptions that it generates, and
  + <font color=red>finally</font>: do anything we want to do irrespective of what happened before. 


---

## Catch Exceptions

<br/>
```java
try{
  //Code that might generate exceptions
}
catch (ExceptionType1 e1){
  //Handle exceptions of ExceptionType1
}
catch (ExceptionType2 e2){
  //Handle exceptions of ExceptionType2
}
catch (ExceptionType3 e3){
  //Handle exceptions of ExceptionType3
}
finally{
  //Activities that happen every time
}
```

<span style="color:#0099ff">Termination vs. Resumption</span><!-- .element: class="fragment" -->

---

## Report Exceptions

<br/>
```java
public boolean method(int x) throws MyException{
    //some code
    throw new MyException(...);
}
```

<span style="color:#0099ff">Exception Propagation</span><!-- .element: class="fragment" -->

---

## Create Exceptions

<br/>
```java
class SimpleException extends Exception{}

public class InheritingExceptions {
  public void f() throws SimpleException {
    System.out.println("Throw SimpleException from f()");
    throw new SimpleException();
  }

  public static void main(String[] args){
    InheritingExceptions sed = new InheritingExceptions();
    try{
      sed.f();
    } catch(SimpleException e){
      System.out.println("Caught it!");
    }
  }
}
```

---

## Exceptions and Logging

- 1: Send error output to the *standard error* stream by writing to **System.err**.

```java
class MyException extends Exception{
  public MyException(){}
  public MyException(String msg){super(msg);}
}

public class FullConstructors {
  public static void f() throws MyException{
    System.out.println("Throwing MyException from f()");
    throw new MyException();
  }
  public static void g() throws MyException{
     System.out.println("Throwing MyException from g()");
    throw new MyException("Originated in g()");
  }

  public static void main(String[] args){
    try{
      f();
    } catch(MyException e){
      e.pringStackTrace(System.out);
    }
    try{
      g();
    } catch(MyException e){
      e.printStackTrace(System.out);
    }
  }
}
```
<span style="color:red">e.printStackTrace()</span><!-- .element: class="fragment" -->

---

## Exception and Logging

- 2: Log the output using the **java.util.logging** facility.

```java
import java.util.logging.*;
import java.io.*;

class LoggingException extends Exception{
  private static Logger logger = Logger.getLogger("LoggingException");
  public LoggingException(){
    StringWriter trace = new StringWriter();
    printStackTrace(new PrintWriter(trace));
    logger.severe(trace.toString());
  }
}

public class LoggingExceptions{
  public static void main(String[] args){
    try{
      throw new LoggingException();
    } catch(LoggingException e){
      System.err.println("Caught "+e);
    }
  }
}


```

---

# END