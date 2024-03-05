---
title: JSON trong Go
categories: [golang]
tags: [json, ai generated]
mermaid: true
---

Go cung cấp package `encoding/json` để xử lý dữ liệu JSON.

**1. Marshalling: Chuyển đổi đối tượng Go thành JSON**

Marshalling cho phép chúng ta chuyển đổi từ đối tượng Go sang chuỗi JSON¹. Dưới đây là một ví dụ đơn giản với struct `User`:

```go
type User struct {
    Id       string `json:"id"`
    Name     string `json:"name"`
    Password string `json:"password"`
}

// an instance of our User struct
user := User{Id: "ID001", Name: "LanKa", Password: "123465"}

byteArray, err := json.Marshal(user)
if err != nil {
    fmt.Println(err)
}

fmt.Println(string(byteArray))
```

**2. Marshalling với Nested Structs**

Chúng ta cũng có thể chuyển đổi các struct lồng nhau¹. Dưới đây là một ví dụ với struct `User` và `Social`:

```go
type User struct {
    Name   string `json:"name"`
    Type   string `json:"type"`
    Age    int    `json:"Age"`
    Social Social `json:"social"`
}

type Social struct {
    Facebook string `json:"facebook"`
    Twitter  string `json:"twitter"`
}

social := Social{Facebook: "https://facebook.com", Twitter: "https://twitter.com"}
user := User{Name: "LanKa", Type: "Author", Age: 25, Social: social}

byteArray, err := json.Marshal(user)
if err != nil {
    fmt.Println(err)
}

fmt.Println(string(byteArray))
```

**3. Unmarshalling: Chuyển đổi JSON thành đối tượng Go**

Unmarshalling cho phép chúng ta chuyển đổi từ chuỗi JSON sang đối tượng Go¹. Dưới đây là một ví dụ:

```go
type Book struct {
    Title  string `json:"title"`
    Author string `json:"author"`
    Pages  int    `json:"pages"`
}

jsonString := `{"title":"Learning Go","author":"John Doe","pages":320}`
var book Book

err := json.Unmarshal([]byte(jsonString), &book)
if err != nil {
    panic(err)
}

fmt.Println(book)
```
