## 문제상황
스키마명.테이블명이 나올경우 어떻게 표현할 것인가에 대한 문제에 봉착했습니다.

## 예시
예를 들면
select * from a.table_A 를 select * from ?.? 로 표현할 수 있을것인가에 대한 문제였습니다.
실제 쿼리를 돌려봤을 때 BadSQLGrammer Exception 에러가 발생하는데요.

?에 값을 바인딩 했을때
a. 이란 스키마명을 찾을 수 없다는 예외가 발생합니다.
즉, a라는 스키마가 매핑 되어야 하는데 a.으로 잘못 매핑 된다는 문제가 발생하는 것이죠.

## 해결책
var sql = String.format("select * from %s.%s", schema, table);
return jdbcTemplate.update(sql);

을 사용하여 문제를 해결했습니다.

## 그 외의 해결책
GPT 3.5버전에 물어본 후 답변 내용은
String sql = "SELECT column1, column2 FROM \"my schema\".\"my_table\" WHERE condition = ?";
List<Map<String, Object>> result = jdbcTemplate.queryForList(sql, yourParameterValue);
이런식으로 사용할 수 있다고 합니다. 


## 마무리
현재로선 String.format을 사용하여 sql변수를 만든 후 jdbcTemplate의 update 메서드를 진행하는 것이 좋다고 생각됩니다.

다른 좋은 해결책이 있다면 공유 부탁드립니다.
