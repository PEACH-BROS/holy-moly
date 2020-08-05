# Primary Key & Unique Key에 대하여

## 기본 키 : *Primary Key*

테이블에는 기본 키가 하나만 있을 수 있다.

not null & unique

기본 키로 단 하나의 컬럼이 지정되어 있다면 해당 컬럼의 데이터는 Table 내에서 유일성이 보장된다.

Index 성질이 보장되므로 검색 시 색인의 Key가 되고 Constraint를 갖기 때문에 다른 테이블과 조인을 할 때 기준 값으로 사용된다.



## 고유 키 : *Unique Key*

Uniqueness를 지닌 Index를 말한다.

중복이 허용되지 않지만 null에 대해서는 허용이 된다.

테이블 당 여러 개를 가질 수 있다.