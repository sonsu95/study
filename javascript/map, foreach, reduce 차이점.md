## Map
1. 배열의 각 요소에 대해 특정 함수를 적용하고, 그 결과를 포함하는 **새 배열을 생성**
2. **변환**이나 **매핑** 작업에 가장 적합
3. Map을 사용하면 **원본 배열은 변하지 않음**.
```javascript
let numbers = [1, 2, 3, 4, 5];
let squared = numbers.map(num => num * num); // [1, 4, 9, 16, 25]
```


## forEach
1. 배열의 각 요소에 대해 특정 함수를 실행.
2. 결과를 반환하지 않으며, 원래의 배열에 영향을 줄 수 있음
3. 주로 배열의 요소를 순회하며 특정작업을 수행할 떄 사용
```javascript
let numbers = [1, 2, 3, 4, 5];
numbers.forEach(num => console.log(num));
```


## reduce
1. 배열의 모든 요소에 대해 특정 함수를 실행하고, 단일 출력값을 생성
2. 배열의 요소를 집계하거나 결합할 때 사용.
```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((total, num) => total + num, 0); // 15
```

<br>

---

### exmaples...
1. array에 push하는 작업을 실행할 경우 forEach를 사용하면 원본 함수를 직접 수정하는 상황이 되어버리기 때문에
map, filter, reduce 등의 메서드를 사용하는 것이 좋다. 
