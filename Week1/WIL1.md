# MVC 패턴이란?

![image-20230320220840793](/Users/jeonmin/Library/Application Support/typora-user-images/image-20230320220840793.png)

MVC는 모델, 뷰, 컨트롤러를 의미한다. 

그리고 MVC 패턴은 새로운 어플리케이션을 개발하는데 있어 세가지 요소를 구분 짓는 것을 의미한다. 세 요소를 각각 구분함으로써 효율적인 유지보수성을 취한다.

## Model

모델은 application의 정보, 데이터를 나타낸다. 모델은 다음과 같은 규칙을 가지고 있다.

### 1. 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야한다.
우리가 어떠한 웹페이지를 만든다고 할 때 웹의 각 요소들에 대한 정보들을 가지고 있어야 한다는 의미이다.

### 2.View나 Controller에 대한 어떤 정보도 알지 말아야한다.
이는 데이터 변경이 일어난 경우 모델에서는 화면 정보를 수정할 수 있도록 View를 참조하는 내부 변수를 가지고 있어서는 안된다는 의미이다. 즉, 모델은 뷰나 컨트롤러에 영향을 주어서는 안된다는 의미이다.

### 3.변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야만 한다.
모델의 데이터가 변경된다면 이를 누군가에게 전달할 수 있어야하고, 또한 누군가가 모델을 변경하도록 하였을 때 이를 수신받을 수 도 있어야 한다는 것이다.

## View
View는 사용자 인터페이스를 나타낸다. 보이는 화면을 담당한다.

### 1.모델이 가지고 있는 정보를 따로 저장해서는 안된다.
만일 모델의 데이터로 부터 어떠한 화면을 그릴 때 뷰는 화면을 그리기만 할 뿐, 모델의 데이터를 따로 저장해서는 안된다는 의미이다.

### 2.모델이나 컨트롤러와 같이 다른 구성요소들을 몰라야 된다.
마치 추상화와 같이 뷰는 뷰의 역할만 하면된다. 화면을 그리기만 할 뿐 모델이나 컨트롤러가 무엇을 하는지는 알 필요 없다.

### 3.변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야만 한다.
화면에서 사용자가 화면의 내용을 변경 하였을 때 이를 모델에게 전달해서 모델의 데이터를 변경하여야 한다.

## Controller
데이터와 사용자 인터페이스 요소들을 잇는 다리 역할

### 1.모델이나 뷰에 대해서 알고 있어야 한다.
모델과 뷰는 서로 모르기 때문에 그 중간을 컨트롤러가 연결해주는 가교 역할을 한다. 때문에 컨트롤러는 모델과 뷰에 대해서 알고 있어야 한다.

### 2.모델이나 뷰의 변경을 모니터링 해야 한다.
모델이나 뷰의 변경 통지를 받은 경우 컨트롤러는 이를 각 구성 요소에게 통지해야 한다.

# API
api는 하나의 프로그램에서 다른 프로그램으로 데이터를 주고 받기 위한 방식.
Ex) 식당의 메뉴판(주인장과 나와 뭐 먹을지 정보 교환 방법)
![image-20230320220912729](/Users/jeonmin/Library/Application Support/typora-user-images/image-20230320220912729.png)

우리가 사용하는 브라우저를 통해서 우리는 평소에도 api를 사용한다. 예를들어 내가 웹툰을 보고 싶다고 하면 API를 통해서 보고자 하는 웹툰에 대해서 GET 요청을 해야한다. 그리고 평소에는 그냥 마우스로 버튼을 클릭하면 이미 html상에 이미 구현되어 있는 기능을 통해서 편하게 API를 요청하고 웹툰을 볼 수 있다.
![image-20230320220941313](/Users/jeonmin/Library/Application Support/typora-user-images/image-20230320220941313.png)

데이터 베이스 api를 통해서 우리는 서버와 통신도 가능하다.

# RESTful
## Rest란?
Rest는 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 것을 의미.
쉽게 말해서 HTTP를 통해서 자원을 명시하고 POST, GET, PUT, DELETE를 통해서 CRUD operation을 적용하는 것을 의미한다.

## Rest API란?
Rest를 기반으로 구현한 API를 REST API라고 한다.
![image-20230320220956007](/Users/jeonmin/Library/Application Support/typora-user-images/image-20230320220956007.png)

## RESTful이란?
REST API를 제공하는 웹 서비스를 RESTful 하다고 한다. 즉, REST 원리를 잘 따르는 방법론을 의미하며 공식적인 것은 아니다.
