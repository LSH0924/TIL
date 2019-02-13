# 람다식(Lambda)

  > (구현하고자 하는 메소드의 매개변수) -> { 실행하는 처리 };
  
- 함수형 인터페이스는 추상메소드가 하나밖에 없고, 람다식을 통해 무슨 메소드인지 판단할 수 있을 때 사용한다.
- Supplier<T> 인터페이스의 메소드는 매개변수가 없다.
- 그 외는 보통 하나의 매개변수를 받는다. 두 개의 매개변수를 받는 메소드를 가진 인터페이스는 이름 앞에 Bi- 가 붙는다.
- 각 메소드의 반환형은 인터페이스의 제네릭스. 기본형을 제네릭스로 넘기고싶을 때는 Wrapper클래스를 사용하면 된다.


#### 람다식의 생략

- 람다식 우변의 생략
    > `(String str) ->` -> `(str) ->` -> `str ->`
    
    - 형식 추론 : 인터페이스에 선언되어 있는 메소드의 매개변수로 람다식의 매개변수 형식을 추론하기 때문에, 데이터 타입을 생략할 수 있다.
    - 매개변수가 하나뿐인(함수형 인터페이스의 메소드) 람다식은 소괄호를 생략할 수 있다. 
    - 매개변수가 없거나, 두 개 이상이면 소괄호를 생략할 수 없다.
    - 데이터타입을 명시하고있을 때도 소괄호를 생략할 수 없다.

- 람다식 좌변의 생략
    > `{return str.toUpperCase();}` -> `str.toUpperCase();`

    - 처리 구문이 한 문장일 경우, return 키워드와 중괄호를 생략할 수 있다.
    - return 이 생략될 경우, 중괄호 또한 반드시 생략해야한다.

```java
import java.util.function.Function;

class Test{
  public static void main(String[] args) {
    Function<String, String> f = (String str) -> {return "f.apply() 의 결과 : " + str;};
    // 람다식 우변의 생략
    Function<String, String> f2 = (str) -> {return "f2.apply() 의 결과 : " + str;};
    Function<String, String> f3 = str -> {return "f3.apply() 의 결과 : " + str;};
    
    // 람다식 좌변의 생략
    Function<String, String> f4 = str -> "f4.apply() 의 결과 : " + str;
    
    // 컴파일에러는 나지 않고 정상 실행됨
    System.out.println(f.apply("f"));
    System.out.println(f2.apply("f2"));
    System.out.println(f3.apply("f3"));
    System.out.println(f4.apply("f4"));
  }
}
```

#### **메소드 참조**
- 람다식 내부의 처리 구분이 하나일 때 사용한다.
- 해당 클래스의 메소드만을 참조한다고 명시한다.
- 메소드의 매개변수와 소괄호를 기술할 수 없다.
- static 메소드의 참조, 인스턴트 메소드 참조, 생성자 참조가 있다.
- 메소드 참조시, 해당 함수형 인터페이스에 들어가는 변수 이름이 아니라 데이터타입을 사용한다.
    > s -> s.toUpperCase();     (O)
    > <br>s :: toUpperCase();       (X)
    > <br>String :: toUpperCase();  (O)

- 메소드 참조시, 주어지는 클래스 안에 같은 이름의 Static 메소드가 있을 경우, 메소드를 특정할 수 없다는 컴파일 에러가 발생한다.
    ```java
    Function<Integer, Double> func = Double :: hashCode;
    ```

##### static 메소드 참조
> 클래스명 or 인터페이스변수명 :: 메소드명;

- 매개변수가 여러 개여도 생략할 수 있음.

##### 인스턴스 메소드 참조
> 인스턴스 변수명 :: 메소드명;

- 예시
    ```java
    import java.util.Arrays;

    class MethodOne{
      void method(String str){
        System.out.println("메소드 실행 : " + str);
      }
    }
    class Test{
      public static void main(String[] args) {
        String[] str = new String[] {"a", "b", "c"};
        Arrays.asList(str).forEach(new MethodOne()::method);
      }
    }
    ```
    
    - java.lang.Iterable 의 default void forEach
    - Collection은 Iterable의 인터페이스를 상속하고 있으므로, Collection들은 .forEach() 를 사용할 수 있다.
    - .forEach()의 매개변수가 null이 되면 NullPointException이 발생한다.

##### 생성자 참조
> 클래스명 :: new;

- 클래스의 인스턴스화, 배열 생성에 사용된다.

    ```java
    Function<Integer, String[]> func = String[] :: new;
    ```


-----
### 참고
 - Java プログラマ Gold SE8
 - 직접 돌려봄
