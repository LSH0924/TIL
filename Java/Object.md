### Object.equals(Object obj)
- Object 클래스 제공.
- 자신과 매개변수로 받은 오브젝트와의 일치 여부를 boolean  형으로 반환한다. 
- Object클래스의 .equals()는 연산자 "==" 로 비교한 값과 결과가 같다.
```java
Object obj1 = new Object();
Object obj3 = obj1;
Object obj2 = new Object();

System.out.println("obj1.equals(obj2) : " + obj1.equals(obj2));  
System.out.println("obj2.equals(obj3) : " + obj2.equals(obj3));
System.out.println("obj3.equals(obj1) : " + obj3.equals(obj1));
```
> 결과
> obj1.equals(obj2) : false
> obj2.equals(obj3) : false
> obj3.equals(obj1) : true

#### .equals()의 오버라이드
Object 클래스를 상속하는 String 클래스에서도 .equals()를 사용할 수 있다.
```java
String str1 =  new String("똑같니");
String str2 =  new String("똑같니");

System.out.println("str1.equals(str2) : " + str1.equals(str2));
System.out.println("str1 == str2 : " + (str1 == str2));
```
> 결과
> str1.equals(str2) : true
> str1 == str2 : true

Object 클래스의 .equals()의 결과는 == 연산자로 두 객체를 비교한 결과와 같다. 하지만 String 클래스에서는 .equals() 메소드와 == 연산자로 비교한 결과가 다르다.  String 클래스의 .equals() 는 객체의 메모리 값이 아닌, 객체가 가지고 있는 문자열 자체를 비교하도록 오버라이딩 되어 있기 때문이다.

### Object.hashCode()

객체의 주소값을 이용해서 객체가 가지고 있는 고유의 해시코드를 반환한다.
```java
Object obj = new Object();
Object obj2 = new Object();	

System.out.println(obj.hashCode());
System.out.println(obj2.hashCode());
```
> 결과
> 366712642
> 1829164700

#### .equals()와 .hashCode() 를 동시에 오버라이딩 해야 하는 이유

HashSet, HashMap, Hashtable 등의 중복을 허용하지 않는 프레임워크에서 객체를 비교할 때,  hashCode() 를 사용해 얻은 해시코드 값이 다르면 다른 객체로 판단하기 때문이다.

#### .equals()와 .hashCode() 오버라이드 규칙

- 같은 오브젝트의 .hashCode() 가 여러 번 호출되어도 같은 결과를 반환한다.
- 두 개의 오브젝트를 .equals() 로 비교한 값이 true이면, 해시코드를 비교한 값 또한 true이어야 한다.
- 두 개의 오브젝트를 .equals() 로 비교한 값이 false 이면, 두 오브젝트의 해시코드는 같을 수도 있고 다를 수도 있다.
- 두 오브젝트의 해시코드가 다를 경우, 이 오브젝트들을 .equals()한 결과는 false이어야 한다.