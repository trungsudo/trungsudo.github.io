---
title: Strategy Design Pattern
categories: [design pattern, behavioral ctural pattern]
tags: [design pattern, behavioral pattern, ai generated]
mermaid: true
---

# Strategy Design Pattern

## Vấn đề

- Bạn đang phát triển một ứng dụng vẽ đồ họa.
- Trong class `GraphicsApp`, bạn có một phương thức `drawShape(Shape shape)`.
- Ban đầu, ứng dụng chỉ hỗ trợ vẽ hình vuông và hình tròn. Nhưng sau này, bạn muốn mở rộng ứng dụng để vẽ thêm nhiều hình khác như tam giác, ngũ giác, lục giác, vv. Mỗi lần thêm một hình mới, bạn phải sửa đổi phương thức `drawShape()`, điều này vi phạm nguyên tắc mở/đóng trong SOLID, làm cho mã nguồn trở nên khó bảo trì và dễ gây ra lỗi.

## Giải pháp

- `Strategy pattern` có thể giải quyết vấn đề trên.
- Thay vì viết mã để vẽ từng loại hình trong `drawShape()`, chúng ta có thể định nghĩa một interface `DrawStrategy` với một phương thức `draw()`.
- Mỗi lớp cụ thể như `Square`, `Circle`, `Triangle` sẽ triển khai interface này theo cách riêng của nó.
- `GraphicsApp` chỉ cần gọi phương thức `draw()` của `DrawStrategy`, không cần biết chi tiết bên trong.
- Khi cần thêm hình mới, chỉ cần thêm lớp mới triển khai `DrawStrategy`, không cần sửa đổi mã nguồn hiện có.

## Một số ví dụ thực tế

- Trong ứng dụng vẽ đồ họa, chúng ta có thể dùng Strategy pattern để vẽ các loại hình khác nhau.
- Trong ứng dụng nén file, chúng ta có thể dùng Strategy pattern để chọn giữa các thuật toán nén khác nhau như ZIP, RAR, 7z.
- Trong ứng dụng di chuyển đồ vật, chúng ta có thể dùng Strategy pattern để chọn giữa các phương thức di chuyển khác nhau như bằng tay, bằng xe đẩy, bằng xe tải.

## Khái niệm

- `Strategy pattern` bao gồm 3 thành phần chính:

  - `Strategy`: là interface định nghĩa một hành động cụ thể.
  - `ConcreteStrategy`: là các lớp cụ thể triển khai interface `Strategy`.
  - `Context`: là lớp sử dụng `Strategy`. Nó chứa một tham chiếu đến một đối tượng `Strategy` và gọi phương thức của `Strategy` mà không cần biết chi tiết bên trong.

- Sơ đồ UML biểu diễn `Strategy pattern`:

  ```mermaid
  classDiagram
      Context --|> Strategy : uses >
      Strategy <|.. ConcreteStrategy1 : implements >
      Strategy <|.. ConcreteStrategy2 : implements >
  ```

## Code

- Dưới đây là mã nguồn minh họa bằng C++ và Golang.

```cpp
// C++
#include <iostream>

// Strategy
class DrawStrategy {
public:
    virtual void draw() = 0;
};

// ConcreteStrategy
class Square : public DrawStrategy {
public:
    void draw() override {
        std::cout << "Draw square\n";
    }
};

class Circle : public DrawStrategy {
public:
    void draw() override {
        std::cout << "Draw circle\n";
    }
};

// Context
class GraphicsApp {
private:
    DrawStrategy* strategy;
public:
    void setStrategy(DrawStrategy* strategy) {
        this->strategy = strategy;
    }
    void drawShape() {
        strategy->draw();
    }
};

int main() {
    GraphicsApp app;
    Square square;
    Circle circle;

    app.setStrategy(&square);
    app.drawShape();  // Draw square

    app.setStrategy(&circle);
    app.drawShape();  // Draw circle

    return 0;
}
```

```go
// Golang
package main

import "fmt"

// Strategy
type DrawStrategy interface {
    Draw()
}

// ConcreteStrategy
type Square struct{}

func (s Square) Draw() {
    fmt.Println("Draw square")
}

type Circle struct{}

func (c Circle) Draw() {
    fmt.Println("Draw circle")
}

// Context
type GraphicsApp struct {
    strategy DrawStrategy
}

func (g *GraphicsApp) SetStrategy(s DrawStrategy) {
    g.strategy = s
}

func (g *GraphicsApp) DrawShape() {
    g.strategy.Draw()
}

func main() {
    app := &GraphicsApp{}
    square := &Square{}
    circle := &Circle{}

    app.SetStrategy(square)
    app.DrawShape()  // Draw square

    app.SetStrategy(circle)
    app.DrawShape()  // Draw circle
}
```

## Ưu nhược điểm

### Ưu điểm

- Tăng khả năng mở rộng: khi cần thêm hành động mới, chỉ cần thêm lớp mới triển khai `Strategy`, không cần sửa đổi mã nguồn hiện có.
- Tăng khả năng tái sử dụng: các `ConcreteStrategy` có thể được sử dụng lại ở nhiều nơi trong chương trình.
- Tách biệt mã nguồn: `Context` không cần biết chi tiết bên trong `Strategy`, giúp mã nguồn dễ đọc và dễ bảo dưỡng hơn.

### Nhược điểm

- Tăng số lượng lớp: mỗi hành động cần một lớp `ConcreteStrategy`.
- Khách hàng phải biết về các `ConcreteStrategy` để chọn đúng `Strategy`.

## So sánh với các design pattern khác

- `Strategy pattern` giống `State pattern` vì cả hai đều dựa trên composition để thay đổi hành vi của object tại runtime. Nhưng trong `State pattern`, các state thường *biết lẫn nhau* và _có thể thay đổi state của context_. Trong khi đó, các strategy thường *độc lập* và _không biết lẫn nhau_.
- `Strategy pattern` giống `Template Method pattern` vì cả hai đều cung cấp cách để thay đổi phần của thuật toán. Nhưng `Template Method pattern` sử dụng inheritance và cần override phương thức để thay đổi phần của thuật toán, trong khi `Strategy pattern` sử dụng composition và cần thay đổi object để thay đổi phần của thuật toán.
