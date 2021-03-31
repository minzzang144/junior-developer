# GraphQL Server Modularized

[이전 글 보러가기 -> GraphQL Mutation](./Mutation.md)

### Apollo Server Modularized

GraphQL Query, GraphQL Mutation 글에서 Apollo를 사용해 GraphQL Server를 만드는 법을 해봤다.<br>

문제는 모든 Query와 Mutation을 정의한 typeDefs와 resolver를 단 하나의 파일에서 공유하고 있다는 것이다.<br>

만약 이 정도의 typeDefs와 resolver를 가진다면 한 파일에 정의해두는 것이 낫지만 실제 프로그램을 만들게 된다면 이정도 type과 resolver로는 안될 것이다.<br>

따라서 한 파일 내에서 해결하지 않고 여러개의 모듈로 나눠서 import 해보는 방식으로 사용하는 것을 추천한다.<br>

typeDefs와 resolver는 하나의 Document로 이루어져도 되지만 여러 개의 배열을 가져도 문제가 없기 때문에 이것을 배열화 해보겠다.<br>

나눠야 할 목록들<br>

1. 루트 타입 Query

2. 루트 타입 Mutation

3. 단일 typeDefs와 resolver

4. resolver에 사용할 기능들

Equipment를 예를 들어보자.<br>

- 루트 타입 Query

  ```
  const typeDefs = gql`
      type Query {
          equipments: [Equipment]
      }
  `
  ```

- 루트 타입 Mutation

  ```
  // ...
  const typeDefs = gql`
      type Mutation {
        deleteEquipment(id: String): Equipment
      }
  `
  // ...
  ```

- Equipment 단일 타입

  ```
  // ...
  const typeDefs = gql`
      type Equipment {
          id: String
          used_by: String
          count: Int
          new_or_used: String
      }
  `
  const resolvers = {
      Query: {
          equipments: (parent, args) => dbWorks.getEquipments(args),
      },
      Mutation: {
          deleteEquipment: (parent, args) => dbWorks.deleteItem('equipments', args),
      }
  }
  // ...
  ```

- resolver에 사용할 기능들

  ```
  ...
  getEquipments: (args) => dataFiltered('equipments', args)
  ...
  ```

하나의 파일에서 정의했던 모든 타입과 행위를 4개의 모듈로 나누었다.<br>

이제 실제 apollo server 파일을 보자.<br>

```
// ...
const queries = require('루트 쿼리 타입 경로')
const mutations = require('투르 뮤테이션 타입 경로')
const equipments = require('Equipment 단일 타입과 리졸버 경로')
// ...
const typeDefs = [
    queries,
    mutations,
    equipments.typeDefs,
]
const resolvers = [
    equipments.resolvers
]
// ...
```

한 파일에 모든 정의와 기능을 집어넣는 것보다 훨씬 깔끔한 코드가 되었다.<br>

출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)

[다음 글 보러가기 -> GraphQL Type](./Type.md)
