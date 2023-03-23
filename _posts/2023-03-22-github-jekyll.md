---
title: Host a static website with GithubPage
categories: [jekyll, github]
tags: [jekyll, github]
---

# Host a static website with github

- [Github Pages](https://pages.github.com/) cho phép ta host một trang web miễn phí tại đường dẫn *https://\<username\>.github.io*
  - Miễn phí
  - CI/CD sẵn của github nên update/thêm mới dễ dàng
- Ta dùng [Jeklly](https://jekyllrb.com/) để generate website
  - Viết bài bằng Markdown, Liquid mà không cần nhiều kiến thức về HTML & CSS (tất nhiền có thì custom được đẹp hơn rồi)
  - Và nếu không biết HTML hay CSS mà vẫn muốn có site đẹp (nói thừa :D) thì ta có thể dùng [community-maintained theme](https://jekyllrb.com/docs/themes/)

## Create a professional static webpage with Jeklly

> Ta hoàn toàn không cần cài môi trưòng Jeklly ở local mà vẫn có thể deploy trực tiếp lên github vì bản thân GithubPage dùng Jekyll rồi.
{: .prompt-info }

- Nhằm mục đích xem trước các thay đổi mà không cần push lên github nên ta sẽ tiến hành setup môi trường ở local :))

### Setup Jeklly environment

- Có thể thực hiện theo hướng dẫn cài đặt tại [Jekyll Installation](https://jekyllrb.com/docs/installation/). Tuy nhiên ở đây chúng ta dùng docker image [bretfisher/jekyll](https://hub.docker.com/r/bretfisher/jekyll) cho nó nhanh.

- Có thể tạo mới một trang web bằng jekyll bằng lệnh *jekyll new*, nhưng như đã nói ở trên chúng ta sẽ không tạo site từ đầu mà sẽ từ một theme có sẵn. Chọn theme nào thật ưng là ưng rồi clone về, trong ví dụ này ta dùng [Jekyll Theme Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy).

- Cách 1: Chỉ thao tác trên webUI: Làm theo hướng dẫn tại [Getting started](https://chirpy.cotes.page/posts/getting-started/)
  -  Tạo github project mới bằng cách fork từ theme project hoặc từ theme template. Ở đây ta dùng template, truy cập [Chirpy Starter](https://github.com/cotes2020/chirpy-starter), chọn <kbd>Use this template</kbd> > <kbd>Create a new repository</kbd>  
  
- Cách 2: Chỉ dùng dòng lệnh thì làm như sau:

    ```bash
    # Clone theme
    $ git clone https://github.com/cotes2020/jekyll-theme-chirpy.git
    Cloning into 'jekyll-theme-chirpy'...
    remote: Enumerating objects: 8420, done.
    remote: Total 8420 (delta 0), reused 0 (delta 0), pack-reused 8420
    Receiving objects: 100% (8420/8420), 3.65 MiB | 174.00 KiB/s, done.
    Resolving deltas: 100% (4712/4712), done.
    # Start jekyll serve in current theme dir
    $ cd jekyll-theme-chirpy/
    $ docker run -p 8080:4000 -v $(pwd):/site bretfisher/jekyll-serve
    ...
    Auto-regeneration: enabled for '/site'
    Server address: http://0.0.0.0:4000/
    Server running... press ctrl-c to stop.

    ```

    - Để xem web, truy cập http://localhost:8080

### Custom your page

- Để custom được thì ta phải biết sửa ở đâu đã
  - Xem [Directory Structure](https://jekyllrb.com/docs/structure/) để có cái nhìn tổng quan cấu trúc thư mục của một jekll site

- _config.yml

## Host a static website with Github

- Làm theo hướng dẫn của [GithubPage](https://pages.github.com/)
