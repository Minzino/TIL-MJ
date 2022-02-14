# 14주차 과제 : 제네릭

## 제네릭이란?

- 시작하기 전에 제네릭이 무엇이고 제네릭을 왜 사용해야하는지 알아보자

# **제네릭이란?**

- 데이터 타입(data type)을 일반화(generalize)하는것을 의미한다
- 제네릭은 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법이다.
- 이렇게 컴파일 시 type check를 하면 장점이 있다.
    - 클래스나 메소드 내부에서 사용되는 객체의 타입의 안정성을 높일 수 있다.
    - 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있다.
- Java 5 이전에서는 여러 타입을 사용하는 대부분의 클래스나 메소드에서 인수나 반환값으로 Object타입을 사용했었다. 하지만 이 경우에는 반환된 Object 객체를 다시 원하는 타입으로 타입을 변환해야하고 , 이때 오류가 발생할 가능성도 생긴다. 하지만 Java5부터 도입된 제네릭을 사용하면 컴파일 시에 미리 타입이 정해지므로, 타입 검사나 타입 변환과 같은 번거로운 작업을 생략할 수 있게 된다.

# **제네릭을 사용해야하는 이유**

- 제네릭 타입을 사용함으로써 **잘못된 타입이 사용될 수 있는 문제를 컴파일과정에서 제거할 수 있기 때문**이다.
- 자바 컴파일러는 코드에서 잘못 사용된 타입 때문에 발생하는 문제점을 제거하기 위해 제네릭 코드에 대해 강한 타입 체크를 한다.
- **실행 시 타입 에러가 나는 것보다 컴파일 시에 미리 타입을 강하게 체크해서 에러를 사전에 방지하는 것이 좋다**.
- 제네릭 코드를 사용하면 타입을 국한하기 때문에 요소를 찾아올 때 **타입 변환을 할 필요가 없어 프로그램 성능이 향상되는 효과**를 얻을 수 있습니다.

# **제네릭 사용법**

### **제네릭의 선언 및 생성**

```
class GenericSample<T> {  // 제네릭 타입 T 선언
    T element;
    void setElement(T element) {
    this.element = element;
    T getElement() {
    return element;
}
```

### **타입 변수**

- 아무런 이름이나 지정해도 컴파일하는데 전혀 상관이 없다.
- 현존하는 클래스를 사용해도되고 존재하지 않는 것을 사용해도 된다.
- 임의의 참조형 타입을 의미한다.
- 꼭 'T' 를 사용안하고 어떠한 문자를 사용해도되지만 아래의 네이밍을 지켜주는 것이 좋다.
- 여러 개의 타입 변수는 쉼표(,)로 구분하여 명시할 수 있다.
- 타입 변수는 클래스에서뿐만 아니라 메소드의 매개변수나 반환값으로도 사용할 수 있다.

### **제네릭 타입의 이름 정하기**

- E : 요소 (Element, 자바 컬렉션에서 주로 사용됨)
- K : 키
- N : 숫자
- T : 타입
- V : 값
- S,U,V : 두번 째, 세 번째, 네 번째에 선언된 타입

### **예제**

```
class GenericSample<T> {
    T element;

    public static void main(String[] args) {
        GenericSample<Integer> integerGenericSample = new GenericSample<>();
        integerGenericSample.setElement(3);
        GenericSample<String> stringGenericSample = new GenericSample<>();
        stringGenericSample.setElement("thewing");
        System.out.println("integerGenericSample.getElement() = " + integerGenericSample.getElement());
        System.out.println("stringGenericSample.getElement() = " + stringGenericSample.getElement());
    }

    public void setElement(T element) {
        this.element = element;
    }
    public T getElement () {
        return element;
    }

}
```

- 제네릭의 타입을 어떻게 넣느냐에 따라 결과가 달라진다

### **결과**

```
integerGenericSample.getElement() = 3
stringGenericSample.getElement() = thewing
```

### **바이트코드**

Object로 담는지 궁금해서 바이트코드도 한번 열어보았다.

![https://blog.kakaocdn.net/dn/l5vXZ/btqX6f6kX4c/YawuNGGV4uNthHJHxqvBv1/img.png](https://blog.kakaocdn.net/dn/l5vXZ/btqX6f6kX4c/YawuNGGV4uNthHJHxqvBv1/img.png)

![https://blog.kakaocdn.net/dn/csVnVX/btqXZHCPYPk/Ifu2tJrs32yhYn1KHCUZ91/img.png](https://blog.kakaocdn.net/dn/csVnVX/btqXZHCPYPk/Ifu2tJrs32yhYn1KHCUZ91/img.png)

- 예상대로 Object가 있다.

# **제네릭 주요 개념 (바운디드 타입, 와일드 카드)**

- 제네릭 타입에는 여러가지가 있다.

### **바운드 타입 매개변수(Bounded type parameter)**

- 바운드타입은 특정 타입의 서브 타입으로 제한한다. 클래스나 인터페이스 설계할 때 가장 흔하게 사용할 정도로 많이 볼 수 있는 개념이다.

### **예제**

```
public class BoundTypeSample <T extends Number>{
    public void set(T value) {}

    public static void main(String[] args) {
        BoundTypeSample<Integer> boundTypeSample = new BoundTypeSample<>();
        boundTypeSample.set("Hi");
    }
}
```

![https://blog.kakaocdn.net/dn/bPzTpT/btqXZHCPZaZ/lqrWT1Nm6XydqlytoCNy21/img.png](https://blog.kakaocdn.net/dn/bPzTpT/btqXZHCPZaZ/lqrWT1Nm6XydqlytoCNy21/img.png)

- 이와 같이 컴파일 에러가 난다.
- `BoundTypeSample` 클래스의 Type 파라미터를 `T`로 선언하고 `<T extends Number>` 로 선언한다. `BoundTypeSample`의 타입으로 `Number`의 서브 타입만 허용한다는 것이다.
- `Integer`는 Number의 서브타입이기 때문에 BoundTypeSample와 같은 선언이 가능하지만 `set`함수의 인자로 문자열을 전달하려고 했기 때문에 컴파일 에러가 발생하게된다.

### **WildCard**

- 제네릭으로 구현된 메소드의 경우 선언된 타입으로만 매개변수를 입력해야한다. 이를 상속받은 클래스 혹은 부모 클래스를 사용하고 싶어도 불가능하고 어떤 타입이 와도 상관없는 경우에 대응하기 좋지 않다. 이를 위한 해법으로 Wildcard를 사용한다.

### **와일드 카드 종류**

### **Unbounded WildCard**

- Unbounded Wildcard는 List<?> 와 같은 형태로 물음표만 가지고 정의되어지게된다. 내부적으로 Object로 정의되어서 사용되고 모든 타입의 인자를 받을 수 있다. 타입 파라미터에 의존하지 않는 메소드만을 사용하거나 Object 메소드에서 제공하는 기능으로 충분한 경우에 사용한다.
    - Unbounded 뜻
        - 무한한, 끝이 없는
- Object 클래스에서 제공되는 기능을 사용하여 구현할 수 있는 메서드를 작성하는 경우
- 타입 파라미터에 의존적이지 않은 일반 클래스의 메소드를 사용하는 경우
    - ex) List.clear, List.size 등등

### **Upper Bounded Wildcard**

- Upper Bounded Wildcard는 List<? extends Foo> 와 같은 형태로 사용하고 특정 클래스의 자식 클래스만을 인자로 받는다는 것이다. 임의의 Foo 클래스를 상속받는 어느 클래스가 와도 되지만 사용할 수 있는 기능은 Foo클래스에 정의된 기능만 사용이 가능하다.

### **Lower Bounded Wildcard**

- Lower Bounded Wildcard는 List<? super Foo>와 같은 형태로 사용하고, Upper Bounded Wildcard와 다르게 특정 클래스의 부모 클래스만을 인자로 받는다는 것이다.

### **기타**

### **매개변수화 타입(Parameterized type)**

- 하나 이상의 타입 매개변수(type parameter)를 선언하고 있는 클래스나 인터페이스를 제네릭 클래스, 또는 제네릭 인터페이스라고 하고 이를 제네릭 타입이라고 한다. 각 제네릭 타입에서는 매개변수화 타입(Parameterized type)들을 정의한다

```
List<String> list = new ArrayList<>();
```

- <> 안에 있는 String은 실 타입 매개변수라고하고 List 인터페이스에 선언되어있는 List의 E를 형식 타입 매개변수라고 한다. 제네릭은 타입 소거자(Type erasure)에 의해 자신의 타입 요소 정보를 삭제한다.
- 이것을 컴파일 해보면 아래와 같이 변경이 된다

```
ArrayList list = new ArrayList();
```

### **예제**

```
package LiveStudy._14Week;

import java.util.ArrayList;
import java.util.List;

public class TestSample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
    }
}
```

컴파일 파일

![https://blog.kakaocdn.net/dn/FtlDB/btqXY1BwpZF/uaUOFn9r7ItxMVcs2lZOd0/img.png](https://blog.kakaocdn.net/dn/FtlDB/btqXY1BwpZF/uaUOFn9r7ItxMVcs2lZOd0/img.png)

- 신기하게도 new ArrayList만 있다.

바이트코드로 확인해보자

![https://blog.kakaocdn.net/dn/cAFLLi/btqX82FEGUI/k9KqPaoZhWJ2tH8dNdYAkk/img.png](https://blog.kakaocdn.net/dn/cAFLLi/btqX82FEGUI/k9KqPaoZhWJ2tH8dNdYAkk/img.png)

- ArrayList를 생성할 때 어떠한 타입 정보도 들고 있지 않다. new ArrayList()로 생성한 것과 동일하게 바이트코드가 생성된다
- declaration = 선언은 List 으로 선언하므로 빨간 부분을 보면 List으로 지역변수가 선언되는 것 같다(?)
- 컴파일러는 커파일 단계에서 List 컬렉션에 String 인스턴스만 저장되어야 한다는 것을 알게 되었고 또 그것을 보장해주기 때문에 ArrayList list로 변경하여도 런타임에 동일한 동작을 보장한다. E, List, List 과 같은 타입들을 비 구체화(non-reifiable type) 타입이라고 하며 그 반대로 구체화(reifiable type)타입이 있으며 primitives, non-generic types, raw types 또는 List<?> Map 과 같이 Unbounded Wildcard Type이 있다.

비 구체화 타입(non-reifiable type)

- 타입 소거자에 의해 컴파일 타임에 타입 정보가 사라지는 것(런타임에 구체화 하지 않는 것)

구체화 타입(reifiable type)

- 자신의 타입 정보를 런타임 시에 알고 지키게 하는 것(런타임에 구체화 하는 것)

### **제네릭 선언에 사용하는 타입의 범위도 지정할 수 있다.**

- <>에 어떤 타입도 상관도 상관없다고 했지만, wildcard로 사용하는 타입을 제한할 수는 있다. 먼저 방법부터 알려주면 "?" 대신 "? extends 타입"으로 선택하는 것

```
public class WildcardGeneric<W> {
    W wildcard;

    public W getWildcard() {
        return wildcard;
    }

    public void setWildcard(W wildcard) {
        this.wildcard  = wildcard;
    }
}
```

```
public class Car {
    protected String name;

    public Car(String name) {
        this.name = name;
    }
    public String toString() {
        return "Car name=" + name;
    }

}
```

```
public class Bus extends Car{
    public Bus(String name) {
        super(name);
    }

    public String toString() {
        return "Bus name=" + name;
    }
}
```

```
public class CarWildcardSample {

    public static void main(String[] args) {
        CarWildcardSample sample = new CarWildcardSample();
        sample.callBoundedWildcardMethod();
    }

    public void callBoundedWildcardMethod() {
        WildcardGeneric<Car> wildcard = new WildcardGeneric<Car>();
        wildcard.setWildcard(new Car("Mustang"));
        boundedWildCardMethod(wildcard);

    }

    public void boundedWildCardMethod(WildcardGeneric<? extends Car> c) {
        Car value = c.getWildcard();
        System.out.println(value);
    }
}
```

- 앞서 사용했던 "?"라는 wildcard는 어떤 타입이 오더라도 상관이 없었다. 하지만 , `boundedWildCardMethod()`에는 "?"대신 "? extends Car"라고 적어준 것이 보일 것이다. 이렇게 정의한 것은 제네릭 타입으로 Car를 상속받은 모든 클래스를 사용할 수 있다는 의미이다. 따라서, boundedWildcardMethod()의 매개 변수에는 다른 타입을 제네릭 타입으로 선언한 객체가 넘어올 수 없다. 즉 컴파일시 에러가 발생하므로 반드시 Car클래스와 관련되어 있는 상속한 클래스가 넘어와야만한다.
- 결과

```
Car name=Mustang
```

### **? extends 타입**

- Bounded Wildcards라고 부른다

### **메소드를 제네릭하게 선언**

- 매개 변수로 사용된 객체에 값을 추가할 수가 없다는 것이다

```
public class GenericWildcardSample {
    public static void main(String[] args) {
        GenericWildcardSample sample = new GenericWildcardSample();
        sample.callGenericMethod();
    }

    public <T> void genericMethod(WildcardGeneric<T> c, T addValue) {
        c.setWildcard(addValue);
        T value = c.getWildcard();
        System.out.println(value);
    }

    public void callGenericMethod() {
        WildcardGeneric<String> wildcard = new WildcardGeneric<String>();
        genericMethod(wildcard,"Data");
    }
}
```

```
Data
```

- ?를 사용하는 Wildcard처럼 타입을 두리뭉실하게 하는 것보다는 이처럼 명시적으로 메소드 선언시 타입을 지정해주면 보다 더 견고한 코드를 작성할 수 있다.

# **제네릭 메소드 만들기**

- 제네릭 메소드란 메소드의 선언부에타입 변수를 사용한 메소드를 의미한다
- 이때 타입 변수의 선언은 메소드 선언부에서 반환 타입 바로 앞에 위치한다

### **예제**

원본 docs Collections.sort

![https://blog.kakaocdn.net/dn/Uhsht/btqX6frISjp/iqmkufJjDGtETrnevGwK2K/img.png](https://blog.kakaocdn.net/dn/Uhsht/btqX6frISjp/iqmkufJjDGtETrnevGwK2K/img.png)

```
public static <T> void sort(...) {...}
```

아래 예제의 제네릭 클래스에서 정의된 타입 변수 `T`와 제네릭 메소드에서 사용된 타입 변수 T는 별개이란 것을 알아야한다.

```
import java.util.Comparator;
import java.util.List;

public class Collections {
    public static <T> void sort(List<T> list, Comparator<? super T> c) {
        list.sort(c);
    }

}
```

# **Erasure**

### **제네릭의 타입 소거(Generics Type Erasure)**

- 위에서 한번 Erasure를 언급했었다. Erasure란 원소 타입을 컴파일 타임에서만 검사를하고 런타임에는 해당 타입 정보를 알 수가 없다. 즉 컴파일 상태에만 제약 조건을 적용하고, 런타임에는 타입에 대한 정보를 소거하는 프로세스이다

```
List<Object> list = new ArrayList<Integer>(); //compile error
list.add("thewing"); // type 이 일치하지 않아 add가 안된다
```

- 이와 같은 상황에서 컴파일 오류를 확인이 가능하다

Java 컴파일러는 타입 소거를 아래와 같이 적용을 한다.

- 제네릭 타입(Example) 에서는 해당 타입 파라미터(T) 나 Object로 변경해준다. Object로 변경하는 경우 unbounded 된 경우를 뜻하며, 이는 <E extends Comparable>와 같이 bound를 해주지 않은 경우를 의미한다. 이 소거 규칙에 대한 바이트 코드는 제네릭을 적용할 수 있는 일반 클래스, 인터페이스, 메서드에 적용이 가능하다.
- 타입 안정성 보존을 위해 필요시 type casting을 넣어준다
- 확장된 제네릭 타입에서 다형성을 보존하기 위해 bridge method를 생성한다

```
public static  <E> boolean containsElement(E [] elements, E element){
    for (E e : elements){
        if(e.equals(element)){
            return true;
        }
    }
    return false;
}
```

실제로 이렇게 선언되어 있는 제네릭 메서드의 경우 선언 방식에 따라 컴파일러가 타입 파라미터 E를 실제 유형의 Object로 변경한다

```
public static  boolean containsElement(Object [] elements, Object element){
    for (Object e : elements){
        if(e.equals(element)){
            return true;
        }
    }
    return false;
}
```

따라서 컴파일러는 코드의 형식 안정성을 보장하고 런타임 오류를 방지한다.

### **Type Erasure의 유형**

클래스 수준에서 컴파일러는 클래스의 Type Parameter를 버리고 첫 번째 바인딩으로 대체하거나 Type Parameter가 바인딩 되지 않은 경우 Object로 변환한다

배열을 사용하여 Stack 구현의 예시를 보자

```
public class Stack<E> {
    private E[] stackContent;

    public Stack(int capacity) {
        this.stackContent = (E[]) new Object[capacity];
    }

    public void push(E data) {
        // ..
    }

    public E pop() {
        // ..
    }
}
```

- 컴파일시 컴파일러는 바인딩되지 않은 형식 매개변수 E를 Object로 바꾸게된다

```
public class Stack {
    private Object[] stackContent;

    public Stack(int capacity) {
        this.stackContent = (Object[]) new Object[capacity];
    }

    public void push(Object data) {
        // ..
    }

    public Object pop() {
        // ..
    }
}
```

Type Parameter E가 바인딩 된 경우

```
public class BoundStack<E extends Comparable<E>> {
    private E[] stackContent;

    public BoundStack(int capacity) {
        this.stackContent = (E[]) new Object[capacity];
    }

    public void push(E data) {
        // ..
    }

    public E pop() {
        // ..
    }
}
```

컴파일러는 바인딩 된 형식 매개 변수 E를 첫 번째 바인딩 된 클래스인 Comparable로 대체한다

```
public class BoundStack {
    private Comparable [] stackContent;

    public BoundStack(int capacity) {
        this.stackContent = (Comparable[]) new Object[capacity];
    }

    public void push(Comparable data) {
        // ..
    }

    public Comparable pop() {
        // ..
    }
}
```

### **Method Type Erasure**

- Method Type Erasure의 경우 method-level type erasure 가 저장되지 않고 바인딩되지 않은 경우 부모 형식 Object로 변환되거나 바인딩 될 때 첫 번째 바인딩 된 클래스로 변환된다

**주어진 배열의 내용을 표시하는 예제**

```
public static <E> void printArray(E[] array) {
    for (E element : array) {
        System.out.printf("%s ", element);
    }
}
```

**컴파일시 컴파일러는 Type parameter E를 Object로 바꾼다**

```
public static void printArray(Object[] array) {
    for (Object element : array) {
        System.out.printf("%s ", element);
    }
}
```

**바인딩 된 method type parameter의 경우**

```
public static <E extends Comparable<E>> void printArray(E[] array) {
    for (E element : array) {
        System.out.printf("%s ", element);
    }
}
```

Type Parameter E를 지우고 Comparable로 대체한다

```
public static void printArray(Comparable[] array) {
    for (Comparable element : array) {
        System.out.printf("%s ", element);
    }
}
```

# **과제하면서 느낀점**

- 제네릭에 대해서는 처음 공부를 했는데 어려웠지만 좋은 경험을 했다.

## Reference

- 자바의 신
- https://docs.oracle.com/javase/tutorial/extra/generics/intro.html
- https://docs.oracle.com/javase/tutorial/java/generics/index.html
- https://docs.oracle.com/javase/tutorial/java/generics/types.html
- https://docs.oracle.com/javase/tutorial/extra/generics/methods.html
- http://wonwoo.ml/index.php/post/1743
- https://medium.com/@joongwon/java-java의-generics-604b562530b3
- http://www.tcpschool.com/java/java_generic_various
- https://jyami.tistory.com/99