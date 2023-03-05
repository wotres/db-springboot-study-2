# DB study
## Summary
### 데이터 접근 기술
#### SQLMapper
* SQL 작성시 해당 SQL 의 결과를 객체로 매핑
* JDBC 직접 사용시 발생하는 중복 제거
* JdbcTemplate / MyBatis
#### ORM
* 기본적인 SQL은 JPA 가 대신 작성하고 처리
* 개발자가 저장하고싶은 객체를 자바 컬렉션에 저장하고 조회하면 ORM 기술이 데이터베이스에서 저장하고 조회
* JPA 는 ORM 표준
* Hibernate 는 가장 많이 사용하는 JPA 구현체
* 자바에서 ORM 사용시 JPA 인터페이스 사용 및 구현체로 하이버네이트를 사용
* JPA / Hibernate / 스프링 데이터 JPA / Querydsl

### DTO(data transfer object)
* 데이터 전송객체
* 기능은 없고 데이터 전달만 하는 용도

### 키선택
* 자연키(주민등록번호) 와 대리키(생성된 키) 중 대리키를 사용하자 
  * ex. id bigint generated by default as identity,

### JDBCTemplate
* JdbcTemplate 는 DataSource 필요
* template.queryForObject() 는 결과 로우가 하나일 때 사용
* template.query() 는 결과 하나이상일때 사용
  * template.query(sql, itemRowMapper(), param.toArray())
  * RowMapper 는 반환 결과인 ResultSet 을 객체로 변환
* MemoryConfig 는 Map<Long, Item> 으로 저장
* 이름 지정 파라미터
  * Map
  * SqlParameterSource (인터페이스)
    * MapSqlParameterSource (구현체) => 메서드 체인
    * BeanPropertySqlParameterSource => 자바빈 프로퍼티 규약을 통해 자동으로 파라미터 생성
      * getItemName 이 있다면 key=itemName, value=값 
* 자동 생성시 메서드와 컬럼이 맞지않으면 별칭 사용
  * select item_name as itemName
* NamedParameterJdbcTemplate => 이름 기반 파라미터 바인딩을 지원
* 동적 쿼리를 해결하지못함


## Code
* @EventListener(ApplicationReadyEvent.class) : 스프링 컨테이너가 초기화를 다 끝내고 실행 준비시 발생하는 이벤트
* @Import(MemoryConfig.class) : 설정 파일로 사용
* @Profile("local") : 특정 프로필의 경우에만 해당 스프링 빈을 등록
  * application.properties 의 spring.profiles.active 속성을 읽음