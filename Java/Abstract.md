# 추상(Abstract) 클래스
  - abstract 키워드를 클래스의 선언부에 붙인다.
  - 구상화 된 메소드와 추상 메소드를 같이 사용할 수 있다.
  - 추상클래스 자체로는 new 로 인스턴스화 시킬 수 없지만, 서브클래스에 상속시켜 사용하거나 익명클래스를 만들어 사용할 수 있다. 
  - 추상클래스를 상속한 서브클래스가 구상클래스일 경우, 추상 클래스의 추상 메소드를 전부 오버라이드 해야 한다. 
  - 추상클래스를 상속한 서브클래스가 추상클래스일 경우, 슈퍼클래스의 추상메소드를 오버라이드 할 필요는 없다.

# 추상 메소드
 - abstract 키워드를 가진 메소드
 - 추상클래스 안에 선언될 때는 메소드의 실현부를 구체화시킬 필요는 없다.
 - 오버라이드 할 때 구체적인 메소드의 처리를 정한다.

```java
// 추상클래스1
abstract class SuperAbstractClass{
  // 추상클래스1의 추상메소드
  abstract void abstractMethod();
}

// 추상클래스1을 상속하는 서브 추상클래스2
abstract class SubAbstractClass extends SuperAbstractClass{}

// 추상클래스1을 상속하는 구체화된 서브클래스
class MaterializedClass extends SuperAbstractClass{
  // 추상클래스1의 추상메소드를 오버라이드 해야 컴파일에러가 나지 않는다.
  @Override
  void abstractMethod() {
    System.out.println("Overridied abstract Method");
  }
}

class Test{
  public static void main(String[] args) {
    // 추상클래스는 자신의 인스턴스를 만들 수 없으므로 컴파일에러
    // SuperAbstractClass ab1 = new SuperAbstractClass();
    
    // 추상클래스를 상속받은 클래스를 추상클래스 형의 변수에 넣을 수 있다.
    SuperAbstractClass ab2 = new MaterializedClass();
    MaterializedClass ab3 = new MaterializedClass();
    
    // ab2가 SuperAbstractClass형 이지만, 들어있는 것은 MaterializedClassd의 인스턴스이기 때문에 오버라이드 된 메소드가 실행된다. 
    ab2.abstractMethod();
    ab3.abstractMethod();
  }
}
```
-----
### 참고
 - Java プログラマ Gold SE8
 - 직접 돌려봄

