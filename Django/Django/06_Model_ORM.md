# Dùng Model để tạo bảng Database trong Django
## **1) Khái niệm ORM**
- **Object-Relational Mapping** ( **ORM** ) là một kĩ thuật cho phép chúng ta truy vấn và quản lý dữ liệu từ database bằng cách sử dụng mô hình hướng đối tượng . Khi nhắc đến **ORM** , hầu hết mọi người đều nói đến thư viện đã áp dụng kĩ thuật **ORM** .

    <img src=https://i.imgur.com/kwtfEc0.png>

- Vì những thư viện **ORM** là những thư viện đã hoàn chỉnh được viết bằng ngôn ngữ lập trình đang dùng và nó đã gói nhóm code cần thiết để quản lý data, như vậy bạn không cần sử dụng **SQL** nữa mà sẽ tương tác trực tiếp qua cách viết hướng đối tượng của ngôn ngữ đang sử dụng .
## **2) Field và các option trong Model**
### **2.1) Field**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.DecimalField)
### **2.2) Các option của Field**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/topics/db/models/#field-options)
### **2.3) Quan hệ giữa các DB**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/topics/db/models/#relationships)
## **2) Liên kết Database**
- Sử dụng file `models.py` trong folder của app để thiết kế database .
- **VD :** Tạo bảng **Data** trong database có các cột như sau :
    - `id` : lưu id của record
    - `name`: lưu tên, chiều dài tối đa 30 ký tự, không cho phép giá trị `null`
    - `address`: lưu địa chỉ mail, chiều dài tối đa `60` ký tự, không cho phép giá trị `null`
    - `zipcode`: mã địa chỉ thành phố, lưu dưới dạng số nguyên, không cho phép giá trị `null` .
    ```py
    class Data(models.Model):
        id = models.AutoField(primary_key=True)
        name = models.CharField(max_length=30, null=False)
        address = models.EmailField(max_length=30, null=False)
        zipcode = models.IntegerField(null=False)

        def __str__(self):
            return self.name
    ```
    - Trong đó :
        - Khai báo class `Data` kế thừa `models.Model`, là class được viết ra ánh xạ đến bảng trong database .
        - Khai báo field `id` ánh xạ đến cột `id` trong bảng. Field `id` sẽ ở dạng `AutoField`, tự động tăng lên `1` giá trị cho các record mới. Giá trị bắt đầu là `1`
        - Khai báo field `name` ánh xạ đến cột `name` trong bảng. Field `name` sẽ thuộc `CharField` có tham số `max_length=30`, có nghĩa cột `name` sẽ lưu trữ kiểu **string** và dài nhất `30` ký tự .
        - Khai báo field `address` ánh xạ đến cột `address` trong bảng. Field `address` ở dạng `Email`, tự động check các điều kiện cần của một Email, tham số `max_length=30` có nghĩa cột `address` sẽ lưu trữ kiểu **string** và dài nhất `30` ký tự .
        - Khai báo field `zipcode` ánh xạ đến cột `zipcode` trong bảng, Field `zipcode` sẽ ở dạng số nguyên .
## **3) Migrations**
- **Migrations** là những cách **Django** kiểm tra sự thay đổi mà người lập trình đã tác động đến các **models** (thêm hay xoá **model**, thêm sửa xoá các field trong **models**, …) mà người lập trình muốn nó tác động đến **database** . **Migrations** được thiết kế cách tự động, tuy nhiên bạn nên biết để dễ config và chỉnh sửa .
- Khi mới tạo project **Django** , **Django** sẽ mặc định config database là **SQLite** - một dạng database đơn giản , dễ học , dễ làm dự án lúc đầu . Đồng thời trong lần **migrate** đầu tiên, **Django** sẽ tự động tạo ra file `db.sqlite3` để lưu trữ database . Tất cả được thể hiện trong file `settings.py` :
    
    <img src=https://i.imgur.com/1Yu5Rlx.png>

- **B1 :** Tạo file `migration` cho Database để update model trong app `blog` . Sử dụng lệnh `makemigrations` để tạo các file `migration` , báo cho Django biết các model  được thay đổi ở app `home` :
    ```sh
    $ python3 manage.py makemigrations home
    ```
    => Output :
    ```
    Migrations for 'home':
    home/migrations/0001_initial.py
        - Create model Data
    ```
    > Nếu không ghi tên app kèm theo thì **Django** sẽ quét toàn bộ các app hiện tại .
    - Sau khi chạy lệnh , folder `migrations` trong app sẽ có các file `xxxx_initial.py` để lưu lại các sự thay đổi trong model :
        ```sh
        home
        ├── admin.py
        ├── apps.py
        ├── __init__.py
        ├── migrations
        │   ├── 0001_initial.py
        │   └── __init__.py
        ├── models.py
        ├── templates
        │   └── pages
        ├── tests.py
        ├── urls.py
        └── views.py
        ```
        - Nội dung file `0001_initial.py` :

            <img src=https://i.imgur.com/ixTrUeC.png>
- **B2 :** Sử dụng lệnh `migrate` để yêu cầu **Django** chạy các file `migrations` và thực hiện thay đổi đến database :
    ```sh
    $ python3 manage.py migrate
    ```
    ```
    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, home, sessions
    Running migrations:
      Applying home.0001_initial... OK
    ```
    => Kết quả OK có nghĩa là đã migrate thành công .
    > Ngoài các cột `title`, `body` và `date` được tạo thì có thêm cột `id` mặc định được **Django** tạo sẵn (là id để phân biệt các record, giá trị sẽ tự tăng) 

    <div align=center><img src=https://i.imgur.com/9Zcarbh.png width=50%></div>
## **4) Cách tương tác với các database khác**
- **Django** hiện tại hỗ trợ **ORM** cho `5` hệ quản trị cơ sở dữ liệu là: **SQLite**, **MySQL**, **MariaDB**, **PostgreSQL** và **Oracle**.
### **4.1) MySQL**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/ref/databases/#mysql-notes)
### **4.2) MariaDB**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/ref/databases/#mariadb-notes)
### **4.3) PostgreSQL**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/ref/databases/#postgresql-notes)
### **4.4) Oracle**
- [Tham khảo](https://docs.djangoproject.com/en/3.0/ref/databases/#oracle-notes)
## **5) Sử dụng API Python Shell để tương tác Database**
- Để sử dụng những câu lệnh truy vấn trực tiếp đến Database, sử dụng API Shell có sẵn của **Django** .
- Sử dụng lệnh sau để kích hoạt chế độ **shell** :
    ```sh
    $ python3 manage.py shell
    ```
    <img src=https://i.imgur.com/bJ8XKCm.png>
- Muốn truy vấn bảng nào trong Database, cần import vào **shell** :
    ```py
    >>> from blog.models import Post
    ```
### **5.1) Insert giá trị vào bảng**
- Khởi tạo các đối tượng theo hướng **OOP** để insert giá trị vào các bảng :
    ```py
    >>> a = Post()
    >>> a.title = 'First Title'
    >>> a.body = 'First Body'
    >>> a.save()
    >>> b = Post()
    >>> b.title = 'Second Title'
    >>> b.body = 'Second Body'
    >>> b.save()
    ```
    - Hoặc có thể viết gọn lại như sau :
    ```py
    >>> a = Post(title='First Title', body='First Body')
    >>> a.save()
    >>> b = Post(title='Second Title', body='Second Body')
    >>> b.save()
    ```
- Sau khi tạo xong có thể sử dụng **DB Browser for SQLite** để kiểm tra bảng vừa tạo :
    - Thực hiện query truy vấn ( theo cấu trúc `[app_name]_[table_name]` ) :
        ```
        select * from blog_post;   
        ```
    - Kết quả hiển thị :

        <img src=https://i.imgur.com/B6tg6hT.png>

    - Giờ hiển thị ở đây có thể bị sai do cấu hình `TIME_ZONE` trong file `settings.py` đang để mặc định là "`UTC`" . Thay đổi `TIME_ZONE` thành "`Asia/Ho_Chi_Minh`" :
        
        <img src=https://i.imgur.com/96XIedF.png>

### **5.2) Cách select giá trị**
- Để show toàn bộ các record bên trong bảng `post`, sử dụng lệnh sau :
    ```py
    >>> Post.objects.all()
    <QuerySet [<Post: Post object (1)>, <Post: Post object (2)>]>
    ```
- Nếu query theo cách này thì sẽ không thể biết nội dung các record . Ta sẽ sử dụng mẹo để hiển thị các thông tin của record bằng cách thêm hàm `__str__` vào class `Post` :
    
    <img src=https://i.imgur.com/8eqGWZB.png>

    - Kết quả :
        ```py
        >>> Post.objects.all()
        <QuerySet [<Post: First Title>, <Post: Second Title>]>
        ```
### **5.3) Cách update giá trị record**
- Việc cập nhật dữ liệu ở **Django** khá đơn giản , ta chỉ cần truy vấn tìm object Post muốn cập nhật, thay đổi thuộc tính mình cần và dùng phương thức `save()` để cập nhật :
    ```py
    >>> a = Post.objects.get(id=1)
    >>> a.body = 'Changing Body'
    >>> a.save()
    ```