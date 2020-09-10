## 공부하다 알게된 기타 지식들





#### 이스케이프의 자세한 개념

##### 의미 문자 = 메타 문자

- **어떤 기능에서 특별한 의미를 가지는 문자 (특수 기호)**

- Example

  - SQL에서 홑따옴표(')는 문자열의 시작과 끝을 의미
  - URL에서 &는 요청 파라미터롸 파라미터를 구분하는 의미

- 의미 문자를 문자 그 자체로 사용하는 경우

- Example

  - John's Name 이라는 글자가 들어간 것을 검색하는 쿼리를 만들 때 ⇒ content like '%John's Name%'
  - 요청 파라미터 중 파라미터 이름이 CompanyName이고, 파라미터 값이 Bandi&Lunis인 경우 ⇒ …?CompanyName=Bandi&Lunis

- **위와 같이 의미 문자를 그대로 사용히면 잘못 해석 하거나 해석을 하지 못해 오류 발생**

- 이런 문제점을 해결하기 위해서는 의미 문자를 문자 그 자체로 해석 될 수 있도록 변형

  => **이스케이프: 의미 문자에서 의미를 제거하고 문자만 남기는 것**

- 이스케이프를 구현하는 방법

  - 이스케이프를 나타내는 의미 문자를 사용 **( \ )**
    -  '%John's Name%' => '%John\\'s Name%'
  - 해당 기능에서 제공해 주는 규칙을 이용
    - MySQL인 경우 홑따움표를 두 번 사용 
    -  '%John's Name%' => content like '%John''s Name%'
  - 일정한 규칙에 따라서 변경해서 이용
    - 인코딩
    - URL의 경우 URL Encoding 기법을 이용해서 표현
    - CompanyName=Bandi&Lunis => CompanyName=Bandi&Lunis
