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
