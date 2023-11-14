# DI(Dependency Injection)

의존관계 주입

---

### 의존관계

> 의존대상 B가 변하면, 그것이 A에 영향을 미친다. => A가 B를 의존한다.
>
> -이일민, 토비의 스프링 3.1, 에이콘(2012), p113
>

악보가 바뀌면, 연주자의 연주에 변경이 생긴다. => 연주자가 악보를 의존한다.

```java
class Player {
    private Score score;

    public Player() {
        score = new SonataScore();
    }

    public playScore() {
        this.score.play();
    }
}
```

연주자가 재생할 곡을 직접 생성자를 호출해 지정하고 있는 상황.

보다 체계화된 악단이라면, 곡 선정은 연주자가 아니라, 외부에서 이뤄지지 않을까?

연주자가 어떠한 곡을 연주할지 외부에서 결정, 받도록 하는것이 DI(의존관계 주입)이다.

```java
class Player {
    private Score score;

    public Player(Score score) { // 생성자를 이용해 주입하는 방식
        this.score = score;
    }

    public playScore() {
        this.score.play();
    }
}
```

```java
class Player {
    private Score score;

    public void setScore(Score score) { // setter를 이용한 방식
        this.score = score;
    }

    public playScore() {
        this.score.play();
    }
}
```

```java
class Player {
    /*
       final이라 immutable이다.
       final이라 null이 들어갈 때 compile time에 경고가 발생한다
       예전에는 스프링 개발자들이 Property 주입(Setter Injection)을 많이 썼지만(Java Bean의 흔적 😵), 
       최근에는 생성자 주입을 선호. 특히 final 필드로 만들어서 쓰는 걸 강력히 권장하고 있다.
     */
    private final Score score;

    public Player(Score score) { //Spring 컨테이너가 등록된 Score 빈을 주입한다.
        this.score = score;
    }

    public playScore() {
        this.score.play();
    }
}
```

---

### 장점

- 결합성이 느슨해진다. (Decouping)
    - 의존한다는 것은 의존대상의 변화에 취약하다는 것. DI로 구현하게 될 경우, 주입받는 대상의 변경에 따른 추가 변경이 줄어듬.
- 재사용성의 상승
    - 연주자 내부에서만 사용하던 악보를, 편곡자, 무대담당자 등 다양한 클래스에서 재사용 할 수 있다.
- 테스트하기 좋음
    - 연주자의 테스트와 악보의 테스트를 분리해서 진행할 수 있다.

___

- [IoC와 DI패턴 - Martin Fowler](https://martinfowler.com/articles/injection.html)
- [DI는 의존관계(의존성)주입으로 해석해야 한다- 토비](https://groups.google.com/g/ksug/c/77jLQan8Q-E/m/yTo8xj-F-8gJ)
- [DI는 IoC를 사용하지 않아도 된다 by Jin-Wook Chung](https://jwchung.github.io/DI%EB%8A%94-IoC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EC%95%8A%EC%95%84%EB%8F%84-%EB%90%9C%EB%8B%A4)
