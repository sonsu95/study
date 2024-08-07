롬복에서 지원하는 builder 어노테이션은 심플 빌더 패턴을 따른다. 

기본 사용법은 다음과 같다. 

```java
import lombok.Builder;  
  
@Builder  
public class Hamburger {  
    private int buns;  
    private int patties;  
    private int lettuce;  
    private int tomato;  
    private int cheese;  
    private int bacon;  
    private int pickles;  
    private int onions;  
}
```

해당 코드에서 어노테이션을 벗겨보면 다음과 같은 상태가 된다. 

```java  
public class Hamburger {  
    private int buns;  
    private int patties;  
    private int lettuce;  
    private int tomato;  
    private int cheese;  
    private int bacon;  
    private int pickles;  
    private int onions;  
  
    Hamburger(int buns, int patties, int lettuce, int tomato, int cheese, int bacon, int pickles, int onions) {  
        this.buns = buns;  
        this.patties = patties;  
        this.lettuce = lettuce;  
        this.tomato = tomato;  
        this.cheese = cheese;  
        this.bacon = bacon;  
        this.pickles = pickles;  
        this.onions = onions;  
    }  
  
    public static HamburgerBuilder builder() {  
        return new HamburgerBuilder();  
    }  
  
    public static class HamburgerBuilder {  
        private int buns;  
        private int patties;  
        private int lettuce;  
        private int tomato;  
        private int cheese;  
        private int bacon;  
        private int pickles;  
        private int onions;  
  
        HamburgerBuilder() {  
        }  
  
        public HamburgerBuilder buns(int buns) {  
            this.buns = buns;  
            return this;  
        }  
  
        public HamburgerBuilder patties(int patties) {  
            this.patties = patties;  
            return this;  
        }  
  
        public HamburgerBuilder lettuce(int lettuce) {  
            this.lettuce = lettuce;  
            return this;  
        }  
  
        public HamburgerBuilder tomato(int tomato) {  
            this.tomato = tomato;  
            return this;  
        }  
  
        public HamburgerBuilder cheese(int cheese) {  
            this.cheese = cheese;  
            return this;  
        }  
  
        public HamburgerBuilder bacon(int bacon) {  
            this.bacon = bacon;  
            return this;  
        }  
  
        public HamburgerBuilder pickles(int pickles) {  
            this.pickles = pickles;  
            return this;  
        }  
  
        public HamburgerBuilder onions(int onions) {  
            this.onions = onions;  
            return this;  
        }  
  
        public Hamburger build() {  
            return new Hamburger(this.buns, this.patties, this.lettuce, this.tomato, this.cheese, this.bacon, this.pickles, this.onions);  
        }  
  
        public String toString() {  
            return "Hamburger.HamburgerBuilder(buns=" + this.buns + ", patties=" + this.patties + ", lettuce=" + this.lettuce + ", tomato=" + this.tomato + ", cheese=" + this.cheese + ", bacon=" + this.bacon + ", pickles=" + this.pickles + ", onions=" + this.onions + ")";  
        }  
    }  
}
```

toString이 기본적으로 포함되어져 있으며, 심플 빌더 패턴에서처럼 정적 내부 클래스를 사용하고 있는 것을 알 수 있다. 