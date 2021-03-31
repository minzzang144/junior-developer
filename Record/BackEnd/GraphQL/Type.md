# GraphQL Type

[이전 글 보러가기 -> GraphQL Server Modularized](./Modularize.md)

### Scalar Type

|  타입   |                                 설명                                 |
| :-----: | :------------------------------------------------------------------: |
|   ID    | 기본적으로 String역할을 함, 또한 고유 식별자임을 알리는 역할을 한다. |
| String  |                             UTF-8 문자열                             |
|   Int   |                       부호가 있는 32비트 정수                        |
|  Float  |                      부호가 있는 부동소수점 값                       |
| Boolean |                               참/거짓                                |

!가 타입뒤에 붙으면 Non Null을 의미하므로 null을 반환할 수 없고 무조건 값을 가져야 함<br>

### 열거 타입

미리 지정된 값 중에서만 반환<br>

대표적으로는 `enum`타입이 있다.<br>

- 커스텀 타입 생성

  ```
  const { gql } = require('apollo-server')
  const typeDefs = gql`
    enum Role {
        developer
        designer
        planner
    }
    enum NewOrUsed {
        new
        used
    }
  `
  module.exports = typeDefs
  ```

- 루트 타입에 커스텀 타입 추가

  ```
  // ...
  const enums = require('./typedefs-resolvers/_enums')
  // ...
  const typeDefs = [
    // ...
    enums,
    // ...
  ]
  ```

- 커스텀 타입 사용

  ```
  const typeDefs = gql`
    type Equipment {
        id: ID!
        used_by: Role!
        count: Int!
        new_or_used: NewOrUsed!
    }
    type EquipmentAdv {
        id: ID!
        used_by: Role!
        count: Int!
        use_rate: Float
        is_new: Boolean!
    }
  `
  ```

특정 타입의 배열을 반환한다면 -> `used_by: [Role!]` 처럼 배열화 할 수 있다.<br>

!(Non Null)타입은 배열의 안에서도 바깥에서도 사용할 수 있고 안쪽에 있는지 밖에 있는지에 따라 의미가 다르다.<br>

### Union

타입 여럿을 한 배열에 반환하고자 할 때 사용<br>

아래 코드를 보자<br><br>

- 루트 타입 Query

  ```
  const typeDefs = gql`
      type Query {
          // ...
          givens: [Given]
      }
  `
  ```

- Given 단일 typeDefs와 resolvers

  ```
  const { gql } = require('apollo-server')
  const dbWorks = require('../dbWorks.js')
  const typeDefs = gql`
      union Given = Equipment | Supply
  `
  const resolvers = {
      Query: {
          givens: (parent, args) => {
              return [
                  ...dbWorks.getEquipments(args),
                  ...dbWorks.getSupplies(args)
              ]
          }
      },
      Given: {
          __resolveType(given, context, info) {
              if (given.used_by) {
                  return 'Equipment'
              }
              if (given.team) {
                  return 'Supply'
              }
              return null
          }
      }
  }
  module.exports = {
      typeDefs: typeDefs,
      resolvers: resolvers
  }
  ```

Given이라는 유니온타입은 Equipment 또는 Supply 타입 둘 중 어떤 것도 될 수 있다.<br>

Given 타입을 반환해야할 때 만약 Given타입이 used_by를 필드로 가지면 Equipment를 typename으로 반환하고 team을 필드로 가지면 Supply를 typename으로 반환한다.<br>

- GraphQL Playground

  ```
  query {
    givens {
      __typename
      ... on Equipment {
        id
        used_by
        count
        new_or_used
      }
      ... on Supply {
        id
        team
      }
    }
  }
  ```

### Interface

다수의 타입이 공통적인 필드를 가지고 있다면 **Interface** 를 규격해 공통 필드를 가지도록 한다.<br>

- 공통 타입 필드

  ```
  type Equipment {
    id: ID!
    used_by: Role!
    count: Int
    new_or_used: NewOrUsed!
  }
  ...
  type Software {
    id: ID!
    used_by: Role!
    developed_by: String!
    description: String
  }
  ```

  Equipment와 Software는 id와 used_by필드를 공통적으로 가지고 있다. 이 2개의 필드를 interface에 규격하고 상속하도록 만들어보자.

- Interface 규격

  ```
  const { gql } = require('apollo-server')
  const typeDefs = gql`
    interface Tool {
        id: ID!
        used_by: Role!
    }
  `
  const resolvers = {
    Tool: {
    __resolveType(tool, context, info) {
        if (tool.developed_by) {
            return 'Software'
        }
        if (tool.new_or_used) {
            return 'Equipment'
        }
        return null
      }
    }
  }
  module.exports = {
    typeDefs: typeDefs,
    resolvers: resolvers
  }
  ```

- Interface 상속

```
type Equipment implements Tool {
    id: ID!
    used_by: Role!
    count: Int
    new_or_used: NewOrUsed!
}

and

type Software implements Tool {
    id: ID!
    used_by: Role!
    developed_by: String!
    description: String
}
```

요청해야할 필드들이 많아지고 복잡해질 경우 Union과 Interface를 사용해 다양하게 활용할 수 있다.<br>

출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)
