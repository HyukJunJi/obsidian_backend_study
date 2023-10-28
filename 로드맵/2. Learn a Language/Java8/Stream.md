---
tags:
  - Java8
---
### 스트림
스트림은 크게 세 가지로 나눌 수 있습니다
1. 생성하기 : 스트림 인스턴스 생성.
2. 가공하기 : 필터링(filtering) 및 맵핑(mapping) 등 원하는 결과를 만들어가는 중간 작업 (intermediage operations)
3. 결과 만들기 : 최종적으로 결과를 만들어내는 작업 (terminal operations)
```
전체 -> 맵핑 -> 필터링 1 -> 필터링 2 -> 결과 만들기 -> 결과물
```

## 1. 생성하기 

보통 ==배열==과 ==컬렉션==을 이용해서 스트림을 만들지만 이 외에도 다양한 방법으로 스트림을 만들 수 있습니다. 하나씩 살펴보겠습니다.

### 배열 스트림
스트림을 이용하기 위해서는 먼저 생성을 해야 합니다. 
스트림은 ==배열== 또는 ==컬렉션== ==인스턴스==를 이용해서 생성할 수 있습니다. 배열은 다음과 같이
<code>Arrays.stream</code> 메소드를 사용합니다.

```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3); // 1~2 요소[b,c]
```

### 컬렉션 스트림
컬렉션 타입(Collection, List, Set)의 경우 인터페이스에 추가된 디폴트 메소드 <code>stream</code>을 이용해서 스트림을 만들 수 있습니다.
```java
public interface Collection<E> extends Iterable<E> {
	default Stream<E> stream() {
		return StreamSupport.stream(spliterator(), false);
	}
	//..
}
```
위에 메소드를 이용해서 생성하게 된다면
```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
Stream<String> parallelStream = list.parallelStream();// 병렬 처리 스트림
```
### 비어 있는 스트림

비어 있는 스트림 (empty streams)도 생성할 수 있습니다.
빈 스트림은 요소가 없을 때 <code>null</code> 대신 사용할 수 있습니다.
```java
public Stream<String> streamOf(List<String> list) {
	return list == null || list.isEmpty()
		? Stream.empty()
		: list.stream();
}
```
### Stream.builder()
빌더 (Builder)를 사용하면 스트림에 직접적으로 원하는 값을 넣을 수 있습니다. 마지막에 <code>build</code> 메소드로 스트림을 리턴합니다.
```java
Stream<String> builderStream = Stream.<String>builder()
	.add("Eric").add("Elena").add("Java")
	.build();
```
### Stream.generate()
`generate` 메소드를 이용하면 `Supplier<T>` 에 해당하는 람다로 값을 넣을 수 있습니다. `Supplier<T>` 는 인자는 없고 리턴값만 있는 함수형 인터페이스죠. 람다에서 리턴하는 값이 들어갑니다.
```java
public static<T> Stream<T> generate(Supplier<T> s) { ... }
```

`generate()`를 사용하면 스트림의 크기가 정해져있지 않고 무한하기 떄문에 특정 사이즈로 최대 크기를 제한해야 합니다.

```java
Stream<String> generatedStream = 
	Stream.generate(() -> "gen").limit(5);
```
5개의 "gen"이 들어간 스트림이 생성됩니다.
### Stream.iterate()
`iterate` 메소드를 이용하면 초기값과 해당 값을 다루는 람다를 이용해서 스트림에 들어갈 요소를 만듭니다.
```java
Stream<Integer> iteratedStream = 
	Stream.iterate(30, n -> n + 2).limit(5);
```
***30이 초기값이고 값이 2씩 증가하는 값들이 들어가게 됩니다. 즉 요소가 다음 요소의 인풋으로 들어갑니다. 이 방법도 스틀미의 사이즈가 무한하기 떄문에 특정 사이즈로 제한해야 합니다.

### 기본 타입형 스트림
물론 제네릭을 사용하면 리스트나 배열을 이용해서 기본 타입(_int, long, double_) 스트림을 생성할 수 있습니다. 하지만 제네릭을 사용하지 않고 직접적으로 해당 타입의 스트림을 다룰 수도 있습니다. `range` 와 `rangeClosed` 는 범위의 차이입니다. 두 번째 인자인 종료지점이 포함되느냐 안되느냐의 차이입니다.
```java
IntStream intStream = IntStream.range(1, 5);
LongStream longStream = LongStream.rangeClosed(1, 5);
```

제네릭을 사용하지 않기 때문에 불필요한 오토박싱(_auto-boxing_)이 일어나지 않습니다. 필요한 경우 `boxed` 메소드를 이용해서 박싱(_boxing_)할 수 있습니다.
```java
Stream<Integer> boxedIntStream = IntStream.range(1, 5).boxed();
```

===Random=== 클래스를 사용하여 스트림을 생성할때 난수 스트림을 생성할 수 있다.
```java
DoubleStream doubles = new Random().doubles(3);
```

### 파일 스트림
자바 NIO의 `Files` 클래스이 `lines` 메소드는 해당 파일의 각 라인을 스트링 타입의 스트림으로 만들어줍니다.
```java
Stream<String> lineStream = 
	Files.lines(Paths.get("file.txt")),
		Charset.forName("UTF-8");
```

### 스트림 연결하기
`Stream.concat` 메소드를 이용해 두 개의 스트림을 연결해서 새로운 스트림을 만들어낼 수 있습니다.
```java
Stream<String> stream1 = Stream.of("java", "Scala", "Groovy");
Stream<String> stream2 = Stream.of("Python", "Go", "Swift");
Stream<String> concat = Stream.concat(stream1, stream2);
```

## 2. 가공하기
전체 요소 중에서 다음과 같은 API를 이용해서 내가 원하는 것만 뽑아 낼 수 있습니다. 이러한 작업은 스트림을 리턴하기 때문에 려거 작업을 이어 붙여서(chaining) 작성을 할 수 있습니다.
```java
List<String> names = Arrays.asList("A","B","C");
```

### Filtering
필터(filter)은 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업입니다. 인자로 받는 Predicate는 boolean을 리턴하는 함수형 인터페이스로 평가식이 들어가게 됩니다.
```java
Stream<T> filter(Predicate<? super T> predicate);
```
```java
Stream<String> stream = names.stream()
	.filter(name -> name.contains("A"));
```
스트림의 각 요소에 대해서 평가식을 실행하게 되고 "A"가 들어간 이름이 들어간 스트림이 리턴됩니다.

### Mapping
맵(map)은 스트림 내 요소들을 하나씩 특정 값으로 변환해줍니다. 이 때 값을 변환하기 위한 람다를 인자로 받습니다.
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```
스트림에 들어가 있는 값이 input이 되어서 특정 로직을 거친 후 output이 되어 (리턴되는) 새로운 스트림에 담기게 됩니다. 이러한 작업을 맵핑(mapping)이라고 합니다.
```java
Stream<String> stream = names.stream()
	.map(String::toUppserCase);
	//.map(N -> N.toUppserCase );
```

```java
Stream<Integer> stream = 
	productList.stream()
	.map(Product::getAmount);
```

`map` 이외에도 조금 더 복잡한 `flatMap` 메소드도 있습니다.

```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
```
인자로 `mapper`를 받고 있는데, 리턴 타입이 Stream 입니다. 즉, 새로운 스트림을 생성해서 리턴하는 람다를 넘겨야합니다. `flatMap` 은 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어주는 역할을 합니다. 이러한 작업을 플래트닝(_flattening_)이라고 합니다.

다음과 같은 중첩된 리스트가 있습니다.

```java
List<List<String>> list = 
	Arrays.asList(Arrays.asList("a"),
				Arrays.asList("b"));
//[[a],[b]]
```

이를 `flatMap`을 사용해서 중첩 구조를 제거한 후 작업할 수 있습니다.
```java
List<String> flatList =   
  list.stream()  
  .flatMap(Collection::stream)  
  .collect(Collectors.toList());  
// [a, b]
```

### Sorting
정렬의 방법은 다른 정렬과 마찬가지로 Comparator를 이용합니다.
```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```
인자 없이 그냥 호출할 경우 오름차순으로 정렬합니다.
```java
IntStream.of(14, 11, 20, 39, 23)
	.sorted()
	.boxed()
	.collect(collectors.toList());
```
인자를 넘기는 경우와 비교해보겠습니다. 스트링 리스트에서 알파벳 순으로 정렬한 코드와 Comparator 를 넘겨서 역순으로 정렬한 코드입니다.
```java
List<String> lang =   
  Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");  
  
lang.stream()  
  .sorted()  
  .collect(Collectors.toList());  
// [Go, Groovy, Java, Python, Scala, Swift]  
  
lang.stream()  
  .sorted(Comparator.reverseOrder())  
  .collect(Collectors.toList());  
// [Swift, Scala, Python, Java, Groovy, Go]
```

기본적으로 Comparator 사용법과 동일합니다. 이를 이용해서 문자열 길이를 기준으로 정렬해보겠습니다.

```java
lang.stream()  
  .sorted(Comparator.comparingInt(String::length))  
  .collect(Collectors.toList());  
// [Go, Java, Scala, Swift, Groovy, Python]  
  
lang.stream()  
  .sorted((s1, s2) -> s2.length() - s1.length())  
  .collect(Collectors.toList());  
// [Groovy, Python, Scala, Swift, Java, Go]
```
### Iterating

스트림 내 요소들 각각을 대상으로 특정 연산을 수행하는 메소드로는 `peek` 이 있습니다. ‘_peek_’ 은 그냥 확인해본다는 단어 뜻처럼 특정 결과를 반환하지 않는 함수형 인터페이스 Consumer 를 인자로 받습니다.

```
Stream<T> peek(Consumer<? super T> action);
```

따라서 스트림 내 요소들 각각에 특정 작업을 수행할 뿐 결과에 영향을 미치지 않습니다. 다음처럼 작업을 처리하는 중간에 결과를 확인해볼 때 사용할 수 있습니다.
```
int sum = IntStream.of(1, 3, 5, 7, 9)  
  .peek(System.out::println)  
  .sum();
```

## 결과 만들기
가공한 스트림을 가지고 내가 사용할 결과값으로 만들어내는 단계입니다. 따라서 스트림을 끝내는 최종 작업(_terminal operations_)입니다.

##### count,sum
```java
long count = IntStream.of(1, 3, 5, 7, 9).count();  
long sum = LongStream.of(1, 3, 5, 7, 9).sum();
```
##### min,max
```java
OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min();  
OptionalInt max = IntStream.of(1, 3, 5, 7, 9).max();
```
스트림이 빈 경우 `count`,`sum`은 0을 출력하면 됩니다. 하지만 평균, 최소, 최대의 경우에는 표현할 수가 없기 때문에 Optional을 이용해 리턴합니다.
스트림에서는 ==`ifPresent`== 메소드를 이용해서 Optional을 처리할 수 있습니다.
```java
DoubleStream.of(1.1, 2.2, 3.3, 4.4, 5.5)  
  .average()  
  .ifPresent(System.out::println);
```
이 외에도 사용자가 원하는대로 결과를 만들어내기 위해 `reduce`와 `collect`메소드를 제공합니다.
### Reduction
스트림은 `reduce`라는 메소드를 이용해서 결과를 만들어냅니다.
==`reduce`== 메소드는 총 세 가지 파라미터를 받을 수 있습니다.
- ==`accumulator`== : 각 요소를 처리하는 계산 로직. 각 요소가 올 때마다 중간 결과를 생성하는 로직.
- ==`identity`== : 계산을 위한 초기값으로 스트림이 비어서 계산할 내용이 없더라도 이 값은 리턴.
- ==`combiner`== : 병렬(_parallel_) 스트림에서 나눠 계산한 결과를 하나로 합치는 동작하는 로직.
```java
// 1개 (accumulator)  
Optional<T> reduce(BinaryOperator<T> accumulator);  
  
// 2개 (identity)  
T reduce(T identity, BinaryOperator<T> accumulator);  
  
// 3개 (combiner)  
<U> U reduce(U identity,  
  BiFunction<U, ? super T, U> accumulator,  
  BinaryOperator<U> combiner);
```

먼저 인자가 하나만 있는 경우입니다. 여기서 ==`BinaryOperator<T>`== 는 같은 타입의 인자 두 개를 받아 같은 타입의 결과를 반환하는 함수형 인터페이스입니다. 다음 예제에서는 두 값을 더하는 람다를 넘겨주고 있습니다. 따라서 결과는 6(_1 + 2 + 3_)이 됩니다.
###### ==`BinaryOperator<T>`==
```java
OptionalInt reduced =   
  IntStream.range(1, 4) // [1, 2, 3]  
  .reduce((a, b) -> {  
    return Integer.sum(a, b);  
  });
```
이번엔 두 개의 인자를 받는 경우입니다. 여기서 10은 초기값이고, 스트림 내 값을 더해서 결과는 16(_10 + 1 + 2 + 3_)이 됩니다. 여기서 람다는 [메소드 참조](https://futurecreator.github.io/2018/08/02/java-lambda-method-references/)(_method reference_)를 이용해서 넘길 수 있습니다. 
```java
int reducedTwoParams =   
  IntStream.range(1, 4) // [1, 2, 3]  
  .reduce(10, Integer::sum); // method reference
```
마지막으로 세 개의 인자를 받는 경우입니다. Combiner 가 하는 역할을 설명만 봤을 때는 잘 이해가 안갈 수 있는데요, 코드를 한번 살펴봅시다. 그런데 다음 코드를 실행해보면 이상하게 마지막 인자인 combiner 는 실행되지 않습니다.

```java
Integer reducedParallel = Arrays.asList(1, 2, 3)  
  .parallelStream()  
  .reduce(10,  
          Integer::sum,  
          (a, b) -> {  
            System.out.println("combiner was called");  
            return a + b;  
          });
```
결과는 다음과 같이 36이 나옵니다. 먼저 accumulator 는 총 세 번 동작합니다. 초기값 10에 각 스트림 값을 더한 세 개의 값(_10 + 1 = 11, 10 + 2 = 12, 10 + 3 = 13_)을 계산합니다. Combiner 는 identity 와 accumulator 를 가지고 여러 쓰레드에서 나눠 계산한 결과를 합치는 역할입니다. 12 + 13 = 25, 25 + 11 = 36 이렇게 두 번 호출됩니다.

```
combiner was called  
combiner was called  
36
```

### Collecting
`collect` 메소드는 또 다른 종료 작업입니다. `Collector` 타입의 인자를 받아서 처리를 하는데요, 자주 사용하는 작업은 `Collectors` 객체에서 제공하고 있습니다.

이번 예제에서는 다음과 같은 간단한 리스트를 사용합니다. Product 객체는 수량(_amout_)과 이름(_name_)을 가지고 있습니다.
```java
List<Product> productList =   
  Arrays.asList(new Product(23, "potatoes"),  
                new Product(14, "orange"),  
                new Product(13, "lemon"),  
                new Product(23, "bread"),  
                new Product(13, "sugar"));
```

| id  | 나이 | 학교 |
| --- | ---- | ---- |
| 1   | 10   | ㅇㅇ |
| 2   | 20   | ㅇㅇ |
| 3   | 30   | ㄱㄱ |
|     |      |      |
findby학교(ㅇㅇ)
#### _Collectors.toList()_

스트림에서 작업한 결과를 담은 리스트로 반환합니다. 다음 예제에서는 `map` 으로 각 요소의 이름을 가져온 후 `Collectors.toList` 를 이용해서 리스트로 결과를 가져옵니다.

```java
List<String> collectorCollection =  
  productList.stream()  
    .map(Product::getName)  
    .collect(Collectors.toList());  
// [potatoes, orange, lemon, bread, sugar]
```
#### Collectors.joining()
스트림에서 작업한 결과를 하나의 스트링으로 이어 붙일 수 있습니다.
```java
String listToString =   
 productList.stream()  
  .map(Product::getName)  
  .collect(Collectors.joining());  
// potatoesorangelemonbreadsugar
```

`Collectors.joining` 은 세 개의 인자를 받을 수 있습니다. 이를 이용하면 간단하게 스트링을 조합할 수 있습니다.

- delimiter : 각 요소 중간에 들어가 요소를 구분시켜주는 구분자
- prefix : 결과 맨 앞에 붙는 문자
- suffix : 결과 맨 뒤에 붙는 문자
```java
String listToString =   
 productList.stream()  
  .map(Product::getName)  
  .collect(Collectors.joining(", ", "<", ">"));  
// <potatoes, orange, lemon, bread, sugar>
```
#### Collectors.averageingInt()
숫자 값(Integer value) 의 평균 (arithmetic mean)을 냅니다.
```java
Double averageAmount =   
 productList.stream()  
  .collect(Collectors.averagingInt(Product::getAmount));  
// 17.2
```

#### Collectors.summingInt()
숫자값의 합을(sum)을 냅니다.
```java
Integer summingAmount =   
 productList.stream()  
  .collect(Collectors.summingInt(Product::getAmount));  
// 86
```
#### Collectors.groupingBy()
특정 조건으로 요소들을 그룹지을 수 있습니다. 수량을 기준으로 그룹핑해보겠습니다. 여기서 받는 이자는 함수형 인터페이스 Function입니다.
```java
Map<Integer, List<Product>> collectorMapOfLists =  
 productList.stream()  
  .collect(Collectors.groupingBy(Product::getAmount));
```

결과는 ==***Map***== 타입으로 나오는데요, 같은 수량이면 리스트로 묶어서 보여줍니다.

```java
{23=[Product{amount=23, name='potatoes'},   
     Product{amount=23, name='bread'}],   
 13=[Product{amount=13, name='lemon'},   
     Product{amount=13, name='sugar'}],   
 14=[Product{amount=14, name='orange'}]}
```

#### _Collectors.partitioningBy()
위의 `groupingBy` 함수형 인터페이스 Function 을 이용해서 특정 값을 기준으로 스트림 내 요소들을 묶었다면, `partitioningBy` 은 함수형 인터페이스 Predicate 를 받습니다. Predicate 는 인자를 받아서 boolean 값을 리턴합니다.

```java
Map<Boolean, List<Product>> mapPartitioned =  
productList.stream()  
.collect(Collectors.partitioningBy(el -> el.getAmount() > 15));
```
```java
//결과 값
{false=[Product{amount=14, name='orange'},   
        Product{amount=13, name='lemon'},   
        Product{amount=13, name='sugar'}],   
 true=[Product{amount=23, name='potatoes'},   
       Product{amount=23, name='bread'}]}
```
#### _Collector.of()_

여러가지 상황에서 사용할 수 있는 메소드들을 살펴봤습니다. 이 외에 필요한 로직이 있다면 직접 collector 를 만들 수도 있습니다. accumulator 와 combiner 는 `reduce` 에서 살펴본 내용과 동일합니다.

```java
public static<T, R> Collector<T, R, R> of(  
  Supplier<R> supplier, // new collector 생성  
  BiConsumer<R, T> accumulator, // 두 값을 가지고 계산  
  BinaryOperator<R> combiner, // 계산한 결과를 수집하는 함수.  
  Characteristics... characteristics) { ... }
```

코드를 보시면 더 이해가 쉬우실 겁니다. 다음 코드에서는 collector 를 하나 생성합니다. 컬렉터를 생성하는 supplier 에 LinkedList 의 생성자를 넘겨줍니다. 그리고 accumulator 에는 리스트에 추가하는 `add` 메소드를 넘겨주고 있습니다. 따라서 이 컬렉터는 스트림의 각 요소에 대해서 LinkedList 를 만들고 요소를 추가하게 됩니다. 마지막으로 combiner 를 이용해 결과를 조합하는데, 생성된 리스트들을 하나의 리스트로 합치고 있습니다.

```java
Collector<Product, ?, LinkedList<Product>> toLinkedList =   
  Collector.of(LinkedList::new,   
               LinkedList::add,   
               (first, second) -> {  
                 first.addAll(second);  
                 return first;  
               });

```

따라서 다음과 같이 `collect` 메소드에 우리가 만든 커스텀 컬렉터를 넘겨줄 수 있고, 결과가 담긴 LinkedList 가 반환됩니다.

```java
LinkedList<Product> linkedListOfPersons =   
  productList.stream()  
  .collect(toLinkedList);
```

#### Matching
매칭은 조건식 람다 Predicate 를 받아서 해당 조건을 만족하는 요소가 있는지 체크한 결과를 리턴합니다. 다음과 같은 세 가지 메소드가 있습니다.

- 하나라도 조건을 만족하는 요소가 있는지(_anyMatch_)
- 모두 조건을 만족하는지(_allMatch_)
- 모두 조건을 만족하지 않는지(_noneMatch_)

```java
boolean anyMatch(Predicate<? super T> predicate);  
boolean allMatch(Predicate<? super T> predicate);  
boolean noneMatch(Predicate<? super T> predicate);
```

간단한 예제입니다. 다음 매칭 결과는 모두 `true` 입니다.

```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");  
  
boolean anyMatch = names.stream()  
  .anyMatch(name -> name.contains("a"));  
boolean allMatch = names.stream()  
  .allMatch(name -> name.length() > 3);  
boolean noneMatch = names.stream()  
  .noneMatch(name -> name.endsWith("s"));
```

### Iterating

`foreach` 는 요소를 돌면서 실행되는 최종 작업입니다. 보통 `System.out.println` 메소드를 넘겨서 결과를 출력할 때 사용하곤 합니다. 앞서 살펴본 `peek` 과는 중간 작업과 최종 작업의 차이가 있습니다.

```java
names.stream().forEach(System.out::println);
```
[[스트림의 구조] Stream in depth : Stream & Pipeline (tistory.com)](https://namocom.tistory.com/778)
[출처](https://futurecreator.github.io/2018/08/26/java-8-streams/)