# GraphQL

[이전 글 보러가기 -> Rest API](../RestAPI/RestAPI.md)

### GraphQL의 탄생 배경과 REST API의 차이점

REST API는 HTTP 요청을 보낼 때 예측 가능한 URI(자원)과 메서드(행위)를 통해 전송받을 정보를 요청하는 것으로 지난 글에서 정의하였다.<br>

그렇다면 GraphQL은 무엇이고 왜 나오게 된 것인지 이번 글에서 설명해 보도록 하겠다.<br>

예를 들어 REST API를 사용해 학생들의 국어 성적을 보고자 한다면 아래와 같을 것이다.<br>

`(도메인)//classes/(반idx)/students`

문제는 학생들의 국어성적만 받아와도 되는데 그 외의 다른 정보들도 같이 딸려올 것이다. 이것을 __Overfetching__ 이라고 한다.<br>

반대로 `(도메인)//classes/(반idx)`와 같이 요청을 하게 되면 반에 대한 정보만 알 수 있고 학생들에 대한 정보를 알 수 없기 때문에 한 번 더 요청을 해야하는 상황이 발생하는데 이것을 __Undterfetching__ 이라고 한다.<br>

때문에 REST API는 필요없는 요청을 하게되는 경우가 있으며 요청이 많아질 경우 그만큼 시간이 소요될 가능성이 있다.<br>

GraphQL은 REST API의 Overfetching과 Underfetching 문제를 단 하나의 Query 또는 Mutation을 요청함으로써 자신이 원하는 정보만을 가져오는 것이 가능하다.<br>

같은 API 서버를 사용하더라도 필요로 하는 정보가 사용자들마다 다르고 기기마다 다를 수 있다면 이러한 상황에서 GraphQL은 REST API보다 훨씬 더 효율적인 방식이 될 것이다.<br>

하지만 받아야 할 항목이 많아 GraphQL의 Query에 적어서 요청하는 것보다 REST API로 한번에 요청하는 것이 더 좋을 경우도 있다.<br>

즉, REST API와 GraphQL 어느쪽이 더 좋고 나쁘다기보단 요구하는 정보에 따라 요청이 복잡하더라도 원하는 정보만 받아올 GraphQL 또는 요청은 간단하지만 많은 데이터를 받을 REST API, 둘 중 하나를 잘 선택해서 사용하는 것이 중요하다.<br>
