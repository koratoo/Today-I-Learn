SQL에서 COALESCE
리스트의 표현식들 중에서 첫 번째 NOT NULL 값을 반환하는 함수입니다. 예를 들어, 다음 쿼리는 Name 컬럼의 값이 NOT NULL이면 Name 컬럼의 값을 반환하고, Name 컬럼의 값이 NULL이면 ID 컬럼의 값을 반환합니다.

SQL

SELECT COALESCE(Name, ID)

FROM Customers;

 

COALESCE 함수는 리스트의 값들 중에서 하나 이상이 NULL일 경우를 처리하는 데 사용할 수 있습니다. 예를 들어, 다음 쿼리는 Name, ID, Age 컬럼 중에서 첫 번째 NOT NULL 값을 반환합니다.

SQL

SELECT COALESCE(Name, ID, Age)

FROM Customers;

 

COALESCE 함수는 매우 다양한 상황에서 사용할 수 있는 매우 유용한 함수입니다. SQL 개발자라면 반드시 알아두어야 할 필수적인 도구입니다.

COALESCE 함수에 대한 몇 가지 추가 세부 사항은 다음과 같습니다.

COALESCE 함수는 임의의 수의 표현식을 인수로 받을 수 있습니다.
COALESCE 함수는 리스트의 표현식들 중에서 첫 번째 NOT NULL 값을 반환합니다.
모든 표현식이 NULL인 경우, COALESCE 함수는 NULL을 반환합니다.
COALESCE 함수는 스칼라 함수이기 때문에, 단일 값을 반환합니다.
 

 

 

ORDER SIBLINGS BY 절
 

SQL 계층 쿼리에서 사용되는 절입니다. 이 절은 동일한 부모를 공유하는 행을 정렬하는 데 사용됩니다. ORDER SIBLINGS BY 절은 정렬을 위해 사용할 열을 지정합니다. 열은 이름 또는 별칭으로 지정할 수 있습니다.

예를 들어, 다음 쿼리는 ORDER SIBLINGS BY 절을 사용하여 동일한 부모를 공유하는 행을 이름순으로 정렬합니다.

코드 스니펫

SELECT employee_id, employee_name, manager_id

FROM employees

START WITH manager_id IS NULL

CONNECT BY PRIOR employee_id = manager_id

ORDER SIBLINGS BY employee_name;

 

이 쿼리의 결과는 다음과 같습니다.

코드 스니펫

employee_id | employee_name | manager_id

----------+-------------+-----------

1          | John Doe     |

2          | Jane Doe     | 1

3          | Mary Smith   | 2

 

ORDER SIBLINGS BY 절은 계층 쿼리에서 행을 정렬하는 데 유용한 도구입니다. 이 절을 사용하여 행을 이름순, 직위순, 입사일순 등으로 정렬할 수 있습니다.

 

반정규화(denormalization)
 

데이터베이스의 성능을 향상시키기 위해 데이터 중복을 허용하는 기법입니다. 반정규화는 다음과 같은 경우에 사용됩니다.

조회 성능이 중요한 경우
데이터의 집계가 중요한 경우
데이터의 유연성이 중요하지 않은 경우
반정규화는 다음과 같은 방법으로 수행할 수 있습니다.

계산된 컬럼 추가
중복 컬럼 추가
뷰 생성
인덱스 생성
반정규화는 데이터베이스의 성능을 향상시킬 수 있지만, 데이터의 중복으로 인해 데이터베이스 크기가 증가하고, 데이터 무결성이 저하될 수 있다는 단점이 있습니다.
