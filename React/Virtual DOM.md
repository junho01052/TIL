# Virtual DOM
React에는 virtual DOM이라는 가상의 DOM 객체가 존재합니다.  
React는 실제 DOM 객체에 접근하여 조작하는 대신 Virtual DOM 객체에 접근하여 전후를 비교하고 바뀐 부분을 적용합니다.

## Virtual DOM 활용 이유
DOM은 브라우저가 트리구조로 만들어진 객체 모델이기 때문에  
JavaScript는 쉽게 DOM 객체에 접근할 수 있고, 조작할 수 있습니다.  
프로그래밍 언어로 조작하는 DOM은 애플리케이션의 UI 상태가 변경될 때마다 전체를 업데이트합니다.  
이런 렌더링은 브라우저의 구동 능력에 의존하기 때문에 DOM의 조작 속도는 느려지게 됩니다.  
  

일정 부분이 바뀐다면 전체가 아닌 바뀐 부분만 렌더링 하기 위해 Virtual DOM이 등장했습니다.  
Virtual DOM 객체는 화면에 표시되는 내용을 직접 변경하는 것이 아니라 실제 DOM을 조작하는 것보다 훨씬 속도가 빠릅니다.   
어떻게 더 빠른지 아래 그림을 보겠습니다.  
![Virtual Dom](https://user-images.githubusercontent.com/58800295/180723292-06084835-9194-46c5-a712-b2fa8ed94c86.png)  
노란색 원들은 엡데이트된 노드입니다. React에선 상태가 변경된 UI 요소죠.  
상태가 변경되면 먼저 Virtual DOM에 변화를 적용합니다.  
그후 실제 DOM과 비교하고, 바뀐 부분만 브라우저에 적용합니다.