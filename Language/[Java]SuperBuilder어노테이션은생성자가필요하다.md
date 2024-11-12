Annotation Processor로서
@SuperBuilder는 상속 관계에 있는 클래스들에 대해 빌더 패턴을 간편하게 적용할 수 있도록 지원하는 Lombok의 어노테이션이다.
상속 구조에서도 부모 클래스와 자식 클래스의 필드를 모두 포함한 빌더를 생성할 수 있도록 지원한다.

Lombok이 빌더 패턴을 생성하는 과정에서 클래스의 필드를 설정하기 위해 생성자를 통해 필드를 초기화하는 과정이 있기때문에
@NoArgsConstructor, @AllArgsConstructor 등의 추가적인 생성자 어노테이션이 필요할 수 있다.
