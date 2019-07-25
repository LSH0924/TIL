# Promise
- ES6 문법에서 비동기 처리를 다루는 데 사용하는 객체
- 콜백 지옥에 빠지기 쉬운 코드를 보기쉽게 정리할 수 있다.
- `resolve(결과값)` : Promise에서 결과값을 반환할 때 사용. `.then()`으로 받는다.
- `reject(오류)` : Promise에서 오류를 발생시킬 때 사용. `.catch()`로 받는다.

```javascript
const arr = ["사과", "바나나", "복숭아", "수박", "메론", "체리"];

function print(num, fn){
    setTimeout(()=> {
        console.log(arr[num++]);
        if(fn) fn();
        }, 1000);
}

// 콜백지옥...
print(0, () => 
    print(1, ()=>
        print(2, ()=>
            print(3)
        )
    )
);

// promise 객체를 반환하는 함수 만들기
function printPromise(num){
    return new Promise(
        (resolve, reject) => { // 정상결과, 오류를 파라미터로 받는다.
            if(num > arr.length-1) return reject("인덱스가 배열의 크기보다 큽니다.");
            setTimeout(()=>{
                console.log(arr[num]);
                resolve(num + 1);
            }, 1000);
        }
    );
}

printPromise(0)
.then(num => printPromise(num))
.then(num => printPromise(num))
.then(num => printPromise(num))
.then(num => printPromise(num))
.then(num => printPromise(num))
.then(num => printPromise(num))
.catch(e => console.log(e));

// **결과**
// 사과
// 바나나
// 복숭아
// 수박
// 메론
// 체리
// 인덱스가 배열의 크기보다 큽니다.
```