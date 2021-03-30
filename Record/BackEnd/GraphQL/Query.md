# GraphQL Query

[이전 글 보러가기 -> GraphQL](./GraphQL.md)

### Apollo로 구현한 Server와 Client

```
const database = require('./database')
const { ApolloServer, gql } = require('apollo-server')
const typeDefs = gql`
  type Query {
    teams: [Team]
  }
  type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
`
const resolvers = {
  Query: {
    teams: () => database.teams
  }
}
const server = new ApolloServer({ typeDefs, resolvers })
server.listen().then(({ url }) => {
console.log(`🚀  Server ready at ${url}`)
})
```

ApolloServer는 2가지를 인자로 받는다. (typeDefs / resolvers)<br>

typeDefs는 GraphQL에서 사용될 데이터의 타입을 지정해 두는 것이다.(타입스크립트처럼 미리 타입을 지정해둔다고 보면 됨)<br>

resolver는 서비스의 액션들을 함수로 지정한다. 타입은 typeDefs에서 선언하고 실제 데이터는 resolver에서 요청하여 받아온다.<br>

GraphQL Playground는 작성한 Type과 resolver를 확인할 수 있으며 데이터를 요청하여 테스트를 해볼 수 있다.<br>

### Query 구현

```
const typeDefs = gql`
  type Query {
    teams: [Team]
  }
  type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
`
const resolvers = {
  Query: {
    teams: () => database.teams
  }
}
```

- typeDefs

  ```
  type Query {
      teams: [Team]
  }
  ```

  이 부분은 Query 루트 타입으로 자료요청에 사용될 쿼리들을 정의한다.

  현재 쿼리문은 teams라는 명령문을 가지고 있고 Team이라는 타입을 가진 형태를 배열로 반환하게 된다.

  ```
  type Team {
      id: Int
      manager: String
      office: String
      extension_number: String
      mascot: String
      cleaning_duty: String
      project: String
  }
  ```

  Team이라는 데이터 형태를 지정 / 자료형을 가진 필드들로 구성되어 있음.

- resolver

  ```
  const resolvers = {
    Query: {
      teams: () => database.teams
    }
  }
  ```

  실제 데이터를 요청하는 부분으로 typeDefs가 요청될 데이터의 타입을 지정했다면 resolver는 데이터 요청하는 부분을 구현한다.

  실제 프로젝트에서는 데이터베이스에서 요청하는 값을 반환한다.

- GraphQL Playground

  GrpahQL Playground에서 Query를 요청해 실제 받을 데이터를 테스트해 볼 수 있다.

  ```
  query {
    teams: {
      id
      manager
      office
    }
  }
  ```

  모든 팀의 id / manager / office 정보를 조회할 수 있다.

특정 팀만 받아오도록 쿼리를 추가해보자.<br>

- Query 루트 타입 추가

  ```
  type Query {
      ...
      team(id: Int): Team
  }
  ```

  하나의 팀만 받아올 경우 id를 인자로 받을 수 있게 하자

- reslover 추가

  ```
  Query: {
      //...
      team: (parent, args, context, info) => database.teams
          .filter((team) => {
              return team.id === args.id
          })[0],
  }
  ```

  resolver에서 사용된 (parent, args, context, info) 인자들은 Apollo에서 제공하는 인자들이다.

  args는 실제 Query에서 받게된 인자들을 가지게 되며 위의 Query 루트 타입에서 id를 인자로 받았음으로 args인자는 id 속성을 가지게 된다.

  요청한 id값과 team의 id값이 같다면 데이터를 반환하는 reslover이다.

- GraphQL Playground에서 쿼리 실행

  ```
  query {
    team(id: 1) {
        id
        manager
        office
        extension_number
        mascot
        cleaning_duty
        project
    }
  }
  ```

출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)
