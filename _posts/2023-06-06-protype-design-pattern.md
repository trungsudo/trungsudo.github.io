---
title: Protype Design Pattern
categories: [design pattern, creational pattern]
tags: [design pattern, creational pattern]
mermaid: true
---

# Prototype Design Pattern

## Vấn đề

- Tình huống cần tạo ra các đối tượng có cấu trúc và trạng thái giống nhau, việc tạo mới từ đầu nhiều đối tượng giống nhau không chỉ tốn thời gian mà còn làm tăng bộ nhớ sử dụng.

## Giải pháp
- `Prototype Design Pattern` ra đời để giải quyết vấn đề này.
- Pattern này cho phép sao chép các đối tượng hiện có mà không làm cho code của bạn phụ thuộc vào các lớp của chúng.
- Điều này giúp giảm bớt thời gian tạo mới đối tượng và giảm bộ nhớ sử dụng.

## Một số ví dụ thực tế
`Prototype Design Pattern` thường được sử dụng trong các tình huống sau:
- Khi việc tạo một đối tượng mới từ đầu là tốn kém về mặt tài nguyên và thời gian.
- Khi bạn muốn giữ lại trạng thái của một đối tượng để sử dụng lại sau này.

## Khái niệm
- `Prototype Design Pattern` bao gồm hai thành phần chính:
  - **Prototype**: đây là interface hoặc abstract class định nghĩa phương thức `clone()`.
  - **ConcretePrototype**: đây là class cụ thể thực hiện phương thức `clone()`.

- Sơ đồ UML thể hiện `Prototype Design Pattern`:

  ```mermaid
  classDiagram
      Prototype <|-- ConcretePrototype
      class Prototype {
          +clone(): Prototype
      }
      class ConcretePrototype {
          +clone(): Prototype
      }
  ```

## Code
- Dưới đây là ví dụ về `Prototype Design Pattern` trong C++ và Golang.

C++:
```cpp
#include <iostream>
#include <memory>

class Prototype {
public:
    virtual std::unique_ptr<Prototype> clone() const = 0;
    virtual void execute() const = 0;
};

class ConcretePrototype : public Prototype {
public:
    std::unique_ptr<Prototype> clone() const override {
        return std::make_unique<ConcretePrototype>(*this);
    }

    void execute() const override {
        std::cout << "Executing the prototype!\n";
    }
};

int main() {
    std::unique_ptr<Prototype> prototype = std::make_unique<ConcretePrototype>();
    std::unique_ptr<Prototype> clonedPrototype = prototype->clone();
    clonedPrototype->execute();
    return 0;
}
```

Golang:
```go
package main

import "fmt"

type Prototype interface {
	Clone() Prototype
	Execute()
}

type ConcretePrototype struct{}

func (p *ConcretePrototype) Clone() Prototype {
	return &ConcretePrototype{}
}

func (p *ConcretePrototype) Execute() {
	fmt.Println("Executing the prototype!")
}

func main() {
	prototype := &ConcretePrototype{}
	clonedPrototype := prototype.Clone()
	clonedPrototype.Execute()
}
```

## Ưu nhược điểm

### Ưu điểm
- Giảm thiểu việc tạo mới từ đầu, giúp tiết kiệm tài nguyên hệ thống.
- Có thể thêm hoặc xóa các đối tượng tại runtime.

## Nhược điểm:
- Việc clone đối tượng có thể gây ra vấn đề với các đối tượng có trạng thái nội tại.

## So sánh với các design pattern khác
- `Factory Method`: thay vì phải tạo mới từ đầu, `Prototype Pattern` cho phép sao chép các đối tượng đã tồn tại.
- `Abstract Factory`: `Prototype` có thể hỗ trợ việc sao chép và tạo mới các đối tượng từ các lớp cụ thể mà không cần phụ thuộc vào interface của chúng.
