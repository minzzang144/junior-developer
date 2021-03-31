# GraphQL Input

### Argument

조건을 입력 받아 원하는 정보들을 더 명확히 받아올 수 있다.<br>

People 타입의 정보들을 조건을 받아 조건을 필터링한 People만 받아와 보자.<br>

- 루트 타입 Query

  ```
  출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)

    ...
    peopleFiltered(
        team: Int,
        sex: Sex,
        blood_type: BloodType,
        from: String
    ): [People]
    ...
  }
  ```

  team, sex, blood_type, from을 인자로 받아 People 타입의 배열을 반환한다.

- People resolver

  ```
  Query: {
    // ...
    peopleFiltered: (parent, args) => dbWorks.getPeople(args),
  }
  ```

  받아온 인자들을 통해 resolver를 수행

- GraphQL Playground

  ```
  query {
    peopleFiltered (
      team: 1
      blood_type: B
      from: "Texas"
    ) {
      id
      first_name
      last_name
      sex
      blood_type
      serve_years
      role
      team
      from
    }
  }
  ```

  Texas에 있는 1반의 B형인 사람들만을 가져와서 id, first_name ... 등의 필드를 보여준다.

  참고로 peopleFitered된 사람들의 명칭을 바꾸고 싶다면 아래와 같이 명칭을 바꾸는 것도 가능하다.

  ```
  query {
    GoodPeople:peopleFiltered(...) {...}
  }
  ```

### Input

받아와야 할 인자가 너무 많다면 **Input** 을 지정해주는 것이 좋다.<br>

- People typeDefs와 resolver

  ```
  const typeDefs = gql`
      ....
      input PostPersonInput {
          first_name: String!
          last_name: String!
          sex: Sex!
          blood_type: BloodType!
          serve_years: Int!
          role: Role!
          team: ID!
          from: String!
      }
  `
  const resolvers = {
      // ...
      Mutation: {
          postPerson: (parent, args) => dbWorks.postPerson(args),
      }
  }
  ```

  input이라는 키워드를 사용해 PostPersonInput 식별자로 받아올 인자들을 담아두었다

- 루트 타입 Mutation

  ```
  type Mutation {
      postPerson(input: PostPersonInput): People!
      ...
  }
  ```

  받아올 인자를 input(PostPersonInput)으로 받아와 People을 반환한다.

- GraphQL Playground

  ```
  mutation {
    postPerson(input: {
      first_name: "Hanna"
      last_name: "Kim"
      sex: female
      blood_type: O
      serve_years: 3
      role: developer
      team: 1
      from: "Pusan"
    }) {
      id
      first_name
      last_name
      sex
      blood_type
      role
      team
      from
    }
  }
  ```

  input 키워드를 사용해 조건을 추가해준다.

출처: 얄팍한 코딩사전 [GraphQL & Apollo 강좌](https://www.youtube.com/watch?v=9BIXcXHsj0A&t=221s)<br>
