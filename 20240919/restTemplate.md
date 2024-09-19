### REST TEMPLATE

- Spring Framework 에서 HTTP 요청을 보내는 클래스
    - GET, POST, PUT, PATCH, DELETE 모두 호출 가능
    - JSON, XML 등의 데이터로 받아올 수 있음

- 해당 페이지 접속
    - https://jsonplaceholder.typicode.com/posts
    - https://jsonplaceholder.typicode.com/posts/2
    
    ```json
    // 20240919091348
    // https://jsonplaceholder.typicode.com/posts/2
    // https://jsonplaceholder.typicode.com/posts?userId=2
    
    {
      "userId": 1,
      "id": 2,
      "title": "qui est esse",
      "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
    }
    ```
    
    - https://jsonplaceholder.typicode.com/posts?userId=2
    
    ```json
    // 20240919091522
    // https://jsonplaceholder.typicode.com/posts?userId=2
    
    [
      {
        "userId": 2,
        "id": 11,
        "title": "et ea vero quia laudantium autem",
        "body": "delectus reiciendis molestiae occaecati non minima eveniet qui voluptatibus\naccusamus in eum beatae sit\nvel qui neque voluptates ut commodi qui incidunt\nut animi commodi"
      },
      {
        "userId": 2,
        "id": 12,
        "title": "in quibusdam tempore odit est dolorem",
        "body": "itaque id aut magnam\npraesentium quia et ea odit et ea voluptas et\nsapiente quia nihil amet occaecati quia id voluptatem\nincidunt ea est distinctio odio"
      },
      {
        "userId": 2,
        "id": 13,
        "title": "dolorum ut in voluptas mollitia et saepe quo animi",
        "body": "aut dicta possimus sint mollitia voluptas commodi quo doloremque\niste corrupti reiciendis voluptatem eius rerum\nsit cumque quod eligendi laborum minima\nperferendis recusandae assumenda consectetur porro architecto ipsum ipsam"
      },
      {
        "userId": 2,
        "id": 14,
        "title": "voluptatem eligendi optio",
        "body": "fuga et accusamus dolorum perferendis illo voluptas\nnon doloremque neque facere\nad qui dolorum molestiae beatae\nsed aut voluptas totam sit illum"
      },
      {
        "userId": 2,
        "id": 15,
        "title": "eveniet quod temporibus",
        "body": "reprehenderit quos placeat\nvelit minima officia dolores impedit repudiandae molestiae nam\nvoluptas recusandae quis delectus\nofficiis harum fugiat vitae"
      },
      {
        "userId": 2,
        "id": 16,
        "title": "sint suscipit perspiciatis velit dolorum rerum ipsa laboriosam odio",
        "body": "suscipit nam nisi quo aperiam aut\nasperiores eos fugit maiores voluptatibus quia\nvoluptatem quis ullam qui in alias quia est\nconsequatur magni mollitia accusamus ea nisi voluptate dicta"
      },
      {
        "userId": 2,
        "id": 17,
        "title": "fugit voluptas sed molestias voluptatem provident",
        "body": "eos voluptas et aut odit natus earum\naspernatur fuga molestiae ullam\ndeserunt ratione qui eos\nqui nihil ratione nemo velit ut aut id quo"
      },
      {
        "userId": 2,
        "id": 18,
        "title": "voluptate et itaque vero tempora molestiae",
        "body": "eveniet quo quis\nlaborum totam consequatur non dolor\nut et est repudiandae\nest voluptatem vel debitis et magnam"
      },
      {
        "userId": 2,
        "id": 19,
        "title": "adipisci placeat illum aut reiciendis qui",
        "body": "illum quis cupiditate provident sit magnam\nea sed aut omnis\nveniam maiores ullam consequatur atque\nadipisci quo iste expedita sit quos voluptas"
      },
      {
        "userId": 2,
        "id": 20,
        "title": "doloribus ad provident suscipit at",
        "body": "qui consequuntur ducimus possimus quisquam amet similique\nsuscipit porro ipsam amet\neos veritatis officiis exercitationem vel fugit aut necessitatibus totam\nomnis rerum consequatur expedita quidem cumque explicabo"
      }
    ]
    ```
    

- Spring 5.0 → WebClient 를 사용할 것을 권장
    - but rest_template으로 진행

- 방식
    - GET 방식
        - getForEntity: Get 요청 후, 응답을 ResponseEntity로 반환
        - getForObject: Get 요청 후, 응답을 원하는 객체로 반환
    - POST 방식
        - postForEntity: Post 요청 후, 응다븡ㄹ ResponseEntity로 반환
        - postForObject: Post 요청 후, 응답을 원하는 객체로 반환
    - Put, Patch, Delete…

- rest template 실습

```java
@GetMapping("/news/{id}")
public ResponseEntity<String> getNewsById(@PathVariable("id") int id) {
	// RestTemplate 인스턴스를 하나 생성
	RestTemplate rt = new RestTemplate();

	// 헤더 인스턴스 생성
	HttpHeaders headers = new HttpHeaders();

	// header 적용
	headers.add("Content-Type", "application/json; charset=UTF-8");
	HttpEntity<String> httpEntity = new HttpEntity<>(headers);

	// URL 주소 만들기
	String url = "https://jsonplaceholder.typicod.com/posts/" + id;

	try {
//			ResponseEntity<String> result = rt.getForEntity(url, String.class);
		ResponseEntity<String> result = rt.exchange(url, HttpMethod.GET, httpEntity, String.class);
		return result;
	} catch (Exception e) {
		return ResponseEntity
				.status(HttpStatus.INTERNAL_SERVER_ERROR)
				.headers(headers).body("에러!");
	}
}
```