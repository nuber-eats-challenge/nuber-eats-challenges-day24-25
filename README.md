# nuber-eats-challenges-day24-25

<details>
  <summary>
  Day 22-23 정답해설
  </summary>

1. apollo client 설정
- ![](https://i.ibb.co/Mp8smVV/apollo-client-1.png)

- 솔루션에는 위 코드와 같이 typePolicies, Query field가 customizing 설정이 되어 있지만, 필수는 아니기 때문에 uri만 제공해줘도 무방합니다.
- export const client = new ApolloClient({uri, cache: new InMemoryCache()}) 이런식으로만 하셔도 괜찮습니다.
- 위와 같이 설정한 client는 앱 최상단에 <ApollloProvider> 컴포넌트로 감싸서 사용하면 됩니다.
2. react router dom 설정
- react router dom을 사용하시려면 사용하시려는 Route를 Router(BrowserRouter, 또는 HashRouter)로 감싸서 사용하시면 됩니다.
- 솔루션의 /src/routers/logged-out-router.tsx파일처럼 Router > Switch > Route 순으로 사용해주시면 됩니다.
- 사용방법은 워낙에 단순하기 때문에, react router dom을 사용한 구조만 집중해주시면 쉽게 구현할 수 있는 부분입니다.
3. reactive variables(로그인 토큰 및 로그인 상태)
- src/App.tsx파일을 보시면 useReactiveVar가 사용된 것을 볼 수 있는데요, reactive variables라는 것입니다. apollo client의 cache 밖에서의 local state를 다루기 위해 사용하는 것입니다. 링크해드린 문서를 참고하시어, 생성(makeVar), 읽기, 값쓰기 방법을 참고하시어 useReactiveVar 사용법을 익히시면 되겠습니다.
- 솔루션에서는 useReactiveVar를 이용하여 로그인 여부를 저장하고, 로그인 하였을 때에는 makeVar로 생성한 변수(isLoggedInVar)에 값을 변형하여 리액트에서 바로바로 반응할 수 있게끔 설정을 해주어, <App> 컴포넌트에서<LoggedInRouter> 컴포넌트를 사용할지 <LoggedOutRouter>컴포넌트를 사용할 것인지를 결정되게 됩니다. 또한, 로그인 토큰과 관련하여서도 react variables를 사용할 수 있습니다.
- 솔루션의 src/apollo.ts 파일에 보시면 makeVar를 이용하여 로그인 토큰도 localStorage에 연결된 token 값에 반응하도록 설정되어 있는 것을 보실 수 있습니다. 이런 방법으로 로그인 토큰이 저장된 로컬 스토리지를 react로 쉽게 연결하여 사용할 수 있습니다.
4. 로그인 Mutation
- graphql의 mutation을 클라이언트에서 이용하려면 먼저 gql을 사용하여, 쿼리문을 먼저 작성하도록 합니다.
- ![](https://i.ibb.co/SwSfdww/login-mutation.png)
- 쿼리어가 익숙하지 않으시다면 강의 및 graphql의 query & mutation을 참고하도록 합니다. query, mutation, fragment 작성은 graphql api를 사용하는 한 밥먹듯 나오니 꼭 사용법을 알고 넘어가도록 합니다. 위와같이 만든 쿼리어는 종류에 따라 사용해주시면 됩니다. login은 mutation이므로 useMutation을 사용하시면 됩니다.
- ![](https://i.ibb.co/vZNxr2X/use-login.png)
- typescript를 사용하고 있기 때문에 위와 같이 useMutation에 login, loginVariables 타입을 넘겨주었지만, 꼭 필수는 아닙니다. 없으면 없는대로 작업하셔도 됩니다만 타입스크립트의 도움을 받으려면 지정을 해주면 좋습니다. (코드샌드박스에서 작업하셨다면 위 apollo codegen을 사용할 수 없기 때문에 사용 안하셔도 됩니다). 인자로 넘겨준 것은 위의 쿼리어로 만든 LOGIN_MUTATION과 옵션값입니다.
- 위의 솔루션 코드에서는 mutation 수행이 완료되면 onCompleted 콜백함수를 실행하라는 옵션값을 준 것입니다. 자세한 내용은 위 링크를 참고하시면 되겠습니다.
5. useForm(react hook form)
- 최근에 버전7로 api가 업그레이드 되었습니다. 버전7은 강의와는 사용방법이 조금 다르니 참고하시길 바랍니다. 강의처럼 버전6 기반으로 설명드리겠습니다. 버전7 사용하시는 분들은 반드시 react-hook-form의 문서를 참고하셔야 합니다.
- useForm함수는 위와 같이 사용하며 옵션값은 문서를 참고하도록 합니다. 타입스크립트를 지원하기 때문에 위와 같이 타입을 제공해주면 자동완성이 쉽게 되기 때문에 편하고 실수할 일이 적어집니다. handleSubmit은 아래 같은 방법으로 사용할 수 있습니다.
- ![](https://i.ibb.co/YWTJjkz/handle-Submit.png)
- handleSubmit은 onValid에 form 값이 valid했을 때 data를 같이 넘겨줍니다. 위의 로그인 폼 같은 경우에는 onValid에 이메일과 패스워드를 같이 넘겨주기 때문에 솔루션과 같이 꼭 getValues를 사용 안하셔도 됩니다.
- const onValid = (data) => { ... } 형식으로 사용하셔도 무방합니다. register 사용은 솔루션 코드를 참고하시길 바랍니다. react hook form의 핵심 내용이기도 하지만, 여기서 설명하기에는 부족합니다. 꼭 코드와 강의, register 문서를 참고하셔서 사용방법을 익히도록 합니다.

###결론
문서와 강의를 참고하여서 apollo client, gql, react-hook-form, react router dom 등의 사용방법을 익히도록 합니다. 계속 나옵니다. 이번 과제는 난이도가 높지 않아 react 관련 패키지들이 익숙치 않으신 분들이 충분히 연습하시면서 내용들을 익히실 수 있는 부분입니다.
</details>

### Create Account & UI
- 오늘의 강의: 우버 이츠 클론코딩 강의 #15.12 - #15.18 (21.09.15 ~ 16)
- 오늘의 과제: 위의 강의를 시청하신 후, 아래 코드 챌린지를 제출하세요.

### Code Challenge
- On today's challenge make the 'Create Account' page, this includes the route, the mutations, the form, everything! And when you are done, make 'Login' and 'Create Account' page as beautiful as possible.
- [ ] Create Account 페이지를 만듭니다.
- [ ] Create Account 페이지와 Login 페이지를 최대한 꾸며 봅니다.
- [여기 웹페이지](https://mobbin.design/browse/ios/apps) 참고하시면 몇몇 유명한 앱들의 로그인 페이지를 볼 수 있습니다. 여기서 디자인 아이디어를 얻어서 mobile-only UI 만드는데 참고하세요.




<details>
  <summary>
  Hint
  </summary>

- 이전의 챌린지 과제였던 로그인 파트와 크게 다르지 않아서 무난하게 하실 수 있는 챌린지 과제입니다. tailwind와 react hook form, 쿼리어, apollo client가 아직 익숙치 않으신 분들은 많은 연습을 하실 수 있는 과제입니다.
- create account 페이지는 이전에 login 페이지에서 사용한 코드들을 사용하시면 편리합니다.
- react hook form을 이용하여 input을 작성하실 때에 여러가지 rules를 이용하여 다양하게 form의 validation과 message handling을 테스트 해보시길 추천드립니다.
- ex. pattern, valid, maxLength, minLength 등등, react hook form의 [register](https://react-hook-form.com/v6/api/#register) 페이지를 참고바랍니다.
- useForm의 formState를 이용하여 form의 입력이 valid한지 테스트 해보세요.
- nuber-eats 클론 강의처럼 버튼에 loading state를 관리해보시길 추천드립니다~! 강의에서는 useState를 이용하였습니다.

</details>