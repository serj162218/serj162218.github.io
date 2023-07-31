---
title: 'Golang Basic - Unit test'
date: 2023-05-19 02:09:40
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
# Unit test
> If it's difficult to write the test, you might consider to break down the function into pieces.
## Initialize before test
If it need to initalize first like connecting database, we can create a `main_test.go` then wrote this below.
```java
    func TestMain(m *testing.M) {
        initializer.Initialize()
        code := m.Run()
        os.Exit(code)
    }
```

## Testing the route
If we want to testing route which used gin. We can create a router, then use `httptest.NewRecorder()` to test this route.
```java
	//set a router
	router := gin.Default()
	router.POST("/register", controller.Register)

	//create a request with a body with JSON
	user := model.User{
		ID:       "test",
		Email:    "test@example.com",
		Password: "123456",
	}
	requestBody, _ := json.Marshal(user)
	req, _ := http.NewRequest("POST", "/register", bytes.NewBuffer(requestBody))
    // Set the header
	req.Header.Set("Content-Type", "application/json")

    //Recorder
	recorder := httptest.NewRecorder()
	router.ServeHTTP(recorder, req)
```

> Remember that if the route is for register or something, you need to delete the test data after the test.

## Example for Unit test
### Simple test
```java
func TestVerifyToken(t *testing.T) {
	token := "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJJRCI6InRlc3QiLCJFbWFpbCI6InRlc3RAZXhhbXBsZS5jb20ifQ.F8jgR95R3FAs6L1Cn4-eZpLv7N2tKJ7r5VmuqBDYD6k"
	secretKey := []byte("test-key")

	result, err := helper.VerifyToken(token, secretKey)

	if err != nil {
		t.Errorf("expect no error, but got %v", err)
	}

	if !result.Valid {
		t.Errorf("expect result is valided")
	}

	claims, ok := result.Claims.(jwt.MapClaims)
	if !ok {
		t.Errorf("expect result claims to be MapClaims")
	}

	if claims["Email"] != "test@example.com" {
		t.Errorf("expect result Email to be test@example.com")
	}
	if claims["ID"] != "test" {
		t.Errorf("expect result ID to be test")
	}
}
```
### Route test
```java
func TestUserRegister(t *testing.T) {
	//set a router
	router := gin.Default()
	router.POST("/register", controller.UserRegister)

	//create a request with a body with JSON
	user := model.User{
		ID:       "test",
		Email:    "test@example.com",
		Password: "123456",
	}
	requestBody, _ := json.Marshal(user)
	req, _ := http.NewRequest("POST", "/register", bytes.NewBuffer(requestBody))
	req.Header.Set("Content-Type", "application/json")

	recorder := httptest.NewRecorder()

	router.ServeHTTP(recorder, req)

	// Check the status code is what we expect.
	if status := recorder.Code; status != http.StatusOK {
		t.Errorf("handler returned wrong status code: got %v want %v",
			status, http.StatusOK)
	}

	// Check the response body is what we expect.
	expectedResponse := `{"message":"User registered"}`
	if recorder.Body.String() != expectedResponse {
		t.Errorf("handler returned unexpected body: got %v want %v",
			recorder.Body.String(), expectedResponse)
	}

	//Remember that this function is for register. So we need to delete user after testing.
	initializer.DB.Exec("DELETE FROM users WHERE id = 'test'")
}
```
### Test with Redis
```java
/**
 * Test that is the token add into black list successfully
 * Remember to delete the token from redis after the test is done
 */
func TestAddTokenToBlacklist(t *testing.T) {
	token := "test token"
	result := model.AddTokenToBlacklist(token)
	if result != nil {
		t.Errorf("expected no error, but got %v", result)
	}
	isExist, err := initializer.RDB.SIsMember(context.Background(), "black_list", token).Result()
	if err != nil {
		t.Errorf("expected no error, but got %v", err)
	}
	if isExist != true {
		t.Errorf("expected %v, but got %v", true, isExist)
	}
	initializer.RDB.SRem(context.Background(), "black_list", token)
}
```