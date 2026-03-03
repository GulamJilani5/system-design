⏺️ ➡️ 🟦 🔵 🟢 🔴 ⭕🟠 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ HttpHeaders, HttpStatus & ResponseEntity

### ➡️ HttpHeaders

- `org.springframework.http.HttpHeaders`

```java
HttpHeaders headers = new HttpHeaders();
```

##### 🟦 Add Headers

```java
headers.add(String name, String value);
headers.set(String name, String value);
```

##### 🟦 Get Headers

```java
headers.get(String name);
headers.getFirst(String name);
headers.containsKey(String name);
```

##### 🟦 Content-Type

```java
headers.setContentType(MediaType.APPLICATION_JSON);
headers.getContentType();
```

##### 🟦 Authorization

```java
headers.setBearerAuth(String token);
headers.set("Authorization", "Bearer " + token);
headers.setBasicAuth(String username, String password);
```

##### 🟦 Accept

```java
headers.setAccept(List<MediaType>);
headers.getAccept();
```

##### 🟦 Remove Header

```java
headers.remove(String name);
```

##### 🟦 Cache Control

```java
headers.setContentLength(long length);
```

### ➡️ HttpStatus

- `org.springframework.http.HttpStatus`
  It is an enum, not a normal class. 🔴

##### 🟦 1xx – Informational

```java
HttpStatus.CONTINUE
```

##### 🟦 2xx – Success

```java
HttpStatus.OK
HttpStatus.CREATED
HttpStatus.ACCEPTED
HttpStatus.NO_CONTENT
```

##### 🟦 3xx – Redirection

```java
HttpStatus.MOVED_PERMANENTLY
HttpStatus.FOUND
```

##### 🟦 4xx – Client Error

```java
HttpStatus.BAD_REQUEST
HttpStatus.UNAUTHORIZED
HttpStatus.FORBIDDEN
HttpStatus.NOT_FOUND
HttpStatus.CONFLICT
```

##### 🟦 5xx – Server Error

```java
HttpStatus.INTERNAL_SERVER_ERROR
HttpStatus.BAD_GATEWAY
HttpStatus.SERVICE_UNAVAILABLE
```

##### 🟦 Important Methods of HttpStatus

```java
value()                  // returns int (200, 404, etc.)
getReasonPhrase()        // returns "OK", "Not Found"
is2xxSuccessful()
is4xxClientError()
is5xxServerError()
isError()
```

### ➡️ ResponseEntity

- Wraps Body + Headers + Status

##### 🟦

```java

```
