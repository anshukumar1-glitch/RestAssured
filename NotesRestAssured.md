###### DAY 3

### 1.Path & Query Parameters  
A Path Parameter is part of the URL that identifies a specific resource.
Example URL:
```java
https://jsonplaceholder.typicode.com/posts/10
Here:
10 → Path Parameter

given()
    .pathParam("id", 10)
.when()
    .get("/posts/{id}")
.then()
    .statusCode(200);

```
It identifies a specific post.

Query Parameter is used to filter or modify data.
```java
Example URL:
https://jsonplaceholder.typicode.com/posts?userId=1
Here:
userId=1 → Query Parameter
Used for filtering posts by user.

given()
    .queryParam("userId", 1)
.when()
    .get("/posts")
.then()
    .statusCode(200);
```
Code uses this both for my understanding
```java
  import org.testng.annotations.Test;
import static io.restassured.RestAssured.given;

public class PathAndQueryParameters {

	@Test
	void testQueryAndParameters()
	{
	   given()
	   .pathParam("mypath", "users")//path parameters
	   .queryParam("id",5)//query parameters
	   
	   .when()
	     .get("https://jsonplaceholder.typicode.com/{mypath}")
	   
	   .then()
	   .statusCode(200)
	   .log().all();
	}	
}
```
If path param missing: then server will return 404 Not Found.
Q5: Can we pass multiple values for same query param?

Yes:
/posts?id=1&id=2&id=3
REST Assured:
.queryParam("id", 1,2,3)


### 2.Cookies and headers
  - COOKIES:-Cookies are small pieces of data stored by browser and sent automatically with future requests.

Used for:
Session management
Login persistence
Tracking
🔹 Real Example
When you login to a website:
Server sends:
Set-Cookie: sessionId=abc123
Then browser automatically sends:
Cookie: sessionId=abc123
in future requests.
```java
🔹 REST Assured Example
given()
    .cookie("sessionId", "abc123")
.when()
    .get("/profile")
.then()
    .statusCode(200);
```

Real World Examples:
  2️⃣ Banking Portal
When you login to online banking:
Server creates session
Stores session in DB
Gives session cookie
If cookie expires → logout

- Stateful Communication (Server Remembers You)
HTTP is stateless by default.
Cookies make it behave like stateful.
Without cookies:
Server treats every request as new.
With cookies:
Server remembers:
Who you are
Your role
Your cart
Your preferences

In REST Assured:
1️⃣ Login API
2️⃣ Extract session cookie
3️⃣ Use it in future APIs

Example:
```java
Response res = given()
               .body(loginPayload)
               .post("/login");

String session = res.getCookie("sessionId");

given()
    .cookie("sessionId", session)
.when()
    .get("/dashboard");
```
```java
Usually we:
Create login method in @BeforeClass
Store session globally
Reuse in all tests
Example:

static String session;
@BeforeClass
void login() {
    Response res = given()
                   .body(loginPayload)
                   .post("/login");

    session = res.getCookie("sessionId");
}

Then use:
given()
    .cookie("sessionId", session)
.when()
    .get("/dashboard");
```
```java
 package day3;

import static io.restassured.RestAssured.given;

import java.util.Map;

import org.testng.annotations.Ignore;
import org.testng.annotations.Test;

import io.restassured.response.Response;
public class CookiesDemo {
	@Test(priority=3)
	@Ignore
 void testCookies()
 {
	 given()
	  
	 .when()
	    .get("https://www.google.com/")
	 .then()
	     .cookie("AEC","jhgsdhgjhsdgfhgsfgjejkvdfkj")
	     .log()
	     .all();
 } 
	
	@Test(priority=2)
	void getCookiesInfo()
	 {
		Response res= given()
		 
		 .when()
		    .get("https://www.google.com/");// When We want to capture the entire response then we should not write the then() part.
		
		//How to get single cookie info
//	   String cookie_value=res.getCookie("AEC");
//	   System.out.println("Value of Cookie "+cookie_value);
		
		
		//Get All Cookies Info
		
	Map<String,String>hm=res.getCookies();
		//System.out.println(hm.keySet());
	
	for(String key:hm.keySet())
	{
		String cookie_value=hm.get(key);//res.getCookie(key);
		System.out.println(key+", "+cookie_value);
	}
	 }
}
```
- Headers:



### Day 4:

