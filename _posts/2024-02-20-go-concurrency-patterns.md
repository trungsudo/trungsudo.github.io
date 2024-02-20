---
title: Go Concurrency Patterns
categories: [golang]
tags: [concurrency pattern, ai generated]
mermaid: true
---

# Go Concurrency Patterns

## Golang Goroutine và Channel

### Goroutine

- Goroutine là một trong những đặc trưng nổi bật nhất của ngôn ngữ lập trình Go. 
- Chúng là các luồng thực thi nhẹ (lightweight execution thread), trái với các luồng thông thường do hệ điều hành quản lý, Goroutine được quản lý bởi Go runtime. Điều này giúp giảm bớt chi phí liên quan đến việc khởi tạo và chuyển đổi ngữ cảnh (context switching), làm cho Goroutine "nhẹ" hơn và khởi động nhanh hơn so với các luồng thông thường.
- `Go Scheduler` là một phần của `Go runtime`, nó quản lý việc thực thi của các Goroutine. 
  - `Go Scheduler` sử dụng mô hình M:N, trong đó M Goroutines chạy trên N luồng hệ điều hành. Điều này cho phép lập trình viên tạo ra hàng ngàn `Goroutine` mà không phải lo lắng về việc sử dụng quá nhiều tài nguyên hệ thống.
  - `Go Scheduler` không sử dụng mô hình preemptive, nghĩa là một Goroutine sẽ tiếp tục chạy cho đến khi nó hoàn thành, bị chặn, hoặc tình nguyện nhường quyền điều khiển. Điều này giúp giảm bớt chi phí liên quan đến việc chuyển đổi ngữ cảnh.
- Dưới đây là một ví dụ đơn giản về cách sử dụng Goroutine trong Go:
  
  ```go
  package main
  
  import (
  	"fmt"
  	"time"
  )
  
  func say(s string) {
  	for i := 0; i < 5; i++ {
  		time.Sleep(100 * time.Millisecond)
  		fmt.Println(s)
  	}
  }
  
  func main() {
  	go say("world")
  	say("hello")
  }
  ```

- Trong ví dụ trên, chúng ta tạo ra một Goroutine mới bằng cách sử dụng từ khóa `go`. Hàm `say("world")` sẽ chạy đồng thời với hàm `say("hello")`.

### Channel

- `Channel` trong Golang là một cơ chế cho phép hai hoặc nhiều goroutine có thể trao đổi dữ liệu với nhau và đồng bộ hóa việc thực thi của chúng.
- Hoạt động theo mô hình "First In First Out" (FIFO). Điều này có nghĩa là dữ liệu được gửi đến channel sẽ được lưu trữ trong một hàng đợi, và mỗi lần một goroutine khác đọc dữ liệu từ channel, dữ liệu đầu tiên trong hàng đợi sẽ được lấy ra.
- Channel có thể được tạo ra bằng cách sử dụng từ khóa `make` và có thể chứa các giá trị của bất kỳ kiểu dữ liệu nào. Một channel có thể được đóng lại để ngăn không cho gửi thêm dữ liệu nữa bằng cách sử dụng từ khóa `close`.
- Dưới đây là một ví dụ đơn giản về cách sử dụng channel trong Go:
  
  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	messages := make(chan string)
  
  	go func() {
  		messages <- "hello"
  	}()
  
  	msg := <-messages
  	fmt.Println(msg)
  }
  ```

- Trong ví dụ trên, chúng ta tạo ra một channel mới tên là `messages`. Sau đó, chúng ta tạo ra một goroutine mới, trong đó chúng ta gửi chuỗi "hello" vào channel. Cuối cùng, chúng ta đọc dữ liệu từ channel và in ra màn hình.
- Ví dụ sau minh họa cách sử dụng Goroutine và Channel để tính tổng các số từ 1 đến 10:
  
  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func sum(nums []int, c chan int) {
  	sum := 0
  	for _, n := range nums {
  		sum += n
  	}
  	c <- sum // gửi tổng vào channel
  }
  
  func main() {
  	nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
  	c := make(chan int) // tạo channel kiểu int
  	go sum(nums[:5], c) // khởi tạo goroutine tính tổng nửa đầu
  	go sum(nums[5:], c) // khởi tạo goroutine tính tổng nửa sau
  	x, y := <-c, <-c     // nhận hai giá trị từ channel
  	fmt.Println(x, y, x+y) // in ra kết quả
  }
  ```
