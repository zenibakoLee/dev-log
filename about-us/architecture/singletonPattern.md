# Singleton Pattern

구현 원칙:

1. 인스턴스를 오직 한개만 제공하는 클래스.
2. 해당 인스턴스에는 글로벌하게 접근할 수 있는 방법이 제공되어야 한다.

```java
public class Settings {
    private static Setting instance;

    // 클래스 밖에서 생성자를 호출해 인스턴스를 만들 수 없다.
    private Settings() {
    }

    public static Settings getInstance() {
        if (instance == null) { // ! Thread-safe 하지 않음.
            instance = new Settings();
        }
        return instance;
    }
}
```

1. synchronized 키워드 사용하기 => Lock을 가지고 있는 쓰레드만 접근하도록 하는 검증 과정에서 비롯되는 성능 저하 존재.

```java
public static synchronized Settings getInstance(){
//...    
        }
```

2. 이른 초기화 (eager initialization) => 인스턴스 생성이 오래걸리고, 메모리를 많이 사용하는데, 해당 객체를 호출하지 않는다면 비효율

```java
public class Settings {
    private static final Setting instance = new Settings();

}
```

3. double checked locking 방식 => 보다 효율적인 동기화 블럭 만들기
   instance가 있는 경우는 lock이 걸리지 않고, 없는 극히 일부의 상황에만 lock 사용

```java
public class Settings {
    private static volatile Setting instance; // volatile, java 1.5 이후에만 동작

    public static Settings getInstance() {
        if (instance == null) { // ! Thread-safe 하지 않음.
            synchronized (Settings.class) {
                if (instance == null) {
                    instance = new Settings();
                }
            }
        }
        return instance;
    }
}
``` 

4. static inner 클래스 (권장)

- 멀티스레드 환경에서 안전
- getInstance호출이 될 때 SettingHolder 클래스 호출이 되어 인스턴스가 생성되므로 lazy loading도 가능
- double checked locking 과 같이 복잡한 이론적 배경을 이해하지 않아도 됨

```java
public class Settings {
    private static Setting instance;

    // 클래스 밖에서 생성자를 호출해 인스턴스를 만들 수 없다.
    private Settings() {
    }

    private static class SettingHolder {
        private static final Settings instance = new Settings();
    }

    public static Settings getInstance() {
        return SettingHolder.instance;
    }
}
```

---
출처 : [GoF 디자인 패턴] 싱글톤 패턴 - 백기선

---
Spring 에서의 singleton
