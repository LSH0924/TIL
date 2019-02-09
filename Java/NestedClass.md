### Nested Class(Inner Class, 내부 클래스)
- 사용처를 제한시켜서 외부로부터 해당 클래스를 은닉 시키고싶을 때 사용한다. 
- 개중 static이 아닌 Nested Class 를 Inner Class 라고 부른다. 
- 내부 클래스를 메소드 안에 정의할 수 있다. 
- 내부 클래스의 이름은 외부 클래스와 같을 수 없다. 
- 모든 접근제어자를 사용할 수 있으며, 내부 클래스를 추상클래스로 선언할 수도 있다.  
- final 수식자를 붙일 수 있다. 

- **static Nested Class**
    - static인 멤버와 static이 아닌 멤버 모두를 가질 수 있다.
    - 외부 클래스에서 정의한 인스턴스에 접근할 수 없다.

- **none static Nested Class(Inner Class)**
    - static 멤버를 가질 수 없다.
    - 외부 클래스에서 정의한 인스턴스에 접근할 수 있다.
    - Nested Class 가 컴파일되면, `외부클래스명$NestedClass명.class` 파일이 생성된다.
    - 다른 클래스에서도 내부 클래스를 인스턴스화 해서 사용할 수 있다.
    - static이 아닌 Nested Class는 생성자를 사용한다.

```java
	class OuterClass {
		//static Nested Class
        static class StaticDefaultNestedClass{}
		public static class StaticPublicNestedClass{}
		protected static class StaticProtectedNestedClass{}
		private static class StaticPrivateNestedClass{
			static String className = "StaticPrivateNestedClass";
		}
		final static class StaticFinalNestedClass{}
        
		//none static Nested Class
		class DefaultNestedClass{}
		public class PublicNestedClass{}
		protected class ProtectedNestedClass{}
		private class PrivateNestedClass{
			String className = "PrivateNestedClass";
		}
		final class FinalNestedClass{}
		
        // NestedClass, static NestedClass 사용하기
		public static void main(String[] args) {
		    //static Nested Class
			System.out.println(StaticPrivateNestedClass.className);
		    //none static Nested Class
			System.out.println(new OuterClass().new PrivateNestedClass().className);
		}
    }
```
- private 접근제어자를 가진 NestedClass를 OuterClass 내부에서 사용하지 않을 경우 경고가 발생한다.
> The type OuterClass.PrivateNestedClass is never used locally
> <br>The type OuterClass.StaticPrivateNestedClass is never used locally


### 로컬클래스 
- 메소드 안에서 선언된 클래스. static이 붙을 수 없다. 
- 접근제어자를 사용할  수 없다. 
- abstract, final 키워드를 사용할 수 있다. 
- 외부 클래스의 메소드에 접근할 수 있다.
- 로컬 클래스에서 이용할 수 있는 외부 클래스의 메소드나 변수는 final(정수 혹은 오버라이드 불가)이 된다.
- SE7에서는 final 수식자를 명시적으로 부여해 줄 필요가 있었지만, SE8에서는 암묵적으로 부여해주게 되었다. 

