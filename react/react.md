# React


# # 목차

1. SPA / CSR / VirtualDOM
2. JSX
3. Props
4. Component
5. React Hooks (useState, useEffect, useRef, useMemo, useCallback)

# 1. SPA (Single Page Application)

- 말그대로 단일 페이지로 구성된 애플리케이션으로 Client Side Rendering 방식을 사용
    - **Client Side Rendering**
        
        > 페이지의 내용을 브라우저에서 동적으로 생성하는 방식으로
        최초 페이지 로딩시 데이터를 서버에서 한번에 받아와서 HTML 파일을 만들고, 
        이후에는 필요한 데이터만 부분적으로 업데이트하는 방식
        **Virtual DOM**을 이용하기 때문에 가능한 방식
        > 
    - **Virtual DOM**에 대한 설명 이전에 알아야 할 **브라우저 렌더링** 과정
        
        > 0. 서버 요청 및 응답
        > 1. HTML을 파싱해서 DOM Tree 생성
        > 2. CSS를 파싱해서 CSSOM Tree 생성
        > 3. 1과 2를 바탕으로 브라우저에 출력되야할 노드들에 대한 정보인 Render Tree생성
        > 4. 위치를 계산해서 배치하는 Layout(Reflow)
        > 5. 실제 픽셀로 그리는 단계인 Painting(Repainting)
        > 6. 페인팅 과정에서의 각 레이어를 합성해서 최종 이미지를 만드는 Compositing(합성)단계
        > 
    - DOM을 수정할 때마다 렌더트리생성부터 Reflow, Repaint의 과정을 다시 수행하는데 이 때 브라우저 성능 저하가 발생한다
        
        > 기하학적 변화 없이 CSS가 변경되면 페이지 요소의 외관만 변경하는 **Repaint** 발생,
        > 기하학적 변화가 있으면 레이아웃을 변경하고 크기나 위치를 재계산한 뒤 배치하는 **Reflow** 발생
        > ⇒ **Reflow**가 훨씬 더 많은 리소스를 사용한다
        > ⇒ 성능 저하를 막으려면 DOM을 최소한으로 수정해야 하는데 이를 위해  Virtual DOM을 사용
        > 
    - **Virtual DOM**
        
        > DOM을 추상화 시킨 객체로 최초 로딩시 생성한 뒤 실제 DOM의 상태를 추적함
        상호작용이 발생하거나 데이터가 변경되면 새로운 가상돔을 생성한 뒤 연산을 수행함,
        이전 가상DOM과 현재 가상DOM의 차이점을 찾아서 실제 DOM에 업데이트하는 과정을 거침
        > 
- 장점
    - 깜빡임 이슈가 없어서 사용자 경험이 좋아짐
    - 클라이언트에서 작업을 처리하기 때문에 서버의 부담이 줄어듬
    - 새로운 요청이 있으면 필요한 부분만 갱신하기 때문에 사용성이 좋아짐
    - 동적으로 DOM을 생성하기 때문에 TTV와 TTI의 간극이 없다
- 단점
    - 초기 로딩이 느리다
    - 검색엔진최적화에 좋지않다

---

[https://velog.io/@1nthek/React-Virtual-DOM과-렌더링](https://velog.io/@1nthek/React-Virtual-DOM%EA%B3%BC-%EB%A0%8C%EB%8D%94%EB%A7%81)

[https://wrtn.ai/](https://wrtn.ai/) - ‘**브라우저 렌더링 과정에 대해 설명해줘’**

---

# 2. JSX

- JS코드 안에서 XML과 유사한 문법을 사용해 UI를 작성할 수 있게 해주는 확장 문법
⇒ JS에서 HTML과 유사한 코드를 작성할 수 있도록 해줌
    
    ```jsx
    const element = <h1>Hello, world!</h1>;
    ```
    
- JSX안에서 중괄호{}를 사용하여 데이터바인딩할 수 있음
⇒ 변수, 함수 호출, 연산 등을 JSX 안에서 실행하고 결과를 동적으로 표현할 수 있음
    
    ```jsx
    const name = "John";
    const greeting = <p>Hello, {name}!</p>;
    ```
    
- 태그에 속성을 설정할 수 있음
    
    ```jsx
    const button = <button type="submit" onClick={handleClick}>Submit</button>;
    ```
    
- 컴포넌트를 정의하고 사용할 수 있음
    
    ```jsx
    function Greeting(props) {
      return <h1>Hello, {props.name}!</h1>;
    }
    
    const App = () => {
      return (
        <div>
          <Greeting name="John" /> 
          <Greeting name="Jane" />
        </div>
      );
    }
    
    // ===>

    // <div>
    //   <h1>Hello, John!</h1>
    //   <h1>Hello, Jane!</h1>
    // </div>
    ```
    

# 3. Props

- 컴포넌트 간에 정보를 전달하는 데 사용되는 데이터 객체
- 부모 컴포넌트에서 자식 컴포넌트에 데이터를 전달할 수 있고 자식 컴포넌트에서 사용 가능
    
    ```jsx
    // App.js
    import React from 'react';
    import Hello from './Hello';
    
    function App() {
      return (
        <Hello name="react" />
      );
    }
    
    export default App;
    ```
    
    ```jsx
    // Hello.js
    import React from 'react';
    
    function Hello(props) {
      return <div>안녕하세요 {props.name}</div>
    }
    
    export default Hello;
    ```
    

# 4. Component

- React앱의 가장 작은 구성요소로 Flutter의 Widget과 유사하다
- UI를 구성하고 조작하는데 사용되며 재사용이 가능하고, 개별적인 상태와 속성을 가질 수 있다
- 클래스형 컴포넌트와 함수형 컴포넌트로 사용된다
    - **클래스형 컴포넌트**
        
        ```jsx
        import React, { Component } from "react";
        
        export default class MyComponent extends Component {
          render() {
            return <div>Hello, World!</div>;
          }
        }
        // class 키워드, 'Component' 상속, render() 메소드 필요
        
        // 예시
        
        import React, { Component } from 'react';
        
        class MyComponent extends Component {
          constructor(props) {
            super(props);
            this.state = {
              count: 0,
            };
          }
        
          componentDidMount() {
            // 컴포넌트가 마운트된 후에 실행되는 코드
            console.log('컴포넌트가 마운트되었습니다.');
          }
        
          componentDidUpdate(prevProps, prevState) {
            // 컴포넌트가 업데이트된 후에 실행되는 코드
            console.log('컴포넌트가 업데이트되었습니다.');
          }
        
          componentWillUnmount() {
            // 컴포넌트가 언마운트되기 전에 실행되는 코드
            console.log('컴포넌트가 언마운트됩니다.');
          }
        
          incrementCount() {
            this.setState(prevState => ({
              count: prevState.count + 1,
            }));
          }
        
          render() {
            const { count } = this.state;
        
            return (
              <div>
                <h1>카운트: {count}</h1>
                <button onClick={() => this.incrementCount()}>증가</button>
              </div>
            );
          }
        }
        
        export default MyComponent;
        
        ```
        
        - 클래스형 컴포넌트의 단점
            1. 복잡하다
            상태와 생명주기 관리를 위해 별도의 메서드들을 정의하고 관리해야하기 때문에 코드가 길고 복잡하며 가독성이 떨어진다
            2. 자바스크립트 클래스 문법에 대한 이해가 필요
            클래스 상속, 생성자, 메서드 정의 등의 개념을 이해해야 하고 이는 학습 곡선을 높일 수 있고, 초보자에게는 진입장벽이 될 수 있음
            3. this
            function을 쓰고 안 쓰고에 따라서 this가 바뀌고, this 유무에 따라 이벤트 핸들러 등록 방식이 다르다.
            4. 재사용성
            상속을 통해 컴포넌트를 재사용하는 방식이 주로 사용되는데 이는 컴포넌트간의 강한 결합을 만들 수 있으나 보다 유연한 컴포넌트 구조를 만들기 어렵게 할 수 있음
            - 이런 단점들이 있지만 상태와 생명주기 메소드 때문에 사용했어야했음
    - **함수형 컴포넌트** 
    → React Hooks를 통해 상태관리가 가능해졌다 
    → (함수형컴포넌트 + Hook) = 리액트 공식문서에서 권장하는 방식
        
        ```jsx
        import React from 'react';
        
        function MyComponent() {
          return (
        		<>
        		 Hello, World!
        		</>
        	)
        *}*
        ```
        

# 5. React Hooks

- 함수형 컴포넌트는 간단하게 함축적인 프로그래밍이 가능하지만 State와 LifeCycle을 다룰 수 없었는데 React 16.8 버전에 추가된 Hooks를 통해 해당 기능들을 이용할 수 있게 해주는 API
- **useState**
상태 관리
    
    ```jsx
    const [state, setState] = useState(initialState);
    // state : 현재 상태 값
    // setState : 상태 업데이트 함수, 호출시 컴포넌트 렌더링
    // initialState : 초기 상태 값, 함수를 전달하면 첫 렌더링때만 실행됨
    ```
    
    ```jsx
    // 예제
    import React, { useState } from 'react';
    
    function Example() {
    
      const [count, setCount] = useState(0);
    
      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            Click me
          </button>
        </div>
      );
    }
    ```
    

- **useEffect**
생명 주기
    
    ```jsx
    useEffect(() => {
    	// 1-1
      console.log('컴포넌트가 화면에 나타남');
    	// 1-2
      return () => {
        console.log('컴포넌트가 화면에서 사라짐');
      };
    }, []);
    
    // 첫 번째 파라미터 : 함수
    // 1-1(함수) :  마운트 됐을 때 실행
    // 1-2(Cleanup함수) : 언마운트 됐을 때 실행
    
    // 두 번째 파라미터 : 배열
    // 배열 내에 값이 있다면 해당 값들이 변경될 때 useEffect에 등록한 함수 재수행
    // 배열이 비어있다면 컴포넌트가 처음 나타날 때만 useEffect에 등록한 함수를 호출
    ```
    
    ```jsx
    // 1. Component가 Mount 되었을 때(나타날 때)
    // 1-1. 두번째 인자를 생략하면 해당 컴포넌트가 렌더링 될 때마다 useEffect가 실행됨
    useEffect(() => {
      console.log("렌더링 될때마다 실행");
    });
    
    // 1-2. 처음 렌더링 될 때 한 번만 실행하고 싶다면 빈 배열을 넣어주기
    useEffect(() => {
      console.log("맨 처음 렌더링될 때 한 번만 실행");
    },[]);
    
    // 2. Component가 Update 되었을 때(props, state 변경)
    useEffect(() => {
      console.log(name);
      console.log("name이라는 값이 업데이트 될 때만 실행");
    },[name]);
    
    // 2-1 업데이트 뿐 아니라 마운트 될 때도 실행되므로 업데이트 될 때만 실행하고 싶다면 다음과 같이 변경
    const mounted = useRef(false);
    useEffect(() => {
      if (!mounted.current) {
        mounted.current = true;
      } else {
        console.log(name);
        console.log("업데이트 될 때마다 실행");
      }
    }, [name]);
    
    // 3. Component가 Unmount 되었을 때(사라질 때) & Update 되기 직전에
    useEffect(() => {
      console.log("컴포넌트 나타남");
      console.log(name);
      return () => {
        console.log("cleanUp");
      };
    });
    
    // Unmount 될 때만 cleanup 하고 싶다면 빈 배열을,
    // 특정 값이 업데이트되기 직전에 cleanup 하고 싶다면 배열에 해당 값을 넣어주면 됨
    ```
    
- **useRef**
    
    ```jsx
    const ref = useRef(initialValue);
    // initialValue는 초기 참조 값 (선택 사항)
    // ref.current는 현재 저장된 참조 값
    ```
    
- 보통은 DOM에 접근하기위해 사용(예제1)하고 
리렌더링이 필요하지 않은경우(예제2)에도 사용한다
[링크](https://velog.io/@jinyoung985/React-useRef%EB%9E%80)
    
    ```jsx
    // 예제1 : 텍스트필드 포커스 이동
    import React, { useRef } from 'react';
    
    function TextInputWithFocus() {
      const inputRef1 = useRef(null);
      const inputRef2 = useRef(null);
    
      function handleFocus1() {
        inputRef2.current.focus();
      }
      function handleFocus2() {
        inputRef1.current.focus();
      }
    
      return (
    		<>
    	    <div>
    	      <input ref={inputRef1} type="text" />
    	      <button onClick={handleFocus1}>클릭</button>
    	    </div>
    			<div>
    	      <input ref={inputRef2} type="text" />
    	      <button onClick={handleFocus2}>클릭</button>
        </div>
    		</>
      );
    }
    
    export default TextInputWithFocus;
    
    // ---
    
    // 예제 2 : 값 (링크 참고하기)
    import { useState, useRef } from "react";
    import "./styles.css";
    
    function App() {
      const [stateCount, setStateCount] = useState(0);
      const refCount = useRef(0);
      let varCount = 0;
    
      function upState() {
        setStateCount(stateCount + 1);
        console.log("stateCount : ", stateCount);
      }
    
      function upRef() {
        ++refCount.current;
        console.log("refCount : ", refCount.current);
      }
    
      function upVar() {
        ++varCount;
        console.log("varCount : ", varCount);
      }
    
      return (
        <div>
          <div>stateCount : {stateCount} </div>
          <div>refCount : {refCount.current} </div>
          <div>varCount : {varCount} </div>
          <br />
          <button onClick={upState}>state up</button>
          <button onClick={upRef}>ref up</button>
          <button onClick={upVar}>var up</button>
        </div>
      );
    }
    
    export default App;
    ```
    
- **useMemo**
값을 기억해놓고 재사용하기 위해 사용, 
(불필요한 계산 감소, 성능 최적화)
    
    ```jsx
    const value = useMemo(() => fn, [])
    // 첫 번째 파라미터 : 기억한 함수를 '실행'하고 값을 반환
    // 두 번째 인자 : 배열
    // => 두 번째 인자에 있는 값이 업데이트 될 때만 콜백함수를 재호출하여 메모리에 저장된 값을 업데이트,
    // 두 번째 인자가 비어있다면 최초 마운트 될 때만 값을 계산함
    ```
    
    ```jsx
    // 예제
    import React, { useMemo, useState } from 'react';
    
    const HeavyCalculation = ({ value }) => {
    
      const calculateResult = useMemo(() => {
        console.log('Calculating result...');
        const result = value * 2;
        return result;
      }, [value]);
    
      return (
        <div>
          <p>Value: {value}</p>
          <p>Result: {calculateResult}</p>
        </div>
      );
    };
    
    const App = () => {
      const [count, setCount] = useState(0);
    
      const increment = () => {
        setCount(count + 1);
      };
    
      return (
        <div>
          <button onClick={increment}>Increment</button>
          <HeavyCalculation value={count} />
        </div>
      );
    };
    
    // HeavyCalculation 컴포넌트는 value라는 속성을 받는다 
    // calculateResult 함수를 useMemo를 통해 기억하는데
    // calculateResult는 value 값이 변경되지 않는 한 이전 결과 값을 재사용한다
    
    // App 컴포넌트에서는 버튼 클릭 시 count 값을 증가
    // HeavyCalculation 컴포넌트는 count 값에 따라 calculateResult를 계산하고, 
    // count 값이 변경될 때만 calculateResult 함수가 실행한다
    ```
    
- **useCallback** 
값을 기억해놓고 재사용하기 위해 사용, 
(불필요한 계산 감소, 성능 최적화)
    
    ```jsx
    useCallback(fn, [])
    // 첫번째 인자 : 기억한 함수를 '반환'
    // 두번째 인자 : 배열
    ```
    
    ```jsx
    // 예제
    import React, { useState, useCallback } from 'react';
    
    function App() {
      const [count, setCount] = useState(0);
    
      const handleAlert = useCallback(() => {
        alert(`Count: ${count}`);
      }, [count]);
    
      return (
        <div>
          <p>Count: {count}</p>
          <button onClick={() => setCount(count + 1)}>Increase Count</button>
          <button onClick={handleAlert}>Show Alert</button>
        </div>
      );
    }
    
    export default App;
    
    // count가 변경될 때만 handleAlert함수 실행
    ```
