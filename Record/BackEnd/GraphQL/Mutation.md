# GraphQL Mutation

[이전 글 보러가기 -> GraphQL Query](./Query.md)

### Mutation

지난 글에서 GraphQL Query를 다뤄봤다. Mutaion도 Query와 비슷하게 구성되며 typeDefs와 resolver를 구현해야 한다.<br>

데이터를 조회하는 부분은 Query명령에서 이루어지지만 데이터를 생성, 수정, 삭제 하는 부분은 Mutation명령에서 이루어진다.<br>

- 데이터 삭제하기

  `삭제 Mutation`

  ```
  type Mutation {
    deleteEquipment(id: String): Equipment
  }
  ```

  삭제할 Equipment id를 인자로 받는다.

  GraphQL에서 행위에 대한 표현은 명령어에서 이루어지므로 `delete...`으로 시작하는 명령어를 가진다.

  `삭제 resolver`

  ```
  Mutation: {
    deleteEquipment: (parent, args, context, info) => {
        const deleted = database.equipments
            .filter((equipment) => {
                return equipment.id === args.id
            })[0]
        database.equipments = database.equipments
            .filter((equipment) => {
                return equipment.id !== args.id
            })
        return deleted
    }
  }
  ```

  `Playground 명령어`

  ```
  mutation {
    deleteEquipment(id: "notebook") {
      id
      used_by
      count
      new_or_used
    }
  }
  ```

- 데이터 생성하기

  `생성 Mutation`

  ```
  type Mutation {
      insertEquipment(
          id: String,
          used_by: String,
          count: Int,
          new_or_used: String
      ): Equipment
      ...
  }
  ```

  GraphQL에서 행위에 대한 표현은 명령어에서 이루어지므로 `insert...`으로 시작하는 명령어를 가진다.

  `생성 resolver`

  ```
  Mutation: {
      insertEquipment: (parent, args, context, info) => {
          database.equipments.push(args)
          return args
      },
      //...
  }
  ```

  `Playground 명령어`

  ```
  mutation {
    insertEquipment (
      id: "laptop",
      used_by: "developer",
      count: 17,
      new_or_used: "new"
    ) {
      id
      used_by
      count
      new_or_used
    }
  }
  ```

- 데이터 수정하기

  `수정 Mutation`

  ```
  type Mutation {
    editEquipment(
        id: String,
        used_by: String,
        count: Int,
        new_or_used: String
    ): Equipment
    ...
  }
  ```

  GraphQL에서 행위에 대한 표현은 명령어에서 이루어지므로 `edit...`으로 시작하는 명령어를 가진다.

  `수정 resolver`

  ```
  Mutation: {
    // ...
    editEquipment: (parent, args, context, info) => {
        return database.equipments.filter((equipment) => {
            return equipment.id === args.id
        }).map((equipment) => {
            Object.assign(equipment, args)
            return equipment
        })[0]
    },
    // ...
  }
  ```

  `Playground 명령어`

  ```
  mutation {
    editEquipment (
      id: "pen tablet",
      new_or_used: "new",
      count: 30,
      used_by: "designer"
    ) {
      id
      new_or_used
      count
      used_by
    }
  }
  ```

출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)
