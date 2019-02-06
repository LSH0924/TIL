### 상속

- 참조형 변수를 캐스팅할 때 주의할 점 
    - 상속관계가 아닌 대상을 캐스팅했을 때
        ```
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

        ```
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
        ```
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
