### 상속
- 부모가 되는 클래스(슈퍼클래스, 상위클래스)의 속성을 자식 클래스(서브클래스, 하위클래스)가 물려받는 것을 **상속**이라고 한다.
- 자식클래스는 반드시 부모클래스의 모든 속성을 상속해야 한다.
    - 부모클래스와 같은 패키지일 때 상속할 수 있는 멤버의 접근제어자 : public, protected, (default)
    - 부모클래스와 다른 패키지일 때 상속할 수 있는 멤버의 접근제어자 : public, protected
    - 접근제어자 private를 가진 멤버는 상속할 수 없다.

- 자식클래스가 상속받을 수 있는 부모클래스는 하나 뿐이지만, 부모클래스는 여러 개의 자식클래스에게 상속해줄 수 있다.
- extends 키워드를 사용해서 상속 관계를 나타낸다.
- 장점
    - 부모클래스의 멤버를 재사용할 수 있기 때문에 같은 속성을 가진 클래스끼리 중복된 코드를 줄여 간결한 코드를 작성할 수 있다.
    - 만들어진 코드에서 공통된 기능이나 속성 등을 수정하려고 할 때, 부모클래스만 수정하면 되므로 유지보수가 편하다.
    - 다형성 : 하나의 데이터 타입 변수가 여러가지 타입의 객체를 참조할 수 있다.
        ```java
        // 부모클래스, 슈퍼클래스, 상위클래스
        class Messenger{}
        // 자식클래스, 서브클래스, 하위클래스들↓
        class KaKao extends Messenger{}
        class Line extends Messenger{}
        class Telegram extends Messenger{}

        class Test{
            // 컴파일 에러 안 남
            public static void main(String[] a){
                Messenger m1 = new Kakao();
                Messenger m2 = new Line();
                Messenger m3 = new Telegram();
            }
        }

        ```
- 단점
    - 상속 구조가 복잡해지면 부모클래스에 기능을 추가하거나 변경했을 때 자식클래스에 미칠 수 있는 영향을 예측하기 어렵다.
    - 자식클래스는 반드시 부모클래스의 기능을 제공해야 하기 때문에, 여러 계층으로 상속을 구현할 경우 필요하지 않은 멤버까지 상속하게 되어 버릴 가능성이 있다.
    - 상속 관계의 클래스들을 일관성 있게 만들지 않았을 경우, 오히려 이해가 어렵고 사용하기 힘들어질 수 있다.


### 캐스팅
- 참조형 변수를 캐스팅할 때 주의할 점 
    - 상속관계가 아닌 대상을 캐스팅했을 때
        ```java
        class B extends A{}
        class C extends A{}
        class A{
            public static void main(String[] args) {
                B b = new C();
            }
        }
        ```
        > ##### 컴파일 에러
        > 型の不一致: C から B には変換できません  
        > 형식 불일치 : C에서 B로 형변환 할 수 없습니다.

    - 원래부터 슈퍼클래스였던 오브젝트를 캐스팅

        ```java
        class A {
            public static void main(String[] args) {
                A a = new Object();
            }
        }
        ```
        > ##### 컴파일 에러
        > 型の不一致: Object から A には変換できません  
        > 형식불일치 : Object에서 A로 형변환 할 수 없습니다.

    - 서브클래스를 슈퍼클래스 타입으로 캐스팅할 때 
        ```java
            class B extends A{}
            class A {
                public static void main(String[] args) {
                    A a = new B();
                }
            }
        ```
    - 슈퍼클래스의 static이 아닌 메소드만 오버라이드 된다. 

-----
### 참고
 - Java プログラマ Gold SE8
 - 직접 돌려봄
 - 객체지향 상속의 단점과 해법  https://ememomo.tistory.com/140
