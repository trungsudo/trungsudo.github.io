---
title: Composite Design Pattern
categories: [design pattern, structural pattern]
tags: [design pattern, structural pattern, ai generated]
mermaid: true
---

## Vấn đề
- Giả sử một ứng dụng đồ họa cho phép người dùng tạo ra các hình vẽ phức tạp từ các hình cơ bản như đường thẳng, hình tròn, hình chữ nhật.
- Bạn muốn người dùng có thể nhóm các hình cơ bản lại thành một đối tượng duy nhất và thao tác với nó như một hình vẽ riêng biệt. Tuy nhiên, bạn cũng muốn người dùng có thể thay đổi các thuộc tính của từng hình cơ bản trong nhóm một cách linh hoạt. Ví dụ, bạn muốn người dùng có thể di chuyển, xoay, thu phóng cả nhóm hình vẽ hoặc chỉ một hình cụ thể trong nhóm.
- Vấn đề bạn gặp phải là làm sao để thiết kế một cấu trúc lớp cho phép bạn xử lý các đối tượng đơn lẻ và các nhóm đối tượng theo cùng một cách.
  - Nếu bạn sử dụng một lớp cha chung cho tất cả các loại hình vẽ, bạn sẽ phải kiểm tra kiểu của đối tượng trước khi thực hiện các thao tác.
  - Nếu bạn sử dụng nhiều lớp khác nhau cho các loại hình vẽ khác nhau, bạn sẽ phải viết nhiều mã để xử lý các trường hợp khác nhau. Cả hai cách tiếp cận này đều làm cho mã nguồn của bạn phức tạp và khó bảo trì.

## Giải pháp

- Một cách giải quyết hiệu quả cho vấn đề này là sử dụng `Composite design pattern`. Pattern này cho phép bạn xây dựng một cấu trúc cây để biểu diễn các đối tượng phân cấp.
- Trong cây này, các nút lá là các đối tượng đơn lẻ, còn các nút trong là các nhóm đối tượng. Tất cả các nút đều kế thừa từ một lớp cha chung có chứa các phương thức trừu tượng để thực hiện các thao tác chung cho tất cả các loại đối tượng. Nhờ vậy, bạn có thể xử lý các nút lá và các nút trong theo cùng một giao diện, không cần quan tâm đến kiểu của chúng.
- Sau khi áp dụng pattern này, vấn đề sẽ được giải quyết một cách gọn gàng và hiệu quả. Bạn chỉ cần viết một lớp cha `Shape` để định nghĩa các phương thức trừu tượng cho việc di chuyển, xoay, thu phóng và vẽ các hình vẽ. Sau đó, bạn viết các lớp con `Line`, `Circle`, `Rectangle` để kế thừa từ `Shape` và triển khai các phương thức này theo từng loại hình vẽ. Cuối cùng, bạn viết một lớp `CompositeShape` để kế thừa từ `Shape` và chứa một danh sách các đối tượng `Shape` khác. Lớp này cũng triển khai các phương thức của `Shape`, nhưng thay vì thực hiện chúng trên chính nó, nó sẽ gọi chúng trên từng đối tượng con trong danh sách. Như vậy, bạn có thể tạo ra các hình vẽ phức tạp từ các hình vẽ cơ bản và thao tác với chúng như một đối tượng duy nhất.
  
## Một số ví dụ

- Trong `Microsoft Word`, khi bạn chèn một hình ảnh vào văn bản, bạn có thể nhóm nó với các đối tượng khác như văn bản, hình vẽ, biểu đồ... và thao tác với nhóm đó như một đối tượng duy nhất. Bạn có thể di chuyển, xoay, thu phóng cả nhóm hoặc chỉ một thành phần trong nhóm. Đây chính là cách mà `MsWord` sử dụng `Composite pattern` để quản lý các đối tượng đồ họa.

- Trong `Adobe Illustrator`, khi bạn tạo ra một hình vẽ phức tạp từ nhiều hình cơ bản, bạn có thể nhóm chúng lại thành một layer và xem layer đó như một hình vẽ riêng biệt. Bạn có thể di chuyển, xoay, thu phóng cả layer hoặc chỉ một thành phần trong layer. Đây là cách mà Illustrator sử dụng `Composite pattern` để quản lý các layer đồ họa.

- Trong `Google Maps`, khi bạn xem bản đồ của một khu vực, bạn có thể bật hoặc tắt các lớp khác nhau để hiển thị các thông tin khác nhau như địa hình, giao thông, công trình... Mỗi lớp là một nhóm các đối tượng đồ họa có liên quan, và bạn có thể xem cả lớp hoặc chỉ một đối tượng trong lớp. Đây là cách mà `Google Maps` sử dụng `Composite pattern` để quản lý các lớp bản đồ.

## Khái niệm

- `Composite design pattern` là một design pattern thuộc nhóm *structural pattern*, cho phép bạn xây dựng một cấu trúc cây để biểu diễn các đối tượng có quan hệ phân cấp hoặc tổ chức. Bạn có thể xem mỗi đối tượng trong cây là một đối tượng duy nhất hoặc là một nhóm đối tượng gồm nhiều đối tượng con.
- Sử dụng một lớp trừu tượng (abstract class) hoặc một interface để định nghĩa một interface chung cho tất cả các thành phần trong cây. Interface này bao gồm các phương thức để quản lý và thao tác với các thành phần con. Các lớp con sẽ kế thừa hoặc triển khai interface này và tuỳ biến theo nhu cầu.
- Composite pattern bao gồm ba thành phần chính:
  - Component: là lớp cha chung cho tất cả các loại đối tượng trong cây. Nó khai báo các phương thức trừu tượng cho việc quản lý và thao tác các đối tượng con. Nó cũng có thể triển khai các phương thức mặc định cho các nút lá.
  - Leaf: là lớp con của Component, biểu diễn các đối tượng không có con. Nó triển khai các phương thức của Component theo từng loại đối tượng.
  - Composite: là lớp con của Component, biểu diễn các đối tượng có con. Nó chứa một danh sách các đối tượng Component và triển khai các phương thức của Component bằng cách gọi chúng trên từng đối tượng con.

- Để áp dụng `Composite pattern`, bạn cần thiết kế một interface chung cho cả các đối tượng lá (leaf) và các đối tượng tổng hợp (composite). Interface này gọi là Component và nó chứa các phương thức chung cho việc thao tác với các đối tượng. Ví dụ, bạn có thể có một phương thức *operation()* để hiển thị hoặc vẽ các đối tượng.
- Tiếp theo, bạn cần tạo ra các lớp con của `Component` để biểu diễn các loại đối tượng lá và các lớp *Composite* khác nhau.
  - Các lớp lá là những đối tượng không có con và chỉ thực hiện phương thức *operation()* theo cách riêng của chúng.
  - Các lớp *Composite* là những đối tượng có phần tử con và chúng lưu trữ một danh sách các con trong một trường gọi là children. Các lớp tổng hợp cũng cung cấp các phương thức để thêm, xóa hoặc lấy ra các con của chúng, ví dụ *add(), remove()* và *getChild()*. Khi thực hiện phương thức *operation()*, các lớp tổng hợp sẽ duyệt qua danh sách con của chúng và gọi operation() cho từng con.
- Cuối cùng, bạn có thể sử dụng các lớp lá và *Composite* để xây dựng cây của bạn. Bạn có thể kết hợp nhiều lớp tổng hợp lại với nhau để tạo ra các cấu trúc phức tạp hơn. Bạn cũng có thể thêm hoặc xóa các lớp lá vào hoặc ra khỏi các lớp *Composite* một cách linh hoạt.
- Khi bạn cần thao tác với toàn bộ cây hoặc một phần của nó, bạn chỉ cần gọi phương thức *operation()* trên đối tượng gốc của cây hoặc bất kỳ đối tượng nào bạn muốn. Bạn không cần quan tâm đến loại của đối tượng đó là lá hay *Composite*, vì chúng đều tuân theo interface Component chung.

- Sơ đồ UML thể hiện `Composite design pattern`:
  ```mermaid
  classDiagram
  Component <|-- Leaf
  Component <|-- Composite
  Component : +operation()
  Component : +add(Component)
  Component : +remove(Component)
  Component : +getChild(int)
  Leaf : -name
  Leaf : +operation()
  Composite : -children
  Composite : +operation()
  Composite : +add(Component)
  Composite : +remove(Component)
  Composite : +getChild(int)
  ```

## Code

### C++
```cpp
// Định nghĩa một interface chung cho tất cả các thành phần của cấu trúc cây
class Graphic {
    public: virtual~Graphic() {}
    // Vẽ hình
    virtual void draw() = 0;
    // Di chuyển hình
    virtual void move(int x, int y) = 0;
    // Thêm một thành phần vào nhóm
    virtual void add(Graphic * g) {
        throw "Unsupported operation";
    }
    // Xoá một thành phần khỏi nhóm
    virtual void remove(Graphic * g) {
        throw "Unsupported operation";
    }
    // Lấy một thành phần con theo chỉ số
    virtual Graphic * getChild(int index) {
        throw "Unsupported operation";
    }
};

// Định nghĩa một lớp concret cho các thành phần lá (đối tượng đơn giản)
class Line: public Graphic {
    private: int x1,
    y1,
    x2,
    y2; // Tọa độ hai điểm của đường thẳng
    public: Line(int x1, int y1, int x2, int y2): x1(x1),
    y1(y1),
    x2(x2),
    y2(y2) {}
    void draw() override {
        // Vẽ một đường thẳng từ (x1, y1) đến (x2, y2)
        std::cout << "Draw a line from (" << x1 << ", " << y1 << ") to (" << x2 << ", " << y2 << ")\n";
    }
    void move(int x, int y) override {
        // Di chuyển đường thẳng theo vector (x, y)
        x1 += x;
        y1 += y;
        x2 += x;
        y2 += y;
    }
};

// Định nghĩa một lớp concret cho các thành phần composite (đối tượng phức tạp)
class Group: public Graphic {
    private: std::vector < Graphic * > children; // Danh sách các thành phần con
    public: void draw() override {
        // Vẽ tất cả các thành phần con
        for (Graphic * child: children) {
            child -> draw();
        }
    }
    void move(int x, int y) override {
        // Di chuyển tất cả các thành phần con
        for (Graphic * child: children) {
            child -> move(x, y);
        }
    }
    void add(Graphic * g) override {
        // Thêm một thành phần vào nhóm
        children.push_back(g);
    }
    void remove(Graphic * g) override {
        // Xoá một thành phần khỏi nhóm
        children.erase(std::remove(children.begin(), children.end(), g), children.end());
    }
    Graphic * getChild(int index) override {
        // Lấy một thành phần con theo chỉ số
        return children[index];
    }
};

// Mã nguồn của client
int main() {
    // Tạo một đường thẳng
    Line * line = new Line(10, 10, 20, 20);
    // Tạo một nhóm hình vẽ
    Group * group = new Group();
    // Thêm đường thẳng vào nhóm
    group -> add(line);
    // Vẽ nhóm hình vẽ
    group -> draw();
    // Di chuyển nhóm hình vẽ
    group -> move(5, 5);
    // Vẽ lại nhóm hình vẽ sau khi di chuyển
    group -> draw();
    // Xoá đường thẳng khỏi nhóm
    group -> remove(line);
    // Giải phóng bộ nhớ
    delete line;
    delete group;
}
```

### Golang

```go
package main

import "fmt"

// Graphic là interface cho các đối tượng hình vẽ
type Graphic interface {
	// Draw là phương thức để vẽ hình
	Draw()
	// Move là phương thức để di chuyển hình
	Move(x, y int)
	// Add là phương thức để thêm một thành phần vào nhóm
	Add(g Graphic) error
	// Remove là phương thức để xoá một thành phần khỏi nhóm
	Remove(g Graphic) error
	// GetChild là phương thức để lấy một thành phần con của nhóm
	GetChild(index int) (Graphic, error)
}

// Line là lớp triển khai interface Graphic cho đường thẳng
type Line struct {
	x1, y1, x2, y2 int // Tọa độ hai điểm đầu cuối của đường thẳng
}

// NewLine là hàm khởi tạo cho lớp Line
func NewLine(x1, y1, x2, y2 int) *Line {
	return &Line{x1, y1, x2, y2}
}

// Draw là phương thức để vẽ đường thẳng
func (l *Line) Draw() {
	fmt.Printf("Draw a line from (%d, %d) to (%d, %d)\n", l.x1, l.y1, l.x2, l.y2)
}

// Move là phương thức để di chuyển đường thẳng
func (l *Line) Move(x, y int) {
	l.x1 += x
	l.y1 += y
	l.x2 += x
	l.y2 += y
}

// Add là phương thức để thêm một thành phần vào nhóm
// Đối với lớp Line, phương thức này không được hỗ trợ và sẽ trả về lỗi
func (l *Line) Add(g Graphic) error {
	return fmt.Errorf("Unsupported operation")
}

// Remove là phương thức để xoá một thành phần khỏi nhóm
// Đối với lớp Line, phương thức này không được hỗ trợ và sẽ trả về lỗi
func (l *Line) Remove(g Graphic) error {
	return fmt.Errorf("Unsupported operation")
}

// GetChild là phương thức để lấy một thành phần con của nhóm
// Đối với lớp Line, phương thức này không được hỗ trợ và sẽ trả về lỗi
func (l *Line) GetChild(index int) (Graphic, error) {
	return nil, fmt.Errorf("Unsupported operation")
}

// Group là lớp triển khai interface Graphic cho nhóm hình vẽ
type Group struct {
	children []Graphic // Danh sách các thành phần con
}

// NewGroup là hàm khởi tạo cho lớp Group
func NewGroup() *Group {
	return &Group{children: make([]Graphic, 0)}
}

// Draw là phương thức để vẽ nhóm hình vẽ
func (g *Group) Draw() {
	for _, child := range g.children {
		child.Draw()
	}
}

// Move là phương thức để di chuyển nhóm hình vẽ
func (g *Group) Move(x, y int) {
	for _, child := range g.children {
		child.Move(x, y)
	}
}

// Add là phương thức để thêm một thành phần vào nhóm
func (g *Group) Add(child Graphic) error {
	g.children = append(g.children, child)
	return nil
}

// Remove là phương thức để xoá một thành phần khỏi nhóm
func (g *Group) Remove(child Graphic) error {
	for i, c := range g.children {
		if c == child {
			g.children = append(g.children[:i], g.children[i+1:]...)
			return nil
		}
	}
	return fmt.Errorf("Child not found")
}

// GetChild là phương thức để lấy một thành phần con của nhóm
func (g *Group) GetChild(index int) (Graphic, error) {
	if index < 0 || index >= len(g.children) {
		return nil, fmt.Errorf("Index out of range")
	}
	return g.children[index], nil
}

// Mã nguồn của client
func main() {
	// Tạo một đường thẳng
	line := NewLine(10, 10, 20, 20)
	// Tạo một nhóm hình vẽ
	group := NewGroup()
	// Thêm đường thẳng vào nhóm
	group.Add(line)
	// Vẽ nhóm hình vẽ
	group.Draw()
	// Di chuyển nhóm hình vẽ
	group.Move(5, 5)
	// Vẽ lại nhóm hình vẽ sau khi di chuyển
	group.Draw()
	// Xoá đường thẳng khỏi nhóm
	group.Remove(line)
}
```

## Ưu nhược điểm

### Ưu điểm

- Tạo ra các cấu trúc cây phản ánh các đối tượng phức tạp được tạo ra từ các đối tượng đơn giản hơn.
- Thực hiện các thao tác trên cả nhóm đối tượng hoặc từng đối tượng riêng lẻ một cách đồng nhất và minh bạch.
- Tuân thủ nguyên lý `SOLID`, đặc biệt là nguyên lý `Open/Closed` và `Liskov Substitution`.

### Nhược điểm

- Làm cho mã nguồn phức tạp hơn nếu bạn phải xử lý nhiều loại đối tượng khác nhau trong cùng một cấu trúc cây.
- Khó bảo trì hơn nếu bạn thêm hoặc xoá các loại đối tượng mới vào cấu trúc cây.

## So sánh với các design pattern khác

- `Composite design pattern` có điểm chung với `Decorator design pattern` là cả hai đều dựa trên cấu trúc cây để tổ chức các đối tượng. Tuy nhiên, `Decorator design pattern` nhằm mục đích mở rộng chức năng của một đối tượng bằng cách bọc nó bởi một lớp khác, trong khi `Composite design pattern` nhằm mục đích tạo ra các đối tượng phức tạp từ các đối tượng đơn giản hơn.
- `Composite design pattern` có điểm chung với `Flyweight design pattern` là cả hai đều cho phép bạn tiết kiệm bộ nhớ khi làm việc với nhiều đối tượng. Tuy nhiên, `Flyweight design pattern` làm điều này bằng cách chia sẻ các trạng thái bên ngoài của các đối tượng, trong khi `Composite design pattern` làm điều này bằng cách sử dụng các đối tượng nhỏ hơn để xây dựng các đối tượng lớn hơn.

