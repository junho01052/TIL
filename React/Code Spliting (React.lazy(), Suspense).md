# Code Spliting (React.lazy(), Suspense)
모던 웹으로 발전하면서 JavaScript 코드가 무거워졌습니다.  
코드 분할을 하게되면 당장 필요한 코드가 아니면 분리시킨 후 필요할 때 불러와서 사용할 수 있습니다.  
이를 통해 대규모 프로젝트의 경우에도 로딩 속도를 개선할 수 있습니다.

### 번들을 분할하거나 줄이는 법
번들링 되는 파일에는 npm을 통해 다운받는 서드파티 라이브러리도 포함됩니다.  
서드파티 라이브러리는 유용하고 효율적이지만 다양한 메소드를 포함하기에 번들링 시 많은 공간을 차지합니다.  
사용 중인 라이브러리를 전부 import 하는 것보다 사용할 메소드만 불러와 쓰는 것이 좋습니다.  
```javascript
/* 이렇게 lodash 라이브러리를 전체를 불러와서 그 안에 들은 메소드를 꺼내 쓰는 것은 비효율적입니다.*/
import _ from 'lodash';

...

_.find([]);

/* 이렇게 lodash의 메소드 중 하나를 불러와 쓰는 것이 앱의 성능에 더 좋습니다.*/
import find from 'lodash/find';

find([]);
```  

## React에서의 코드 분할
SPA(Single Page Application)이기에, 사용하지 않는 컴포넌트까지 한번에 불러와 첫 화면이 렌더링 될때까지 시간이 오래걸립니다.  
그래서 코드분할이 필요합니다.  
React에서는 dynamic import를 사용하여 코드를 분할합니다.  
이전까지는 파일의 가장 최상위에서 import 하여 라이브러리 및 파일을 불러왔는데, 이를 static import라고 합니다.  

### Dynamic Import
```javascript
form.addEventListener("submit", e => {
  e.preventDefault();
	/* 동적 불러오기는 이런 식으로 코드의 중간에 불러올 수 있게 됩니다. */
  import('library.moduleA')
    .then(module => module.default)
    .then(someFunction())
    .catch(handleError());
});

const someFunction = () => {
    /* moduleA를 여기서 사용합니다. */
}
```  

이 dynamic import는 `React.lazy()`와 함께 사용할 수 있습니다.

## React.lazy()
React는 `React.lazy`를 통해 컴포넌트를 동적으로 import할 수 있기 때문에 초기 렌더링 지연시간을 줄일 수 있습니다.

## React.Suspense
`React.lazy`로 감싼 컴포넌트는 단독으로 쓰일 수 없고, `React.Suspense` 컴포넌트 하위에서 렌더링 해야합니다.  

```javascript
/* suspense 기능을 사용하기 위해서는 import 해와야 합니다. */
import { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
			{/* 이런 식으로 React.lazy로 감싼 컴포넌트를 Suspense 컴포넌트의 하위에 렌더링합니다. */}
      <Suspense fallback={<div>Loading...</div>}>
				{/* Suspense 컴포넌트 하위에 여러 개의 lazy 컴포넌트를 렌더링시킬 수 있습니다. */}
        <OtherComponent />
				<AnotherComponent />
      </Suspense>
    </div>
  );
}
```

## React.lazy와 Suspense 적용  
```javascript
import { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```
라우터가 분기되는 컴포넌트에서 각 컴포넌트에 `React.lazy`를 사용하여 import합니다.  
그리고 Route 컴포넌트들을 `Suspense`로 감싼 후 로딩 화면으로 사용할 컴포넌트를 `fallback` 속성으로 설정해주면 됩니다.  
초기 렌더링 시간이 줄어드는 분명한 장점이 있으나 페이지를 이동하는 과정마다 로딩 화면이 보여지기 때문에 서비스에 따라서 적용 여부를 결정해야 합니다.