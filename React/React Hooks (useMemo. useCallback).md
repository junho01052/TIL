# React Hooks(useMemo, useCallback)
컴포넌트는 기본적으로 상태가 변경되거나 부모 컴포넌트가 리랜더링 될 때마다 리렌더링 하는 구조로 이루어져 있습니다.  
그러나 너무 잦은 렌더링은 좋지 않습니다.  
React Hook은 함수 컴포넌트가 상태를 조작하고 최적화 기능을 사용할 수 있게끔 하는 메소드입니다.  
그 중 렌더링 최적화를 위한 Hook이 useMemo와 useCallback입니다.

## useMemo
특정 값(value)를 재사용하고자 할 때 사용하는 리액트 Hook

```javascript
/* useMemo를 사용하기 전에는 꼭 import해서 불러와야 합니다. */
import { useMemo } from "react";

function Calculator({value}){

	const result = useMemo(() => calculate(value), [value]);

	return <>
      <div>
					{result}
      </div>
  </>;
}
```

```javascript
import React, { useState, useMemo } from "react";
import "./styles.css";
import { add } from "./add";

export default function App() {
  const [name, setName] = useState("");
  const [val1, setVal1] = useState(0);
  const [val2, setVal2] = useState(0);
  const answer = useMemo(() => add(val1, val2), [val1, val2]);

  return (
    <div>
      <input
        className="name-input"
        placeholder="이름을 입력해주세요"
        value={name}
        type="text"
        onChange={(e) => setName(e.target.value)}
      />
      <input
        className="value-input"
        placeholder="숫자를 입력해주세요"
        value={val1}
        type="number"
        onChange={(e) => setVal1(Number(e.target.value))}
      />
      <input
        className="value-input"
        placeholder="숫자를 입력해주세요"
        value={val2}
        type="number"
        onChange={(e) => setVal2(Number(e.target.value))}
      />
      <div>{answer}</div>
    </div>
  );
}

```



useMemo를 활용하면 새로운 렌더링과 비교해 value값이 동일한 경우에는 이전 렌더링의 value값을 그대로 재활용합니다.  
이는 Memoization 개념과 연관이 깊습니다.

### Memoization
알고리즘에서 자주 나오는 개념으로,  
기존의 연산 결과값을 메모리에 저장해두고, 동일한 입력이 들어온다면 재활용하는 프로그램 기법입니다.  
메모이제이션을 적절히 활용하면 중복 연산을 할 필요가 없기 때문에 앱 성능을 최적화 시킬 수 있습니다.

## useCallback
마찬가지로 Memoization을 활용한 Hook입니다.  
useMemo가 값의 재사용을 위한 Hook이라면, useCallback은 함수의 재사용을 위해 사용합니다.

```javascript
/* useCallback를 사용하기 전에는 꼭 import해서 불러와야 합니다. */
import React, { useCallback } from "react";

function Calculator({x, y}){

	const add = useCallback(() => x + y, [x, y]);

	return <>
      <div>
					{add()}
      </div>
  </>;
}
```

```javascript
import { useState, useCallback } from "react";
import "./styles.css";
import List from "./List";

export default function App() {
  const [input, setInput] = useState(1);
  const [light, setLight] = useState(true);

  const theme = {
    backgroundColor: light ? "White" : "grey",
    color: light ? "grey" : "white"
  };

  const getItems = useCallback(() => {
    return [input + 10, input + 100];
  }, [input]);

  const handleChange = (event) => {
    if (Number(event.target.value)) {
      setInput(Number(event.target.value));
    }
  };

  return (
    <>
      <div style={theme} className="wall-paper">
        <input
          type="number"
          className="input"
          value={input}
          onChange={handleChange}
        />
        <button
          className={(light ? "light" : "dark") + " button"}
          onClick={() => setLight((prevLight) => !prevLight)}
        >
          {light ? "dark mode" : "light mode"}
        </button>
        <List getItems={getItems} />
      </div>
    </>
  );
}
```
`button`을 눌러도 “아이템을 가져옵니다.”가 콘솔에 출력되었던 코드가  
useCallback을 활용해 최적화되었습니다.

