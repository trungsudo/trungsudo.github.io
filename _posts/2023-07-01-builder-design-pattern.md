---
title: Builder Design Pattern
categories: [design pattern, creational pattern]
tags: [design pattern, creational pattern, ai generated]
mermaid: true
---

# Builder Design Pattern

## Vấn đề

- Class `Product` với nhiều thuộc tính và phương thức.
- Việc khởi tạo đối tượng `Product` đòi hỏi nhiều bước và thứ tự cụ thể. Điều này tạo ra sự phức tạp khi viết code và khó khăn trong việc quản lý và bảo trì code.
- Đây chính là vấn đề mà `Builder Design Pattern` có thể giải quyết.

## Giải pháp

- `Builder Design Pattern` giúp tách rời quá trình xây dựng đối tượng khỏi đối tượng được tạo ra.
- Điều này giúp giảm bớt sự phức tạp và tăng tính linh hoạt của code.
- Khi áp dụng pattern này, việc khởi tạo đối tượng `Product` trở nên dễ dàng hơn và code trở nên gọn gàng, dễ đọc hơn.

## Một số ví dụ thực tế

- `Builder Design Pattern` thường được sử dụng trong các tình huống như :
  - Xây dựng các đối tượng phức tạp.
  - Tạo ra các đối tượng có nhiều biến thể.

## Khái niệm

- `Builder Design Pattern` gồm có 4 thành phần chính:
  1. **Product**: Đối tượng cuối cùng mà ta muốn xây dựng.
  2. **Builder**: Interface định nghĩa các bước để xây dựng một đối tượng `Product`.
  3. **ConcreteBuilder**: Implementation cụ thể của `Builder`, xây dựng và trả về đối tượng `Product`.
  4. **Director**: Sử dụng `Builder` để xây dựng đối tượng Product.

- Sơ đồ UML của `Builder Design Pattern`:
  
  ```mermaid
  classDiagram
      Director "1" --> "1" Builder : uses
      Builder <|-- ConcreteBuilder
      Builder "1" --> "1" Product : builds
      class Director{
          +Construct(builder: Builder)
      }
      class Builder{
          +BuildPartA()
          +BuildPartB()
          +GetResult() Product
      }
      class ConcreteBuilder{
          +BuildPartA()
          +BuildPartB()
          +GetResult() Product
      }
      class Product{
          +Add(part: string)
          +Show()
      }
  ```

## Code
- Dưới đây là ví dụ về cách sử dụng `Builder Design Pattern` trong C++ và Golang.

C++:
```cpp
// Product
class Product {
public:
    void Add(string part) {
        parts.push_back(part);
    }
    void Show() {
        for (int i = 0; i < parts.size(); i++) {
            cout << parts[i] << ", ";
        }
    }
private:
    vector<string> parts;
};

// Builder
class Builder {
public:
    virtual void BuildPartA() = 0;
    virtual void BuildPartB() = 0;
    virtual Product* GetResult() = 0;
};

// ConcreteBuilder
class ConcreteBuilder : public Builder {
public:
    void BuildPartA() {
        product->Add("PartA");
    }
    void BuildPartB() {
        product->Add("PartB");
    }
    Product* GetResult() {
        return product;
    }
private:
    Product* product = new Product();
};

// Director
class Director {
public:
    void Construct(Builder* builder) {
        builder->BuildPartA();
        builder->BuildPartB();
    }
};

// Client code
int main() {
    Director* director = new Director();
    Builder* builder = new ConcreteBuilder();
    director->Construct(builder);
    Product* product = builder->GetResult();
    product->Show();
    return 0;
}
```

Golang:
```go
// Product
type Product struct {
    parts []string
}

func (p *Product) Add(part string) {
    p.parts = append(p.parts, part)
}

func (p *Product) Show() {
    fmt.Println(strings.Join(p.parts, ", "))
}

// Builder
type Builder interface {
    BuildPartA()
    BuildPartB()
    GetResult() *Product
}

// ConcreteBuilder
type ConcreteBuilder struct {
    product *Product
}

func (b *ConcreteBuilder) BuildPartA() {
    b.product.Add("PartA")
}

func (b *ConcreteBuilder) BuildPartB() {
    b.product.Add("PartB")
}

func (b *ConcreteBuilder) GetResult() *Product {
    return b.product
}

// Director
type Director struct{}

func (d *Director) Construct(builder Builder) {
    builder.BuildPartA()
    builder.BuildPartB()
}

// Client code
func main() {
    director := &Director{}
    builder := &ConcreteBuilder{product: &Product{}}
    director.Construct(builder)
    product := builder.GetResult()
    product.Show()
}
```

## Ưu nhược điểm

### Ưu điểm
- Tách biệt quá trình xây dựng đối tượng khỏi đối tượng được tạo ra.
- Code dễ đọc, dễ hiểu và dễ bảo dưỡng.
- Tăng tính linh hoạt của code.

### Nhược điểm

- Tạo ra nhiều class và đối tượng không cần thiết, làm tăng độ phức tạp của code.

## So sánh với các design pattern khác
- `Factory Pattern`: Dùng khi việc tạo đối tượng đơn giản và không cần nhiều bước.
- `Prototype Pattern`: Dùng khi việc tạo đối tượng đơn giản nhưng tốn nhiều tài nguyên.
- `Singleton Pattern`: Dùng khi chỉ cần một và chỉ một đối tượng duy nhất trong toàn bộ chương trình.
