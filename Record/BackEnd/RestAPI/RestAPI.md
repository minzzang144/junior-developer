# REST API

### API

REST API를 알기 위해서는 먼저 **API** 의 개념을 집고 넘어가야 한다.<br>

누군가가 기계를 만들면 사용자가 그 기계를 전부 활용할 수 있도록 제어장치를 만들어야 한다.<br>

예를 들어 컴퓨터는 사용자가 사용할 수 있도록 모니터, 마우스, 키보드 등을 제공하며 사용자가 컴퓨터와 소통을 할 수 있는 창구는 오로지 모니터, 마우스, 키보드와 같은 장치를 통해서만 할 수 있다.<br>

이처럼 사용자와 소프트웨어 사이에서 소통을 할 수 있도록 마련한 것을 **User Interface(UI)** 라고 부른다.<br>

또한 컴퓨터나 스마트폰에서 원하는 대로 제어하고 정보를 보기 위해서 스크롤바, 버튼, 브라우저 창 등 소프트웨어적인 장치가 마련되어 있다. 이 또한 UI라고 불리운다.<br>

마찬가지로 소프트웨어와 소프트웨어 간에 수많은 요청과 정보 교환이 필요한데 이들 사이에서도 소통할 수 있는 창구가 필요하다.<br>

이처럼 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단을 **Application Programming Interface(API)** 라고 한다.<br>

### REST API

그렇다면 REST API는 무엇인가?<br>

**REST API** 는 우리가 서버에 요청할 때 HTTP 프로토콜을 따르는 방식으로 API를 설계(디자인)하는 것을 말한다.<br>

REST API는 자원(Resource) / 행위(Verb) / 표현(Representations)의 3가지 요소로 구성된다.<br>

중요한 것은 누군가 REST API를 보았을 때 자체 표현 구조(Self-descriptiveness)로 구성되어 있어 REST API만으로 요청을 이해할 수 있어야 한다는 것이다. 모두가 이해할 수 있어야 좋은 API를 설계한 것이며 이것을 좀 더 `Restful`한 API라고 할 수 있는 것이다.<br>

REST API는 다음과 같은 규칙을 지킨다.<br>

- URI는 자원을 표현하는 데에 집중한다.(URI는 자원을 구조와 함께 나타내는 것이다.)

  `https://(도메인)/classes/2/students`

  위의 URI에서 리소스명은 동사보다는 명사를 사용하며 URI의 자원을 표현하는데 중점을 두어야 한다. GET과 같은 행위에 대한 표현이 들어가서는 안된다. EX)//getTodos/1

- 행위에 대한 정의는 HTTP Method(GET, POST, PUT, DELETE 등)를 통해 해야 한다.

  | Method |  Action  |          Role           | Payload |
  | :----: | :------: | :---------------------: | :-----: |
  |  GET   | Retrieve | 모든/특정 리소스를 조회 |    X    |
  |  POST  |  Create  |      리소스를 생성      |    O    |
  |  PUT   | Replace  |  리소스의 전체를 교체   |    O    |
  | PATCH  |  Modify  |  리소스의 일부를 수정   |    O    |
  | DELETE |  Delete  | 모든/특정 리소스를 삭제 |    X    |

  POST & PUT & PATCH는 Payload를 가지고 있어 좀 더 안전하고 많은 정보를 실어보낼 수 있다.

결국 REST API란 HTTP 요청을 보낼 때 어떤 URI에 어떤 메서드를 사용할지 개발자들 사이에 널리 지켜지는 약속이다.<br>

내가 설계한 REST API가 다른 개발자들이 봤을 때 보는 것만으로도 명확히 무슨 일을 하는지 알 수 있도록 하자.<br>
