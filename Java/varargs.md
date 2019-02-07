### 가변인자
- 필요에 따라 매개변수의 개수를 가변적으로 조절할 수 있다.
- 하나의 메소드에 하나의 가변인자만 사용할 수 있다.
- 다른 타입의 매개변수와 같이 사용할 수 있지만, 가변인자는 맨 마지막에 사용해야한다.
    ```java
    public static void varargs(int i, String ...strings) {
        System.out.println(i);
        for(String str : strings) {
            System.out.println(str);
        }
    } //정상 컴파일 됨
    ```

- 가변인자가 여러 개, 혹은 파라미터의 마지막에 위치하지 않을 때
    ```java
    public static void varargs(int ...i, String ...strings, long l) {
        System.out.println(i);
        for(String str : strings) {
            System.out.println(str);
        }
    }
    ```
    > ##### **컴파일에러**
    > The variable argument type String of the method varargs must be the last parameter
	> <br>The variable argument type int of the method varargs must be the last parameter


- 가변인자 부분의 매개변수를 입력하지 않아도 속이 빈 배열이 전달된다. 
- 가변인자 부분의 매개변수에 null을 입력했을 때는, 이 배열에 대한 임의의 처리를 하려는 순간 NullPointException이 발생한다.
- 참조형 타입의 가변인자에 null을 입력할 경우, 경고가 발생한다.  null 자체를 넘기고싶은지, 매개변수와 일치하는 참조형 데이터의 값을 null<sub>(이 경우, 배열로 넘어갈 때 {(String) null}로 넘어간다.)</sub>로 입력하고싶은지 확실하지 않기 때문이다.
    - 가변인자에 아무것도 넣지 않았을 때와 null을 넣었을 때(기본형 데이터타입)
        ```java
        public static void main(String[] args) {
            System.out.println(varargs(1,2,3,4,5));
            System.out.println("-----");
            System.out.println(varargs());
            System.out.println("-----");
            System.out.println(varargs(null));
        }
        public static String varargs(int ...iarr) {
            for(int i : iarr) {
                System.out.print(i + " ");
            }
            return "int Varargs parameter : " + iarr;
        }
        ```
        > ##### **실행결과**
        > 1 2 3 4 5 int Varargs parameter : [I@70dea4e 
        > <br>\----- 
        > <br>int Varargs parameter : [I@5c647e05
        > <br>\-----
        > <br>Exception in thread "main" java.lang.**NullPointerException**
        > <br>at testProject.AtomicTest.varargs(Test.java:13)
        > <br>at testProject.AtomicTest.main(Test.java:9) 

    - 가변인자에 아무것도 넣지 않았을 때와 null을 넣었을 때(참조형 데이터타입)
        ```java
        public static void main(String[] args) {
            System.out.println(varargs(null));
        }
        public static String varargs(String ...sarr) {
            for(String i : sarr) {
                System.out.print(i + " ");
            }
            return "String Varargs parameter : " + sarr;
        }
        ```
        > #### **경고발생**
        > Type null of the last argument to method varargs(String...) doesn't exactly match the vararg parameter type. Cast to String[] to confirm the non-varargs invocation, or pass individual arguments of type String for a varargs invocation.
        > <br>메소드varargs(String...)의 마지막 매개변수 null이, 가변인자 파라미터형과 완전히 일치하지 않습니다. 
        > <br>가변인자 호출을 확인하기 위해 String[]으로 캐스팅하거나, 가변인자 호출에 String형의 개별 매개변수를 전달합니다. 


#### **가변인자를 사용하는 메소드를 오버로드했을 때 호출시 우선순위**

 - 반환형, 매개변수가 일치하는 메소드 -> 암묵적 형변환 가능 -> AutoBoxing가능(Wrapper Class) -> 가변인자

- 각각의 메소드를 하나씩 주석처리 하면서 확인해보자.
    ```java
    public static void main(String[] args) {
        System.out.println(varargs(123456789));
    }
    public static String varargs(int i) {
        return "int parameter : " + i;
    } // 실행순위 1
    public static String varargs(long l) {
        return "long parameter : " + l;
    } //실행순위 2
    public static String varargs(Integer i) {
        return "Integer parameter : " + i;
    } //실행순위 3
    public static String varargs(int ...iarr) {
        for(int i : iarr) {
            System.out.print(i + " ");
        }
        return "Varargs parameter : " + iarr;
    } //실행순위 4
    ```

    
-----
### 참고
 - Java プログラマ Gold SE8
 - 직접 돌려봄
