---
title: Protype Design Pattern
categories: [design pattern, creational pattern]
tags: [design pattern, creational pattern]
mermaid: true
---
# Giới thiệu về Prototype Design Pattern

## Vấn đề

- Bạn cần tạo ra một số thể hiện của một lớp cụ thể, hãy gọi là `ComplexObject`, mà việc khởi tạo nó tốn kém về mặt tài nguyên và thời gian.
- Điều này đặc biệt quan trọng khi `ComplexObject` cần được tạo nhanh chóng trong một vòng lặp.
- Trong trường hợp này, việc sử dụng `Prototype Design Pattern` có thể là một giải pháp hiệu quả.

## Giải pháp

- `Prototype Design Pattern` cho phép bạn sao chép các đối tượng hiện có mà không làm cho code của bạn phụ thuộc vào các lớp của chúng.
- Khi áp dụng pattern này, thay vì tạo mới `ComplexObject` từ đầu mỗi lần, bạn có thể sao chép đối tượng đã tồn tại. Điều này giúp tiết kiệm tài nguyên và thời gian, đặc biệt khi việc tạo đối tượng mới là một quá trình tốn kém.

## Một số ví dụ thực tế

- `Prototype Design Pattern` thường được sử dụng trong các tình huống sau:
  - Khi các lớp cụ thể được xác định tại thời gian chạy.
  - Khi việc tạo ra một phiên bản mới của một đối tượng là một quá trình tốn kém và bạn muốn tối ưu hóa hiệu suất.

## Khái niệm

- Prototype Design Pattern bao gồm các thành phần sau:
  - **Prototype**: Đây là interface hoặc lớp trừu tượng định nghĩa phương thức `clone()`.
  - **ConcretePrototype**: Đây là lớp cụ thể thực hiện phương thức `clone()` để tạo và trả về một bản sao của chính nó.
  - **Client**: Đây là lớp sử dụng phương thức `clone()` để tạo ra một bản sao của đối tượng.

- Sơ đồ UML thể hiện Prototype Design Pattern:

  ```mermaid
  classDiagram
      Prototype <|-- ConcretePrototype
      Client --* Prototype
      class Prototype{
          +clone(): Prototype
      }
      class ConcretePrototype{
          +clone(): Prototype
      }
      class Client{
          -prototype: Prototype
      }
  ```

## Code
- Dưới đây là ví dụ về cách sử dụng Prototype Design Pattern trong C++ và Golang:

C++:
```cpp
#include <iostream>
#include <memory>

class Prototype {
public:
    virtual std::unique_ptr<Prototype> clone() const = 0;
    virtual void doSomething() const = 0;
};

class ConcretePrototype : public Prototype {
public:
    std::unique_ptr<Prototype> clone() const override {
        return std::make_unique<ConcretePrototype>(*this);
    }

    void doSomething() const override {
        std::cout << "Doing something...\n";
    }
};

void clientCode(const Prototype& prototype) {
    auto copy = prototype.clone();
    copy->doSomething();
}

int main() {
    ConcretePrototype prototype;
    clientCode(prototype);
    return 0;
}
```

Golang:
```go
package main

import "fmt"

type Prototype interface {
	Clone() Prototype
	DoSomething()
}

type ConcretePrototype struct{}

func (c *ConcretePrototype) Clone() Prototype {
	return &ConcretePrototype{}
}

func (c *ConcretePrototype) DoSomething() {
	fmt.Println("Doing something...")
}

func clientCode(prototype Prototype) {
	copy := prototype.Clone()
	copy.DoSomething()
}

func main() {
	prototype := &ConcretePrototype{}
	clientCode(prototype)
}
```

## Ưu nhược điểm

### Ưu điểm
- Có thể sao chép các đối tượng mà không cần phụ thuộc vào lớp của chúng.
- Có thể loại bỏ việc mã hóa cứng các lớp.
- Có thể giảm số lượng lớp nhà máy mà ứng dụng của bạn cần tạo ra.
- Có thể sao chép các đối tượng có trạng thái phức tạp.
- 
### Nhược điểm

- Sao chép các đối tượng phức tạp có thể là một quá trình tốn kém về mặt tài nguyên.
- Một số lớp con có thể hỗ trợ việc sao chép, trong khi một số lớp khác có thể không hỗ trợ.

## So sánh với các design pattern khác
- **Factory Method**: Prototype có thể hỗ trợ việc sao chép các đối tượng mà không cần phụ thuộc vào lớp của chúng. Factory Method tạo ra một đối tượng mới bằng cách gọi một phương thức nhà máy, trong khi Prototype tạo ra một đối tượng mới bằng cách sao chép một đối tượng hiện có.
- **Abstract Factory**: Prototype có thể được sử dụng khi bạn cần sao chép các đối tượng mà không cần phụ thuộc vào lớp của chúng. Abstract Factory tạo ra một gia đình các sản phẩm liên quan mà không cần chỉ định các lớp cụ thể của chúng.
- **Builder**: Prototype có thể được sử dụng khi bạn cần sao chép các đối tượng mà không cần phụ thuộc vào lớp của chúng. Builder cho phép bạn tạo ra các đối tượng phức tạp theo từng bước.
