# Apollo Movie App

Apollo Movie App with React.js, apollo and GraphQL

### Set up

1. react project 생성

```bash
$npx create-react-app .
```

2. 라이브러리 설치(router, apollo, graphql)

```bash
$npm i react-router-dom @apollo/client graphql
```

### apollo client 생성 - 호출할 graph api 정보 설정

apollo.js

```javascript
import { ApolloClient, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  uri: "https://graphql-movie-api-2021.herokuapp.com/",
  cache: new InMemoryCache(),
});

export default client;
```

- apollo client 는 GraphQL의 결과를 InMemoryCache 에 저장하며, 이를 통해 클라이언트는 불필요한 네트워크 요청 없이 동일한 데이터에 대해 요청할 수 있음

### React에 apollo client연결

index.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";
import { ApolloProvider } from "@apollo/client";
import client from "./apollo";

ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById("root")
);
```

- 앱 내의 모든 컴포넌트에서 GraphQL API연동이 가능하기 위해 React application을 apollo client로 감싼다.

### Graph API 호출

1. Query 작성 (or Mutation)

Home.js

```javascript
import React from "react";
import { gql, useQuery } from "@apollo/client";

const GET_MOVIES = gql`
  {
    movies {
      id
      medium_cover_image
    }
  }
`;

export default () => {
  const { loading, error, data } = useQuery(GET_MOVIES);
  if (loading) return <p>Loading..</p>;
  return <div>{data.movies.map((m) => m.id)}</div>;
};
```

- gql을 통해 쿼리문을 작성
- useQuery는 쿼리를 인자로 전달받아 실행시켜줌
- GraphQL 서버로부터 데이터를 응답받았으면 데이터를 화면단에 보여줌
