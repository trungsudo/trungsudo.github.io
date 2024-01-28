---
title: Proxy Design Pattern
categories: [design pattern, structural pattern]
tags: [design pattern, structural pattern, ai generated]
mermaid: true
---

# Proxy Design Pattern

## Vấn đề

- Giả sử bạn đang làm việc với một lớp `DatabaseConnection`.
- Lớp này có một phương thức `executeQuery(query: string)` để thực hiện các truy vấn SQL.
- Tuy nhiên, bạn muốn kiểm soát quyền truy cập vào cơ sở dữ liệu, chẳng hạn như chỉ cho phép thực hiện truy vấn nếu người dùng đã xác thực.
- Đây là lúc mà `Proxy Design Pattern` có thể giúp ích.

## Giải pháp

- `Proxy Design Pattern` cho phép bạn tạo ra một đối tượng đại diện (proxy) cho một đối tượng khác, và thông qua đối tượng đại diện này, bạn có thể kiểm soát quyền truy cập vào đối tượng gốc.
- Trong ví dụ ở trên, bạn có thể tạo một lớp `AuthenticatedDatabaseConnectionProxy` có phương thức `executeQuery(query: string)`. Phương thức này sẽ kiểm tra xem người dùng đã xác thực chưa trước khi gọi phương thức `executeQuery(query: string)` của lớp `DatabaseConnection`.

## Một số ví dụ thực tế

- `Proxy Design Pattern` có thể được sử dụng trong nhiều tình huống thực tế. Ví dụ như nó có thể được sử dụng để kiểm soát quyền truy cập vào các tài nguyên hệ thống như cơ sở dữ liệu, file hệ thống, hoặc các dịch vụ mạng.

## Khái niệm

- `Proxy Design Pattern` bao gồm hai thành phần chính: `RealObject` và `ProxyObject`.
  - `RealObject` là đối tượng mà `ProxyObject` đại diện.
  - `ProxyObject` kiểm soát quyền truy cập vào `RealObject`.

- Sơ đồ UML của Proxy Design Pattern:
  
  ```mermaid
  classDiagram
      class RealObject {
          +request()
      }
      class ProxyObject {
          -realObject: RealObject
          +request()
      }
      ProxyObject --> RealObject
  ```

## Code
- Dưới đây là mã C++ và Golang minh họa cho `Proxy Design Pattern`.

C++:
```cpp
// RealObject
class DatabaseConnection {
public:
    void executeQuery(std::string query) {
        // Execute the query
    }
};

// ProxyObject
class AuthenticatedDatabaseConnectionProxy {
private:
    DatabaseConnection* realObject;
    bool isAuthenticated;
public:
    AuthenticatedDatabaseConnectionProxy() {
        realObject = new DatabaseConnection();
        isAuthenticated = false;
    }
    void authenticate() {
        // Authenticate the user
        isAuthenticated = true;
    }
    void executeQuery(std::string query) {
        if (isAuthenticated) {
            realObject->executeQuery(query);
        } else {
            std::cout << "Not authenticated!" << std::endl;
        }
    }
};
```

Golang:
```go
// RealObject
type DatabaseConnection struct {}

func (db *DatabaseConnection) ExecuteQuery(query string) {
    // Execute the query
}

// ProxyObject
type AuthenticatedDatabaseConnectionProxy struct {
    realObject *DatabaseConnection
    isAuthenticated bool
}

func (proxy *AuthenticatedDatabaseConnectionProxy) Authenticate() {
    // Authenticate the user
    proxy.isAuthenticated = true
}

func (proxy *AuthenticatedDatabaseConnectionProxy) ExecuteQuery(query string) {
    if proxy.isAuthenticated {
        proxy.realObject.ExecuteQuery(query)
    } else {
        fmt.Println("Not authenticated!")
    }
}
```

## Ưu nhược điểm

### Ưu điểm

- Cho phép bạn kiểm soát quyền truy cập vào đối tượng mà không cần sửa đổi đối tượng đó
### Nhược điểm 

- Làm tăng độ phức tạp của mã nguồn do cần phải tạo thêm các lớp proxy.

## So sánh với các design pattern khác

- `Proxy Design Pattern` giống như `Decorator Pattern` vì cả hai đều cho phép bạn thay đổi hành vi của một đối tượng mà không cần sửa đổi đối tượng đó.
- Tuy nhiên, mục đích của `Proxy Pattern` là kiểm soát quyền truy cập, trong khi `Decorator Pattern` là thay đổi hoặc mở rộng hành vi của đối tượng.
