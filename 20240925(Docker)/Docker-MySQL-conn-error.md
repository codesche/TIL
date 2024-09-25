### 참고링크

https://seongeun-it.tistory.com/317

### 에러상황

docker-compose.yml 파일을 활용하여 Docker container와 image를 만들었는데 DBeaver 에서 DB 연결이 제대로 되지 않는 문제가 있는 경우 다음과 같이 진행한다.

### 폴더구조

```bash
├── HELP.md
├── Makefile
├── build.gradle
├── db
│   ├── conf.d
│   │   └── my.cnf
│   └── initdb.d
│       ├── create_table.sql
│       └── insert_data.sql
├── docker-compose.yml
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── fastcampus
    │   │           └── pass
    │   │               └── PassBatchApplication.java
    │   └── resources
    │       └── application.yml
    └── test
        └── java
            └── com
                └── fastcampus
                    └── pass
                        └── PassBatchApplicationTests.java

```

### docker-compose.yml

```yaml
version: '3.8'
services:
  mysql:
    container_name: mysql_local
    image: mysql:8.0.30
    volumes:
      - ./db/conf.d:/etc/mysql/conf.d
      - ./db/initdb.d:/docker-entrypoint-initdb.d
    ports:
      - "3307:3307"
    environment:
      - MYSQL_DATABASE=pass_local
      - MYSQL_USER=pass_local_user
      - MYSQL_PASSWORD=passlocal123
      - MYSQL_ROOT_PASSWORD=passlocal123
      - TZ=Asia/Seoul
```

### my.cnf (이 부분이 문제였음)

```yaml
# MySQL8 default character set는 utf8mb4 이므로 client만 선언합니다.
[client]
default-character-set = utf8mb4

# MySQL8 default authentication policy는 `caching_sha2_password`입니다.
# 이를 지원하지 않는 DB Client로 접속하기 위해서는 기존 정책인 `mysql_native_password`로 설정합니다.
[mysqld]
authentication-policy = mysql_native_password

# **(포트가 제대로 설정되어 있지 않았음. 글로벌 포트 설정이 3306으로 되어 있어서 3307로 수정)**
port = 3307
```