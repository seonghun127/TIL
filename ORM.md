## ORM(Object Relational Mapping)

- 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해주는 것

- SQL문을 직접 작성하지 않고 관계형 데이터베이스의 엔티티를 JS의 객체로 표현

- 장점

  1. 객체 지향적인 코드로 비즈니스 로직에 더 집중 가능

     → 복잡한 쿼리문을 사용하지 않고, class와 instance로 처리할 수 있다.

  2. 재사용 및 유지부수의 편리증 증가

- 단점

  1. 완벽한 ORM으로만 서비스를 구현하기 어렵다

  2. Raw Query를 사용하는 것 보다 성능이 떨어진다.

     

## Sequelize

- nodejs에서 RDB를 쉽게 다룰 수 있도록 도와주는 라이브러리(ORM)
- `bootstraping` : 프로젝트 초기 단계를 자동으로 설정할 수 있도록 도와주는 일(Create-react-app 같은 역할?)
- `migration` : 코드에 적혀있는 데이터베이스 테이블에 대해서 **실제 데이터베이스에 테이블을 생성** (github commit 느낌)
- `models`: DB 테이블 스키마를 표현하는 수단
- 모델 생성: `model:generate --name 테이블이름 --attributes 필드1, 필드2..`
- 모델 생성 후, 디폴트 값 설정(이 필요하다면)을 끝내고 마이그레이션
  - `sequelize db:migrate`
  - `sequelize db:undo` // 마이그레이션 되돌릴 때 사용
- `Seed`: 생성된 테이블에 데이터를 추가할 수 있게 해주는 기능