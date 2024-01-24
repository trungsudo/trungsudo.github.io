---
title: Factory Design Pattern
categories: [design pattern, creational pattern]
tags: [design pattern, creational pattern]
mermaid: true
---


# Factory Design Pattern

## Bối cảnh sử dụng

- Hãy tưởng tượng bạn là một lập trình viên đang phát triển một ứng dụng đọc sách điện tử.
- Ứng dụng của bạn hỗ trợ nhiều định dạng sách khác nhau như PDF, ePub, Mobi... Mỗi khi người dùng mở một cuốn sách, ứng dụng cần tạo ra một đối tượng `Book` tương ứng với định dạng của cuốn sách đó.
- Nếu chúng ta sử dụng phương pháp truyền thống để khởi tạo đối tượng `Book` cho mỗi định dạng sách, chúng ta sẽ phải sử dụng nhiều câu lệnh `if-else` hoặc `switch-case`. Điều này làm cho mã nguồn trở nên phức tạp và khó bảo trì.
- Đây chính là lúc mà `Factory Design Pattern` có thể giúp ích.

## Khái niệm: 

- `Factory Design Pattern` thuộc nhóm khái niệm tạo lập *(Creational Pattern)*.
- Mục đích của nó là cung cấp một giao diện để tạo ra các đối tượng trong một lớp cha, nhưng cho phép các lớp con thay đổi loại đối tượng sẽ được tạo.

<div class="mermaid">
classDiagram
direction LR

BookFactory <|-- PDFBookFactory
BookFactory <|-- EpubBookFactory
Book <|-- PDFBook
Book <|-- EpubBook
Book <.. BookFactory

class BookFactory {
  <<interface>>
  +createBook() *Book*
}
note for PDFBookFactory "Book* createBook(){ return new PDFBookFactory() }"
class PDFBookFactory {
  +createBook() Book*
}

class EpubBookFactory {
  +createBook() Book*
}

class Book {
  <<abstract>>
  +readBook() *
}
class PDFBook {
  +readBook()
}
class EpubBook {
  +readBook() 
}
</div>

## Cách hoạt động

- `Factory Design Pattern` hoạt động bằng cách định nghĩa một interface hoặc lớp trừu tượng để tạo đối tượng, nhưng để cho lớp con quyết định lớp nào để khởi tạo.
- Mục đích của việc này là để tạo ra một lớp chung cho việc khởi tạo đối tượng, nhưng tại thời điểm runtime, chúng ta có thể xác định lớp cụ thể nào sẽ được tạo.
- Thay vào đó, nếu sử dụng `Factory Design Pattern`, chúng ta chỉ cần định nghĩa một interface `BookFactory` có phương thức `createBook()`. Mỗi lớp định dạng sách sẽ triển khai interface này và tạo ra đối tượng `Book` tương ứng.

```cpp
class Book {
public:
    virtual void read() = 0;
};

class PDFBook : public Book {
public:
    void read() override {
        // Read a PDF book
    }
};

class EpubBook : public Book {
public:
    void read() override {
        // Read an Epub book
    }
};

class BookFactory {
public:
    virtual Book* createBook() = 0;
};

class PDFBookFactory : public BookFactory {
public:
    Book* createBook() override {
        return new PDFBook();
    }
};

class EpubBookFactory : public BookFactory {
public:
    Book* createBook() override {
        return new EpubBook();
    }
};
```

- Ta có thể sử dụng như sau:

```cpp
  int bookType = 1;  // 1 cho PDFBook, 2 cho EpubBook
  BookFactory* factory;

  if (bookType == 1) {
      factory = new PDFBookFactory();
  } else if (bookType == 2) {
      factory = new EpubBookFactory();
  } else {
      std::cout << "Loại sách không hợp lệ" << std::endl;
      return 1;
  }

  // Dù chọn loại factory nào thì những bước sau đều giữ nguyên
  Book* book = factory->createBook(); // Tạo sách
  book->read();  // Đọc sách
```

## So sánh với các pattern khác

- `Factory Design Pattern` khác với `Singleton Pattern` ở chỗ nó không chỉ tạo ra một đối tượng duy nhất.
- Nó cũng khác với `Prototype Pattern` vì nó không sao chép đối tượng đã tồn tại mà tạo ra một đối tượng mới.

## Ưu nhược điểm

### Ưu điểm
- `Factory Design Pattern` giúp giảm sự phụ thuộc giữa các module của mã nguồn, giúp mã nguồn dễ dàng mở rộng hơn.
- Nó cũng giúp tạo ra các đối tượng một cách linh hoạt hơn và giảm bớt sự phức tạp khi khởi tạo đối tượng.
  
### Nhược điểm
- Một nhược điểm của `Factory Design Pattern là` nó có thể tạo ra nhiều lớp con chỉ để triển khai phương thức tạo đối tượng, điều này có thể làm cho mã nguồn trở nên phức tạp hơn.
