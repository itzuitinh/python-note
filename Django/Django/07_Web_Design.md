# Thiết kế 1 trang CRUD
## **1) Thiết kế toggle Dark Mode**
- **B1 :** Tạo file `custom.css` và file `custom.js`, lồng vào trong file `base.html` :
    ```html
    <link rel="stylesheet" type="text/css" href="{% static 'css/custom.css' %}">
    <script src="{% static 'js/custom.js' %}"></script>
    ```
- **B2 :** Thêm đoạn code sau vào file `custom.css` :
    ```css
    body {
        background-color: white;
        color: black;
    }

    .dark-mode {
        background-color: black;
        color: white;
    }
    ```
- **B3 :** Thêm đoạn code sau vào file `custom.js` :
    ```js
    function toggleDarkMode() {
        var element = document.body;
        element.classList.toggle("dark-mode");
    }
    ```
- **B4 :** Thêm button trong file `base.html`, link với function `toggleDarkMode` vừa tạo :
    ```
    