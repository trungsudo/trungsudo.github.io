---
title: Observer Design Pattern
categories: [design pattern, behavioral  pattern]
tags: [design pattern, behavioral  pattern, ai generated]
mermaid: true
---
# Observer Design Pattern

## Vấn đề

- Bạn đang làm việc trên một ứng dụng tin tức.\
- Trong ứng dụng này, bạn có một class `NewsPublisher` chịu trách nhiệm xuất bản tin tức.
- Bạn cũng có nhiều class khác như `EmailSubscriber`, `AppNotificationSubscriber`, và `SMSNotificationSubscriber` chịu trách nhiệm gửi thông báo cho người dùng khi có tin tức mới.
- Vấn đề đặt ra là làm thế nào để `NewsPublisher` có thể thông báo cho tất cả các subscriber mỗi khi có tin tức mới mà không cần biết rõ về chúng.

## Giải pháp

- `Observer Design Pattern` là giải pháp hiệu quả cho vấn đề này.
- `NewsPublisher` sẽ duy trì một danh sách các đối tượng subscriber và thông báo cho tất cả chúng mỗi khi có tin tức mới.
- Các class subscriber sẽ implement một interface chung, giúp `NewsPublisher` có thể tương tác với chúng mà không cần biết chi tiết cụ thể.

## Một số ví dụ thực tế

- **Ứng dụng tin tức**: Như đã mô tả ở trên, Observer Pattern cho phép ứng dụng tin tức thông báo cho người dùng mỗi khi có tin tức mới mà không cần biết rõ về cách thức gửi thông báo.
- **Hệ thống giám sát**: Trong một hệ thống giám sát, các sensor (như sensor nhiệt độ, độ ẩm...) có thể gửi thông báo đến hệ thống giám sát mỗi khi có sự thay đổi.
- **Ứng dụng thời tiết**: Ứng dụng thời tiết có thể gửi thông báo cho người dùng mỗi khi có dự báo thời tiết mới.

## Khái niệm

- `Observer Pattern` bao gồm hai thành phần chính: `Subject` và `Observer`.
  - `Subject` duy trì một danh sách các `Observer` và thông báo cho chúng khi có sự thay đổi.
  - `Observer` là các đối tượng muốn nhận thông báo từ `Subject`.

- Sơ đồ UML của Observer Pattern:
  
  ```mermaid
  classDiagram
      Subject <|-- ConcreteSubject
      Observer <|-- ConcreteObserver
      Subject : +attach(observer: Observer)
      Subject : +detach(observer: Observer)
      Subject : +notify()
      Observer : +update()
  ```

## Code

- Dưới đây là code minh họa cho Observer Pattern bằng C++ và Golang.

```cpp
// C++
#include <iostream>
#include <list>
#include <string>

class Observer {
public:
    virtual void update(const std::string &message) = 0;
};

class Subject {
private:
    std::list<Observer *> observers;
public:
    void attach(Observer *observer) {
        observers.push_back(observer);
    }
    void detach(Observer *observer) {
        observers.remove(observer);
    }
    void notify(const std::string &message) {
        for (Observer *observer : observers) {
            observer->update(message);
        }
    }
};

class ConcreteObserver : public Observer {
private:
    std::string name;
public:
    ConcreteObserver(const std::string &name) : name(name) {}
    void update(const std::string &message) override {
        std::cout << name << " received message: " << message << std::endl;
    }
};

int main() {
    Subject subject;
    ConcreteObserver observer1("Observer 1"), observer2("Observer 2");
    subject.attach(&observer1);
    subject.attach(&observer2);
    subject.notify("Hello, Observer!");
    return 0;
}
```

```go
// Golang
package main

import "fmt"

type Observer interface {
	Update(message string)
}

type Subject struct {
	observers []Observer
}

func (s *Subject) Attach(observer Observer) {
	s.observers = append(s.observers, observer)
}

func (s *Subject) Detach(observer Observer) {
	for i, obs := range s.observers {
		if obs == observer {
			s.observers = append(s.observers[:i], s.observers[i+1:]...)
			break
		}
	}
}

func (s *Subject) Notify(message string) {
	for _, observer := range s.observers {
		observer.Update(message)
	}
}

type ConcreteObserver struct {
	name string
}

func (o *ConcreteObserver) Update(message string) {
	fmt.Println(o.name, "received message:", message)
}

func main() {
	subject := &Subject{}
	observer1 := &ConcreteObserver{name: "Observer 1"}
	observer2 := &ConcreteObserver{name: "Observer 2"}
	subject.Attach(observer1)
	subject.Attach(observer2)
	subject.Notify("Hello, Observer!")
}
```

## Ưu nhược điểm

### Ưu điểm 

- Giảm sự phụ thuộc giữa các class, giúp code dễ mở rộng hơn.

### Nhược điểm
- Nếu có quá nhiều observer hoặc quá nhiều thông báo được gửi, hiệu năng của hệ thống có thể bị ảnh hưởng.

## So sánh với các design pattern khác

- So với `Strategy Pattern`, `Observer Pattern` cho phép nhiều đối tượng cùng lắng nghe và phản hồi trước sự thay đổi của một đối tượng, trong khi `Strategy Pattern` cho phép thay đổi hành vi của một đối tượng mà không cần thay đổi code của đối tượng đó.
