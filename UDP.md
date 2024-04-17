| **함수형 인터페이스**  | **메서드 형태**        | **API 활용**                                                  | **매개변수** | **반환값** |
| -------------- | ----------------- | ----------------------------------------------------------- | -------- | ------- |
| Runnable       | void run()        | 매개 변수를 사용 안하고 리턴을 하지 않는 함수 형태로 이용  <br>대표적으로 쓰레드의 매개 변수로 이용 | X        | X       |
| Consumer<T>    | void accept(T t)  | 매개 변수를 사용만 하고 리턴을 하지 않는 함수 형태로 이용                           | O        | X       |
| Supplier<T>    | T get()           | 매개 변수를 사용 안하고 리턴만 하는 함수 형태로 이용                              | X        | O       |
| Function<T, R> | R apply(T t)      | 매개값을 매핑(=타입변환)해서 리턴하기                                       | O        | O       |
| Predicate<T>   | boolean test(T t) | 매개값이 조건에 맞는지 단정해서 boolean 리턴                                | O        | O       |
| Operator       | R applyAs(T t)    | 매개값을 연산해서 결과 리턴하기                                           | O        | O       |
**메서드 형태**
**API 활용**
**매개변수**
**반환값**

Runnable
void run()
매개 변수를 사용 안하고 리턴을 하지 않는 함수 형태로 이용  
대표적으로 쓰레드의 매개 변수로 이용
X
X

Consumer<T>
void accept(T t)
매개 변수를 사용만 하고 리턴을 하지 않는 함수 형태로 이용
O
X

Supplier<T>
T get()
매개 변수를 사용 안하고 리턴만 하는 함수 형태로 이용
X
O

Function<T, R>
R apply(T t)
매개값을 매핑(=타입변환)해서 리턴하기
O
O

Predicate<T>
boolean test(T t)
매개값이 조건에 맞는지 단정해서 boolean 리턴
O
O

Operator
R applyAs(T t)
매개값을 연산해서 결과 리턴하기
O
O

출처: [https://inpa.tistory.com/entry/☕-함수형-인터페이스-API](https://inpa.tistory.com/entry/%E2%98%95-%ED%95%A8%EC%88%98%ED%98%95-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-API) [Inpa Dev 👨‍💻:티스토리]