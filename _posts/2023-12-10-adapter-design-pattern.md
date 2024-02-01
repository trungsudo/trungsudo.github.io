---
title: Adapter Design Pattern
categories: [design pattern, structural pattern]
tags: [design pattern, structural pattern, ai generated]
mermaid: true
---

# Adapter Design Pattern

## Vấn đề

- Giả sử bạn đang phát triển một ứng dụng quản lý nhân viên cho một công ty lớn. Bạn đã thiết kế một class `Employee` để lưu trữ thông tin cơ bản của nhân viên như tên, tuổi, số điện thoại, email, v.v.
- Tuy nhiên, công ty bạn cũng có một hệ thống khác để quản lý lương của nhân viên. Hệ thống này sử dụng một class `Salary` để lưu trữ thông tin về lương cơ bản, phụ cấp, thuế, v.v.
- Bạn muốn tích hợp hai hệ thống này để có thể hiển thị thông tin đầy đủ của nhân viên trên một giao diện duy nhất. Tuy nhiên, bạn _không thể sửa đổi code_ của class `Salary` vì nó là một thư viện bên ngoài mà bạn không có quyền truy cập.
- Vậy làm thế nào để bạn có thể sử dụng được class `Salar`y trong ứng dụng của bạn mà không phải viết lại code hoặc thay đổi cấu trúc của class `Employee`?

## Giải pháp

- Một cách giải quyết vấn đề này là sử dụng `Adapter design pattern`. Pattern này cho phép bạn tạo ra một lớp trung gian (adapter) để chuyển đổi giao diện của một lớp (adaptee) thành giao diện mong muốn của một lớp khác (target).
- Trong trường hợp này, bạn có thể tạo ra một lớp `EmployeeAdapter` kế thừa từ lớp `Employee` và chứa một đối tượng của lớp `Salary`. Lớp `EmployeeAdapter` sẽ cung cấp các phương thức để truy xuất và cập nhật thông tin về lương của nhân viên thông qua đối tượng `Salary` bên trong nó.
- Nhờ vậy, bạn có thể sử dụng được lớp `EmployeeAdapter` như một lớp `Employee` bình thường trong ứng dụng của bạn. Bạn không cần phải biết chi tiết về cách hoạt động của lớp `Salary` hay cách chuyển đổi dữ liệu giữa hai lớp.

## Một số ví dụ thực tế

- Một ví dụ thực tế của Adapter design pattern là khi bạn muốn sử dụng một ổ cắm điện có chuẩn khác với chuẩn của quốc gia bạn đang sống. Bạn có thể dùng một bộ chuyển đổi (adapter) để kết nối ổ cắm điện với thiết bị điện của bạn. Bộ chuyển đổi sẽ chuyển đổi điện áp và tần số của nguồn điện cho phù hợp với thiết bị điện.

- Một ví dụ khác là khi bạn muốn sử dụng một thư viện hay framework có giao diện khác với giao diện mong muốn của ứng dụng của bạn. Bạn có thể tạo ra một adapter để chuyển đổi giao diện của thư viện hay framework thành giao diện mong muốn. Ví dụ, bạn có thể tạo ra một adapter để sử dụng một thư viện vẽ đồ họa có giao diện OpenGL trong một ứng dụng sử dụng giao diện DirectX.

- Một ví dụ nữa là khi bạn muốn sử dụng một lớp có giao diện không tương thích với giao diện của lớp cha của nó. Bạn có thể tạo ra một adapter để kế thừa từ lớp cha và chứa một đối tượng của lớp con. Adapter sẽ chuyển đổi các phương thức của lớp con thành các phương thức của lớp cha. Ví dụ, bạn có thể tạo ra một adapter để sử dụng một lớp Duck có phương thức quack() và fly() trong một ứng dụng sử dụng lớp Bird có phương thức sing() và fly().

## Khái niệm

- `Adapter design pattern` là một loại _structural pattern_, tức là nó liên quan đến cách tổ chức và kết nối các đối tượng với nhau.
- Pattern này bao gồm ba thành phần chính:
  
  - **Target**: là interface mong muốn của client, ví dụ như lớp `Employee` trong ví dụ trên.
  - **Adaptee**: là giao interface có của một lớp hay thư viện, ví dụ như lớp `Salary` trong ví dụ trên.
  - **Adapter**: là lớp trung gian để chuyển đổi interface của `adaptee` thành interface của `target`, ví dụ như lớp `EmployeeAdapter` trong ví dụ trên.

- Sơ đồ UML thể hiện `Adapter design pattern`:

  ```mermaid
  classDiagram
  class Target {
    +request()
  }
  class Adaptee {
    +specificRequest()
  }
  class Adapter {
    -adaptee: Adaptee
    +request()
  }
  Target <|-- Adapter
  Adapter o-- Adaptee
  ```
  
## Code

- Dưới đây là code minh họa cho ví dụ về quản lý nhân viên bằng C++ và Golang. Code có comment chi tiết chức năng và nhiệm vụ của các thành phần.

- C++
  
```cpp
// Lớp Employee để quản lý thông tin của nhân viên
class Employee {
  private: string name; // Tên nhân viên
  int id; // Mã nhân viên
  public: Employee(string name, int id) {
    this -> name = name;
    this -> id = id;
  }
  string getName() {
    return name;
  }
  int getId() {
    return id;
  }
};

// Lớp Salary để tính toán lương của nhân viên
class Salary {
  private: double base; // Lương cơ bản
  double bonus; // Thưởng
  public: Salary(double base, double bonus) {
    this -> base = base;
    this -> bonus = bonus;
  }
  double getSalary() {
    return base + bonus;
  }
  void setSalary(double base, double bonus) {
    this -> base = base;
    this -> bonus = bonus;
  }
};

// Lớp EmployeeAdapter để chuyển đổi giao diện của Salary thành Employee
class EmployeeAdapter: public Employee {
  private: Salary * salary; // Đối tượng Salary bên trong EmployeeAdapter
  public: EmployeeAdapter(string name, int id, double base, double bonus): Employee(name, id) {
    salary = new Salary(base, bonus);
  }
  // Phương thức để truy xuất lương của nhân viên
  double getSalary() {
    return salary -> getSalary();
  }
  // Phương thức để cập nhật lương của nhân viên
  void setSalary(double base, double bonus) {
    salary -> setSalary(base, bonus);
  }
};

// Code của client
int main() {
  // Tạo một đối tượng EmployeeAdapter
  EmployeeAdapter * emp = new EmployeeAdapter("Nguyen Van A", 123, 1000, 200);
  // Sử dụng đối tượng EmployeeAdapter như một đối tượng Employee bình thường
  cout << "Name: " << emp -> getName() << endl;
  cout << "ID: " << emp -> getId() << endl;
  cout << "Salary: " << emp -> getSalary() << endl;
  // Cập nhật lương của nhân viên
  emp -> setSalary(1200, 300);
  cout << "New salary: " << emp -> getSalary() << endl;
  return 0;
}
```

- Golang
```go
// Employee is the target interface that defines the methods of an employee
type Employee interface {
	GetName() string
	GetAge() int
	GetPosition() string
	GetSalary() float64
	SetSalary(float64)
}

// EmployeeImpl is a concrete implementation of the Employee interface
type EmployeeImpl struct {
	name     string
	age      int
	position string
}

// NewEmployee creates a new EmployeeImpl with the given name, age and position
func NewEmployee(name string, age int, position string) *EmployeeImpl {
	return &EmployeeImpl{name: name, age: age, position: position}
}

// GetName returns the name of the employee
func (e *EmployeeImpl) GetName() string {
	return e.name
}

// GetAge returns the age of the employee
func (e *EmployeeImpl) GetAge() int {
	return e.age
}

// GetPosition returns the position of the employee
func (e *EmployeeImpl) GetPosition() string {
	return e.position
}

// GetSalary returns the salary of the employee, which is not implemented in this class
func (e *EmployeeImpl) GetSalary() float64 {
	panic("not implemented")
}

// SetSalary sets the salary of the employee, which is not implemented in this class
func (e *EmployeeImpl) SetSalary(float64) {
	panic("not implemented")
}

// Salary is the adaptee class that defines the methods of a salary
type Salary struct {
	baseSalary float64
	bonus      float64
	tax        float64
}

// NewSalary creates a new Salary with the given base salary, bonus and tax
func NewSalary(baseSalary float64, bonus float64, tax float64) *Salary {
	return &Salary{baseSalary: baseSalary, bonus: bonus, tax: tax}
}

// GetBaseSalary returns the base salary of the salary
func (s *Salary) GetBaseSalary() float64 {
	return s.baseSalary
}

// GetBonus returns the bonus of the salary
func (s *Salary) GetBonus() float64 {
	return s.bonus
}

// GetTax returns the tax of the salary
func (s *Salary) GetTax() float64 {
	return s.tax
}

// GetNetSalary returns the net salary of the salary, which is calculated as base salary + bonus - tax
func (s *Salary) GetNetSalary() float64 {
	return s.baseSalary + s.bonus - s.tax
}

// SetBaseSalary sets the base salary of the salary
func (s *Salary) SetBaseSalary(baseSalary float64) {
	s.baseSalary = baseSalary
}

// SetBonus sets the bonus of the salary
func (s *Salary) SetBonus(bonus float64) {
	s.bonus = bonus
}

// SetTax sets the tax of the salary
func (s *Salary) SetTax(tax float64) {
	s.tax = tax
}

// EmployeeAdapter is the adapter class that inherits from Employee and contains a Salary object
type EmployeeAdapter struct {
	*EmployeeImpl // anonymous field to inherit methods from EmployeeImpl
	salary        *Salary // adaptee object to delegate salary-related methods to Salary object
}

// NewEmployeeAdapter creates a new EmployeeAdapter with the given name, age, position and salary object
func NewEmployeeAdapter(name string, age int, position string, salary *Salary) *EmployeeAdapter {
	return &EmployeeAdapter{EmployeeImpl: NewEmployee(name, age, position), salary: salary}
}

// GetSalary returns the net salary of the employee by calling the GetNetSalary method of the Salary object 
func (ea *EmployeeAdapter) GetSalary() float64 {
	return ea.salary.GetNetSalary()
}

// SetSalary sets the base salary of the employee by calling the SetBaseSalary method of the Salary object 
func (ea *EmployeeAdapter) SetSalary(baseSalary float64) {
	ea.salary.SetBaseSalary(baseSalary)
}
```


## Ưu nhược điểm 

### Ưu điểm

- Cho phép sử dụng lại các lớp có giao diện không tương thích với yêu cầu của khách hàng
- Tăng tính linh hoạt và khả năng mở rộng của hệ thống
- Giảm sự phụ thuộc giữa các lớp bằng cách sử dụng thành phần hợp thành (composition) thay vì kế thừa (inheritance)
- Dễ dàng thay đổi hoặc thêm các lớp được thích nghi (adaptee) mà không ảnh hưởng đến các lớp khác

### Nhược điểm

- Tăng số lượng các lớp trong hệ thống, làm cho mã nguồn phức tạp hơn
- Có thể gây hiểu lầm cho người đọc mã nguồn khi không rõ mục đích của các lớp bộ chuyển đổi (adapter)

## So sánh với các design pattern khác

- `Adapter pattern` khác với `Decorator pattern` ở chỗ `Adapter pattern` không thay đổi hoặc bổ sung hành vi cho giao diện được thích nghi, mà chỉ chuyển đổi giao diện này sang một giao diện khác mà khách hàng mong muốn
- `Adapter pattern` khác với `Facade pattern` ở chỗ `Adapter pattern` cho phép các lớp có giao diện không tương thích làm việc với nhau, trong khi `Facade pattern` cung cấp một giao diện đơn giản hơn cho một hệ thống phức tạp
- `Adapter pattern` khác với Bridge pattern ở chỗ `Adapter pattern` làm cho hai giao diện không liên quan trở nên tương thích, trong khi `Bridge patter`n tách rời một trừu tượng (abstraction) khỏi việc triển khai (implementation) của nó
