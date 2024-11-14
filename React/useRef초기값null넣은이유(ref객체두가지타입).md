> useRef의 current.value가 되지않았다.<br>
```
Property 'value' may not exist on type 'string'. Did you mean 'valueOf'?ts(2568)
lib.es5.d.ts(529, 5): 'valueOf' is declared here.
```
이런에러가 눈에 들어왔다.
<br><br><br>
***에러 난 소스코드*** <br>
```javascript
const statusRef = useRef("");

<Form.Label>상태</Form.Label>
<Form.Control as="select" ref={statusRef} defaultValue="">
  <option value="">전체</option>
  <option value="1">동작</option>
  <option value="0">중지</option>
</Form.Control>
```
###### 초기값을 null로 수정해주니 에러가 사라졌다...
<br>

##### useRef로 생성한 ref객체 타입 종류<br>
- 값 저장,DOM 접 용도
  - React.MutableRefObject<T>
  - ex ) const inputRef: React.MutableRefObject<HTMLInputElement>
- DOM 취득 용도
  - React.RefObject<T>
  - ex ) const statusRef: React.RefObject<any>
- 함수형 컴포넌트에서 ref 객체는 기본적으로 MutableRefObject로 만들어진다 
##### 에러 원인<br>
- useRef('') 이렇게 빈문자열로 초기화하면 useRef의 current 프로퍼티가 string으로 인식됨
- 이후 statusRef.current.value 와 같이 속성 접근이 가능하려면 "current가 dom element를 가리키고 있어야함"
- useRef('') 이후 string이라고 해놓고 value 속성을 가져오려니 에러가 난 거였음
##### 결론<br>
- useRef(null)이어야 DOM 엘리먼트가 할당되기 전까지 current가 null로 유지되고, 할당 이후에 DOM 엘리먼트의 타입으로 동작
- typescript야 뭐 제네릭쓰고 초기값 타입잘맞춰서 세팅해야겠지만 typescript 아니면 null로 초기값 설정하기를 권장
  
##### useRef 추가 설명<br>
인자로 넘어온 초깃값을 useRef 객체의 .current 프로퍼티에 저장한다.<br>
- DOM 객체를 직접 가리켜서 내부 값을 변경하거나 focus()를 사용할때 주로 사용<br>
- 값이 변경되어도 컴포넌트가 리렌더링되지 않도록 하기 위한 값들을 저장하기 위해서 사용<br>
typescript에서는 useRef 사용시 반드시 제네릭과 초기값을 설정해야한다.
