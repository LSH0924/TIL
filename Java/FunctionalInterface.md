# Functional interface(함수형 인터페이스)
- 다른 static, default 등의 메소드 개수와는 상관없이 가지고 있는 추상 메소드의 개수가 한 개 뿐일 때, 함수형 인터페이스라고 한다.
- 람다식을 사용하기 위해 도입되었다.
- SE8에서 추가된 함수형 인터페이스

| 인터페이스명 | 함수명 | 설명 |
| --- | --- | --- |
| Function< T, R > | R apply(T t) | 매개변수로 T 타입을 받고 R 타입을 반환 | 
| BiFunction< T,U,R > | R apply(T t, U u) | 매개변수로 T와 U를 받고, R 타입을 반환 |
| Consumer< T > | void accept(T t) | 반환형 없이 대상 객체(매개변수)에 변화를 주고싶을 때 사용 |
| BiConsumer< T > | void accept(T t, U u) | 반환형 없이 대상객체(매개변수 2개)에 변화를 주고싶을 때 사용 |
| Predicate< T > | boolean test(T t) | 주어진 조건이 참인지 거짓인지를 판별 |
| BiPredicate< T, U > | boolean test(T t, U u) | 주어진 두 조건이 참인지 거짓인지를 판별 |
| Supplier< T > | T get() | 매개변수를 받지 않지만, T 타입을 반환
| UnaryOperator< T > | T apply(T t) | Function<T,T> 와 같음
| BinaryOperator< T > | T apply(T t1, T t2) | BiFunction<T,T,T>와 같음

```java
import java.util.function.BiConsumer;
import java.util.function.BiFunction;
import java.util.function.BiPredicate;
import java.util.function.BinaryOperator;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;
import java.util.function.UnaryOperator;

class Test{
  public static void main(String[] args) {
    Function<String, String> function = str -> "Function : " + str;
    BiFunction<String, Integer, String> biFunction = (str, i) -> "BiFunction : " + str + i;
    Consumer<String> consumer = str -> System.out.println("Consumer : " + str.toUpperCase());
    BiConsumer<String, String> biConsumer = (str, str2) -> System.out.println("BiConsumer : " + str.toUpperCase() + str2.toUpperCase());
    Predicate<Integer> predicate = i -> i < 0;
    BiPredicate<Integer, Integer> biPredicate = (i1, i2) -> i1 < i2;
    Supplier<String> supplier = () -> "SUPPLIER";
    UnaryOperator<Integer> unaryOperator = i -> i + i;
    BinaryOperator<Integer> binaryOperator = (i1, i2) -> i1 * i2;
    
    System.out.println(function.apply("function.apply()"));
    System.out.println(biFunction.apply("parameter", 2));
    consumer.accept("abcdefg");
    biConsumer.accept("hOt ", "Chocolate");
    System.out.println(predicate.test(0));
    System.out.println(biPredicate.test(10, 20));
    System.out.println(supplier.get());
    System.out.println(unaryOperator.apply(3));
    System.out.println(binaryOperator.apply(2, 5));
  }
}
```

> <strong>결과</strong>
> <br>Function : function.apply()
> <br>BiFunction : parameter2
> <br>Consumer : ABCDEFG
> <br>BiConsumer : HOT CHOCOLATE
> <br>false
> <br>true
> <br>SUPPLIER
> <br>6
> <br>10

- 암묵적 final
    익명 클래스를 구현할 때 처럼 로컬 변수를 람다식 내부에서 사용할 수 있다. 이때, 내부에서 암묵적으로 final 키워드가 부여된다. 하지만 선언(초기화)한 뒤 대입 연산자를 이용해 변경이 되어버린 변수는 암묵적 final 취급할 수 없다.


# 기본 데이터 형을 다루는 함수형 인터페이스
- int, double, long의 기본적인 함수형 인터페이스

| 인터페이스명 | 함수명 |
| --- | --- |
| IntFunciton< R > | R apply (int value) |
| IntConsumer | void accept(int value) |
| IntPredicate | boolean teest(int value) |
| IntSupplier | int getAsInt() |
| IntUnaryOperator| int applyAsInt(int operand) |
| IntBinaryOperator | int applyAsInt(int r, int l)|
| ToIntFunction< T > | int applyAsInt(T value)|
| ToIntBiFunction< T,U > | int applyAsInt(T t, U u) |
| IntToDoubleFunction | double applyAsDouble(int value) |
| IntToLongFunction | logn applyAsLong(int value) |
| ObjIntConsumer< T > | void accept(T t, int value) |
   
 - double, long 타입을 대상으로 한 인터페이스도 같은 이름 규칙으로 선언되어 있다.
- boolean형 특화 함수형 인터페이스

| 인터페이스명 | 함수명 |
| --- | --- |
| BooleanSupplier | boolean getAsBoolean()|

- 기본형만 다루는 함수형 인터페이스에 WrapperClass를 넣으면 컴파일 에러가 발생함. 
```java
  // 에러 발생 예제코드
```
-----
### 참고
 - Java プログラマ Gold SE8
 - 직접 돌려봄
