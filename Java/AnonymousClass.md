### 익명클래스
- 클래스의 선언과 동시에 인스턴스화 시키는 방법.
- 추상클래스를 상속시키지 않고 인스턴스화 시키고싶을 때 익명클래스를 사용한다.
- 선언한 익명 클래스는 생성자로 불러온 클래스의 서브클래스가 된다.

	```java
	abstract class NoNamedClass{
		abstract void method();
	}

	class Test{
		public static void main(String[] args) {
			new NoNamedClass(){     //인스턴스화 시킨 익명클래스는 NoNamedClass의 서브클래스 취급을 받는다
				@Override
				void method() {
					System.out.println("이 클래스는 익명클래스 입니다");
				}
			}.method();
		}
	}
	```

- 선언한 익명 클래스는 한 문장으로 인식되기 때문에 꼭 세미콜론으로 끝내주어야 한다. 
- 접근제어자는 사용할 수없다.
- abstract, final 키워드를 사용할 수 있다. 
- 외부 클래스의 멤버에 접근할 수 있다. 
- 외부 클래스의 메소드의 매개변수, 로컬변수에 접근할 수 있다. 
- 외부 클래스에 접근할 경우, 암묵적으로 final이 적용된다. 
- 생성자는 정의할 수 없다. 
