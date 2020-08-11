# Template và Jinja trong Django
## **1) Giới thiệu**
- **Template** là một layout được thiết kế các khung web có sẵn , ta chỉ cần thêm nội dung chính của nó vào , và nhờ các **template** ta mới tiết kiệm thời gian trong việc phát triển website.
- **VD :**
    <img src=https://i.imgur.com/LvZprMC.png>
    <img src=https://i.imgur.com/c3p8hFv.png>
    => Đây là 2 trang có sự tương đồng, sử dụng **template**
- Ta sẽ tạo một **template** chung và chỉ cần đưa nội dung vào đó để tái sử dụng **template** => Sử dụng **template engine** để làm việc này .
- **Jinja** là một **template engine** cho **Python** nhằm tạo các **template** .
## **2) Tạo các template**
- **B1 :** Trong app `home` , tạo một folder tên là `templates` , trong folder đó sẽ tạo thêm 1 folder `pages` . Trong folder `pages` sẽ tạo 1 file khung html là `base.html` :
    ```sh
    home
    ├── admin.py
    ├── apps.py
    ├── __init__.py
    ├── migrations
    │   ├── __init__.py
    │   └── __pycache__
    │       └── __init__.cpython-37.pyc
    ├── models.py
    ├── templates           # New folder
    │   └── pages
    │       └── base.html
    ├── tests.py
    ├── urls.py
    └── views.py
    ```
- **B2 :** Thiết kế file `base.html` trong đó có 2 block để sau này điền nội dung vào.  Một là block `title` nằm trong tag `<title>`, hai là block `content` nằm ở tag `<body>`. Lồng thêm `{% load static %}` để liên kết với thư mục `static`:
    ```html
    <!DOCTYPE html>
      <html lang="en">
      <head>
        {% load static %}
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>{% block title %}{% endblock %}</title>
        <link rel="stylesheet" type="text/css" href="{% static 'css/bootstrap.min.css' %}">
        <script src="{% static 'js/bootstrap.min.js' %}"></script>
        <link href="{% static 'img/favicon.png' %}" rel="icon">
      </head>

      <body>
        <header id="header" class="mt-5">
          <div class="text-center">
            <h2><a class="text-dark text-decoration-none mt-5" href="#">CRUD Examples</a></h2>
          </div>
        </header>
        {% block content %}
        {% endblock %}
      </body>
    </html>
    ```
- **B3 :** Tạo một **template** `home.html` tái sử dụng **template** `base.html` ( file `home.html` nằm trong folder `pages` ) :
    ```sh
    templates/
    └── pages
        ├── base.html
        └── home.html      # New File

    1 directory, 2 files
    ```
- **B4 :** Tạo nội dung cho file `home.html` :
    ```html
    {% extends "pages/base.html" %}

    {% load static %}
    {% block title %}Trang chủ{% endblock %}
    {% block content %}
      <section id="nav">
        <div class="container mt-5">
          <div class="col-lg-8 mx-auto">
            <div class="d-inline">
              <div class="clearfix">
                <div class="float-left">
                  <a class="btn btn-primary js-create-object" style="width: 200%" href="#">Create</a>
                </div>
                <div class="float-right">
                  <form action="#" method="get">
                    <div class="input-group">
                      <input type="text" name="q" class="form-control" placeholder="Search">
                      <div class="input-group-append">
                        <button class="btn btn-primary" type="submit">Search</button>
                      </div>
                    </div>
                  </form> 
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    {% endblock %}
    ```
    => Như vậy các nội dung nằm trong những block **template** `home.html` sẽ được thay thế vào trong `base.html`
- **B5 :** Sửa file `views.py` trong app `home`, gọi template `home.html` :
    ```py
    from django.shortcuts import render
    from django.http import HttpResponse
    # Create your views here.
    def index(request):
        return render(request, 'pages/home.html')
    ```
    > Gọi **template** bằng hàm `render()` . Hàm này có 2 tham số truyền vào : đầu tiên là `request` là request từ máy client , thứ 2 là tên đường dẫn tới **template** muốn dùng . (chỉ cần gọi đường dẫn bên trong folder `templates`)
- **B6 :** Start Server và kiểm tra :
    
    <img src=https://i.imgur.com/j9yvNQA.png>

>### Nguyên lý hoạt động của **Template Jinja**

<img src=https://i.imgur.com/xnDc2sr.png>