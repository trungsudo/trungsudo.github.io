---
title: Decorator Design Pattern
categories: [design pattern, structural pattern]
tags: [design pattern, structural pattern, ai generated]
mermaid: true
---

# Decorator Design Pattern

## Vấn đề

- Giả sử bạn đang làm việc với lớp `Window`, có các phương thức như `draw()` và `getDescription()`.
- Bạn muốn mở rộng chức năng của lớp `Window` mà không làm thay đổi mã nguồn gốc.
- Đây là một vấn đề phổ biến mà các lập trình viên thường gặp phải: làm thế nào để thêm chức năng vào một lớp mà không cần sửa đổi mã nguồn gốc của nó?

## Giải pháp

- `Decorator Design Pattern` có thể giải quyết vấn đề này.
- Pattern này cho phép bạn thêm chức năng vào một đối tượng mà không làm thay đổi mã nguồn gốc của lớp đó.
- Bằng cách sử dụng Decorator Pattern, bạn có thể tạo ra các lớp Decorator mới kế thừa từ lớp `Window` và mở rộng chức năng của nó.

## Một số ví dụ thực tế

- Giao diện người dùng: Trong một ứng dụng giao diện người dùng, bạn có thể muốn thêm các chức năng như cuộn, thay đổi màu sắc, thêm viền vào một cửa sổ. Bằng cách sử dụng Decorator Pattern, bạn có thể tạo ra các lớp như `ScrollableWindow`, `ColoredWindow`, `BorderedWindow` mà không cần sửa đổi lớp `Window` gốc.

- Phân quyền người dùng: Trong một hệ thống quản lý người dùng, bạn có thể muốn thêm các quyền hạn như "Admin", "Editor", "Viewer" cho một người dùng. Bằng cách sử dụng Decorator Pattern, bạn có thể tạo ra các lớp như `AdminUser`, `EditorUser`, `ViewerUser` mà không cần sửa đổi lớp `User` gốc.

- Thao tác với tệp: Trong một ứng dụng xử lý tệp, bạn có thể muốn thêm các chức năng như nén, mã hóa, giải mã cho một tệp. Bằng cách sử dụng Decorator Pattern, bạn có thể tạo ra các lớp như `CompressedFile`, `EncryptedFile`, `DecryptedFile` mà không cần sửa đổi lớp `File` gốc.

## Khái niệm

- Decorator Pattern bao gồm các thành phần sau:

  - **Component**: là một interface định nghĩa các phương thức mà ConcreteComponent và Decorator phải thực hiện.
  - **ConcreteComponent**: là một lớp cụ thể thực hiện Component.
  - **Decorator**: là một lớp trừu tượng kế thừa từ Component và chứa một tham chiếu đến một đối tượng Component.
  - **ConcreteDecorator**: là một lớp cụ thể kế thừa từ Decorator, nó mở rộng chức năng của Component mà nó trang trí.

- Sơ đồ UML của `Decorator Pattern`:

  ```mermaid
  classDiagram
  Component <|.. ConcreteComponent
  Component <|.. Decorator
  Decorator o-- Component
  Decorator <|.. ConcreteDecoratorA
  Decorator <|.. ConcreteDecoratorB
  ```

## Code

- Dưới đây là ví dụ về cách sử dụng `Decorator Pattern` trong C++ và Golang.

### C++

```cpp
// Component
class Window {
public:
    virtual void draw() = 0;
    virtual std::string getDescription() = 0;
};

// ConcreteComponent
class SimpleWindow : public Window {
public:
    void draw() {
        // Draw window
    }

    std::string getDescription() {
        return "Simple window";
    }
};

// Decorator
class WindowDecorator : public Window {
protected:
    Window* windowToBeDecorated; // the Window being decorated

public:
    WindowDecorator (Window* w) : windowToBeDecorated(w) {}
    void draw() {
        windowToBeDecorated->draw();
    }
    std::string getDescription() {
        return windowToBeDecorated->getDescription();
    }
};

// ConcreteDecorator
class VerticalScrollBarDecorator : public WindowDecorator {
public:
    VerticalScrollBarDecorator (Window* w) : WindowDecorator(w) {}

    void draw() {
        drawVerticalScrollBar();
        WindowDecorator::draw();
    }

    std::string getDescription() {
        return WindowDecorator::getDescription() + ", including vertical scrollbars";
    }

private:
    void drawVerticalScrollBar() {
        // Draw the vertical scrollbar
    }
};

int main() {
    // Create a decorated Window with vertical scrollbars
    Window* decoratedWindow = new VerticalScrollBarDecorator(new SimpleWindow());
    // Print the Window's description
    std::cout << decoratedWindow->getDescription() << std::endl;

    return 0;
}
```

### Golang

```go
// Component
type Window interface {
    Draw()
    GetDescription() string
}

// ConcreteComponent
type SimpleWindow struct{}

func (sw *SimpleWindow) Draw() {
    // Draw window
}

func (sw *SimpleWindow) GetDescription() string {
    return "Simple window"
}

// Decorator
type WindowDecorator struct {
    WindowToBeDecorated Window // the Window being decorated
}

func (wd *WindowDecorator) Draw() {
    wd.WindowToBeDecorated.Draw()
}

func (wd *WindowDecorator) GetDescription() string {
    return wd.WindowToBeDecorated.GetDescription()
}

// ConcreteDecorator
type VerticalScrollBarDecorator struct {
    WindowDecorator
}

func (vsbd *VerticalScrollBarDecorator) Draw() {
    vsbd.drawVerticalScrollBar()
    vsbd.WindowDecorator.Draw()
}

func (vsbd *VerticalScrollBarDecorator) GetDescription() string {
    return vsbd.WindowDecorator.GetDescription() + ", including vertical scrollbars"
}

func (vsbd *VerticalScrollBarDecorator) drawVerticalScrollBar() {
    // Draw the vertical scrollbar
}

func main() {
    // Create a decorated Window with vertical scrollbars
    decoratedWindow := &VerticalScrollBarDecorator{
        WindowDecorator: WindowDecorator{
            WindowToBeDecorated: &SimpleWindow{},
        },
    }
    // Print the Window's description
    fmt.Println(decoratedWindow.GetDescription())
}
```

## Ưu nhược điểm

### Ưu điểm

- Cho phép bạn thêm chức năng vào một đối tượng mà không làm thay đổi mã nguồn gốc của lớp đó.
- Tuân thủ nguyên tắc mở rộng mà không sửa đổi (Open/Closed Principle).

### Nhược điểm

- Có thể tạo ra nhiều lớp nhỏ và phức tạp hơn.
- Có thể gây khó khăn khi debug do nó tạo ra nhiều lớp nhỏ.

## So sánh với các design pattern khác

- `Decorator Pattern` khá giống với `Strategy Pattern` vì cả hai đều cho phép bạn thay đổi hành vi của một đối tượng tại thời gian chạy.
- Tuy nhiên, `Strategy Pattern` thay đổi toàn bộ hành vi của đối tượng, trong khi `Decorator Pattern` chỉ thêm hoặc ghi đè một số hành vi.
