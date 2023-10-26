# Annotation

Java Annotations은 클래스, 메서드, 변수 등의 요소에 메타데이터를 추가하여 프로그램 동작을 변경하거나 기능을 확장합니다.
자바 어노테이션은 클래스 파일에 임베디드되어 컴파일러에 의해 생성된 후 자바 가상머신에 포함되어 작동합니다.

- 사전 정의 어노테이션: 자바는 여러 사전 정의(Predefined) 어노테이션을 제공합니다. @Override, @Deprecated, @SuppressWarnings

- 사용자 정의 어노테이션: 프로그래머는 자신만의 어노테이션을 정의하고 사용할 수 있습니다. 어노테이션은 @interface 키워드를 사용하여 정의됩니다.
    - @Override: 메서드가 슈퍼 클래스에서 상속된 메서드를 오버라이드한다는 것을 나타냅니다.
    - @Deprecated: 메서드, 클래스, 필드 또는 다른 요소가 더 이상 권장되지 않는다는 것을 나타냅니다.
    - @SuppressWarnings: 컴파일러 경고를 억제하는 데 사용됩니다. 어떤 종류의 경고를 억제할 것인지를 매개변수로 지정합니다. 예를 들어, @SuppressWarnings("deprecation")
      은 사용 중단된 메서드를 사용하는 관련 경고를 억제합니다.

- 기타 어노테이션에 적용되는 어노테이션 (메타 어노테이션)
    - @Retention: 어노테이션을 언제까지 유지할 것인지를 정의하는 데 사용됩니다. 세 가지 옵션이 있으며 RetentionPolicy.SOURCE, RetentionPolicy.CLASS,
      RetentionPolicy.RUNTIME 중 하나를 선택합니다.
        - RetentionPolicy.SOURCE: 컴파일 시점까지만 유지되며, 런타임에 정보를 사용할 수 없습니다.
        - RetentionPolicy.CLASS: 클래스 파일까지 유지되지만 런타임에 정보를 사용할 수 없습니다.
        - RetentionPolicy.RUNTIME: 런타임까지 유지되며, 런타임에 리플렉션을 통해 어노테이션 정보를 읽을 수 있습니다.
    - @Documented: 해당 어노테이션을 Javadoc 문서에 포함시키도록 지정합니다. 이를 통해 다른 개발자들이 해당 어노테이션의 사용법과 의미를 문서화에서 쉽게 이해할 수 있습니다.
    - @Target: 어노테이션을 어디에 적용할 수 있는지 정의합니다. @Target은 다양한 요소(ElementType) 중 하나를 선택하게 되며, 예를 들어 ElementType.TYPE,
      ElementType.METHOD, ElementType.FIELD 등을 지정할 수 있습니다.
    - @Inherited: 클래스가 어떤 어노테이션을 상속할지를 지정합니다. 만약 부모 클래스가 어노테이션을 가지고 있고 이 어노테이션이 @Inherited로 표시되어 있다면, 자식 클래스도 동일한 어노테이션을
      상속합니다.
- 자바 7이후 추가된 어노테이션
    - @SafeVarargs: 가변 인자(varargs) 매개변수에 안전하다는 것을 나타냅니다. 이 애너테이션을 사용하면 컴파일러 경고를 억제할 수 있습니다. 주로 제네릭 메서드와 함께 사용됩니다.
    - @FunctionalInterface: 함수형 인터페이스를 정의하는 데 사용됩니다. 함수형 인터페이스는 하나의 추상 메서드만을 가지며, 람다 표현식을 사용할 때 사용되는 인터페이스 유형을 나타냅니다.
    - @Repeatable: 동일한 타입의 어노테이션을 여러 번 사용할 수 있도록 허용합니다. 이를 통해 여러 개의 동일한 어노테이션을 배열이나 컨테이너 없이 직접 사용할 수 있습니다.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    String value() default "Default Value";

    int count() default 0;
}
```



