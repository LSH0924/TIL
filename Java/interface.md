# 인터페이스 Interface
- SE8에서의 변화
    + static, 추상메소드 외에도 구체화 된 메소드를 넣을 수 있도록 변경.
    + default 메소드 사용가능 
- Interface 키워드 사용
- 인스턴스화 할 수 없으며, 오버라이드 혹은 구현해서 사용해야한다.
- 구현시 implements 키워드를, 상속시 extends 키워드를 사용.
- 정수로서의 변수를 선언할 수 있다. 이 때, 자동으로 public static final 이 부여된다.
- 정적인(static) 추상메소드는 선언할 수 없다. 
- 인터페이스에 선언된 메소드에는 자동으로 public abstract 가 부여된다. 


## 인터페이스의 default 메소드
- 지정할 수 있는 접근 제어자는 오직 public 뿐이다. 명시적으로 표기하지 않아도 암묵적으로 붙게 된다.
- Object 클래스가 제공하는 Object.equals(), Object.hashCode(), Object.toString()은 디폴트 메소드로 사용할 수 없다.
- default 메소드를 가지고있는 인터페이스를 구현하는 클래스에서 default 메소드를 오버라이드 해서 사용할 수 있다.
- 같은 이름의 default 메소드를 가지고 있는 인터페이스 두 개를 구현할 때는 default 메소드를 오버라이드 해주어야 한다.
    + 오버라이드 하지 않았을 때
        ```java
        interface interfaceTest { 
        default void method() {
            System.out.println("interfaceTest"); 
        }
        }
        interface interfaceTest2{
        default void method() {
            System.out.println("interfaceTest2"); 
        }
        }
        class A implements interfaceTest, interfaceTest2{}
        ```
        
        > ##### **컴파일에러**
        > 매개 변수 ()및 ()을 가지는 중복된 default 메소드 method는 인터페이스 interfaceTest2 및 interfaceTest에서 상속됩니다.

    + 오버라이드 했을 때
    + 같은 이름을 가진 default 메소드를 오버라이드 할 경우, 부르고싶은 인터페이스의 default 메소드임을 명확히 써야한다. 
    + `부르고싶은인터페이스.super.추상메소드명();`<br><br>
        ```java
        class A implements interfaceTest, interfaceTest2{
        @Override
        public void method() {
            interfaceTest.super.method();
            interfaceTest2.super.method();
        }
        }
        ```

        > ##### **실행 결과**
        > interfaceTest
        > interfaceTest2 


## implements와  extends의 우선순위

```java
interface methodA{ void method();}
interface X extends methodA { 
    default void method() {  
        System.out.println("X extends methodA");
    }
}
class Y implements methodA{
    @Override
    public void method() {
        System.out.println("Y implements methodA"); 
    }
}
class A extends Y implements X{
    public static void main(String[] args) {
        new A().method();
    }
}
```
> ##### **실행결과**
> Y implements methodA
> // methodA를 implements한 Y의 메소드가 우선적으로 호출된다. 
