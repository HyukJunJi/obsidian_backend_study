SQL 문법의 종류 3가지

## 데이터 정의 언어 (DDL : Data Definition Language)
테이블이나 관계의 구조를 생성하는데 사용하며 CREATE, ALTER, DROP, TRUNCATE 문 등이 있다.
- **CREATE** - 새로운 데이터베이스 관계 (테이블 ) View, 인덱스 , 저장 프로시저 만들기.
- **DROP** - 이미 존재하는 데이터 베이스 관계( 테이블 ) , 뷰, 인덱스,  저장 프로시저를 삭제한다.
- **ALTER** - 이미 존재하는 데이터베이스 개체에 대한 변경, **RENAME**의 역할을 한다.
- **TRUNCATE** - 관계( 테이블 )에서 데이터를 제거한다. (한번 삭제시 돌이킬 수 없음.)
## 데이터 조작 언어 (DML : Data Manipulation Language)
데이블에 데이터 검색, 삽입, 수정, 삭제하는 데 사용하며 SELECT, UPDATE, DELETE, INSERT문 등이 있다.
- **SELECT** - 검색 (질의)
- **INSERT** - 삽입 (등록)
- **UPDATE** - 업데이트 (수정)
- **DELETE** - 삭제

## 데이터 제어 언어 (DCL : Data Control Language)
데이터의 사용 권한을 관리하는 데 사용하며 GRANT, REVOKE 문 등이 있다.

- **GRANT** - 특정 데이터베이스 사용자에게 특정 작업에 대한 수행 권한을 부여한다.
- **REVOKE** - 특정 데이터베이스 사용자에게 특정 작업에 대한 수행 권한을 박탈 OR 회수한다.
데이터베이스 사용자에게 **GRANT 및 **REVOKE**로 설정 할 수 있는 권한.

- **CONNECT** - 데이터베이스 or 스키마에 연결하는 권한.

- **SELECT -** 데이터베이스에서 데이터를 검색할 수 있는 권한

- **INSERT** - 데이터베이스에서 데이터를 등록(삽입)할 수 있는 권한

- **UPDATE** - 데이터베이스의 데이터를 업데이트 할 수 있는 권한

- **DELETE** - 데이터베이스의 데이터를 삭제할 수 있는 권한.

- **USAGE -** 스키마 또는 함수와 같은 데이터베이스 개체를 사용할 수 있는 권한

출처: [https://zzdd1558.tistory.com/88](https://zzdd1558.tistory.com/88) [YundleYundle:티스토리]
[[PostgreSQL]]
[[MySQL]]
[[MariaDB]]
[[MS SQL]]
[[Oracle]]

다음과정
[[NoSQL Databases]]
