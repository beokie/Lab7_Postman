# 🧪 Kiểm Thử API với Postman

> **Môn học:** Kiểm thử phần mềm  
> **Sinh viên:** [Họ và tên của bạn]  
> **MSSV:** [Mã số sinh viên]  
> **Nhóm:** Nhóm 6 – COUR01.LT2  
> **Trường:** Đại học Phenikaa  

---

## 📌 Mục tiêu

Bài thực hành này nhằm:
- Học cách sử dụng công cụ kiểm thử **Postman**
- Thực hiện các HTTP request cơ bản (GET, POST, PUT, DELETE)
- Kiểm tra và xác thực response từ API
- Viết test script tự động trong Postman

---

## 🔗 API được sử dụng: JSONPlaceholder

**Base URL:** `https://jsonplaceholder.typicode.com`

JSONPlaceholder là một REST API giả (mock) miễn phí, thường dùng để kiểm thử và tạo prototype. Nó cung cấp các endpoint sẵn sàng sử dụng, cho phép tester mô phỏng các request API thực mà không cần dựng backend.

### Các endpoint chính:

| Endpoint | Mô tả |
|----------|-------|
| `GET /posts` | Lấy danh sách bài viết |
| `GET /posts/{id}` | Lấy chi tiết 1 bài viết |
| `POST /posts` | Tạo bài viết mới |
| `PUT /posts/{id}` | Cập nhật toàn bộ bài viết |
| `PATCH /posts/{id}` | Cập nhật một phần bài viết |
| `DELETE /posts/{id}` | Xóa bài viết |
| `GET /users` | Lấy danh sách người dùng |
| `GET /comments?postId={id}` | Lấy comment theo bài viết |

---

## ⚙️ Cài đặt & Chuẩn bị

### Bước 1: Cài đặt Postman
1. Truy cập [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. Tải và cài đặt phiên bản phù hợp với hệ điều hành
3. Đăng ký tài khoản miễn phí (hoặc dùng Guest Mode)

### Bước 2: Tạo Collection mới
1. Mở Postman → Click **"New"** → Chọn **"Collection"**
2. Đặt tên: `JSONPlaceholder API Testing`
3. Thêm mô tả: `Kiểm thử các API endpoint của JSONPlaceholder`

---

## 📋 Các Test Case Thực Hiện

---

### Test Case 1: GET – Lấy danh sách bài viết

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_GET_001 |
| **Phương thức** | GET |
| **URL** | `https://jsonplaceholder.typicode.com/posts` |
| **Mục tiêu** | Kiểm tra API trả về danh sách 100 bài viết |

**Kết quả mong đợi:**
- Status Code: `200 OK`
- Response là mảng JSON gồm 100 phần tử
- Mỗi phần tử có các field: `userId`, `id`, `title`, `body`

**Test Script (Tab "Tests" trong Postman):**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is an array of 100 posts", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an('array');
    pm.expect(jsonData.length).to.eql(100);
});

pm.test("Each post has required fields", function () {
    const jsonData = pm.response.json();
    jsonData.forEach(post => {
        pm.expect(post).to.have.property('userId');
        pm.expect(post).to.have.property('id');
        pm.expect(post).to.have.property('title');
        pm.expect(post).to.have.property('body');
    });
});
```

**📸 Ảnh minh hoạ:**

> *(Chụp màn hình Postman sau khi gửi request – hiển thị response body và kết quả test)*

![TC_GET_001](./screenshots/tc_get_001.png)

---

### Test Case 2: GET – Lấy chi tiết 1 bài viết

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_GET_002 |
| **Phương thức** | GET |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Mục tiêu** | Kiểm tra API trả về đúng bài viết có id = 1 |

**Kết quả mong đợi:**
- Status Code: `200 OK`
- Response là object JSON với `id = 1`

**Test Script:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Post ID is 1", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(1);
});

pm.test("Response time is less than 1000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

**📸 Ảnh minh hoạ:**

> *(Chụp màn hình hiển thị response của post id=1)*

![TC_GET_002](./screenshots/tc_get_002.png)

---

### Test Case 3: POST – Tạo bài viết mới

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_POST_001 |
| **Phương thức** | POST |
| **URL** | `https://jsonplaceholder.typicode.com/posts` |
| **Headers** | `Content-Type: application/json` |
| **Mục tiêu** | Kiểm tra tạo bài viết mới thành công |

**Request Body (raw JSON):**
```json
{
  "title": "Bài viết kiểm thử",
  "body": "Đây là nội dung bài viết được tạo qua Postman",
  "userId": 1
}
```

**Kết quả mong đợi:**
- Status Code: `201 Created`
- Response chứa dữ liệu vừa gửi kèm `id` mới

**Test Script:**
```javascript
pm.test("Status code is 201 Created", function () {
    pm.response.to.have.status(201);
});

pm.test("Response contains new post data", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.title).to.eql("Bài viết kiểm thử");
    pm.expect(jsonData.userId).to.eql(1);
    pm.expect(jsonData).to.have.property('id');
});
```

**📸 Ảnh minh hoạ:**

> *(Chụp màn hình POST request với Body và Response 201)*

![TC_POST_001](./screenshots/tc_post_001.png)

---

### Test Case 4: PUT – Cập nhật toàn bộ bài viết

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_PUT_001 |
| **Phương thức** | PUT |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Headers** | `Content-Type: application/json` |
| **Mục tiêu** | Cập nhật toàn bộ nội dung bài viết id=1 |

**Request Body:**
```json
{
  "id": 1,
  "title": "Tiêu đề đã được cập nhật",
  "body": "Nội dung mới sau khi cập nhật",
  "userId": 1
}
```

**Kết quả mong đợi:**
- Status Code: `200 OK`
- Response phản ánh dữ liệu đã cập nhật

**Test Script:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Title has been updated", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.title).to.eql("Tiêu đề đã được cập nhật");
});
```

**📸 Ảnh minh hoạ:**

![TC_PUT_001](./screenshots/tc_put_001.png)

---

### Test Case 5: DELETE – Xóa bài viết

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_DELETE_001 |
| **Phương thức** | DELETE |
| **URL** | `https://jsonplaceholder.typicode.com/posts/1` |
| **Mục tiêu** | Kiểm tra xóa bài viết id=1 thành công |

**Kết quả mong đợi:**
- Status Code: `200 OK`
- Response body là object rỗng `{}`

**Test Script:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response body is empty object", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.eql({});
});
```

**📸 Ảnh minh hoạ:**

![TC_DELETE_001](./screenshots/tc_delete_001.png)

---

### Test Case 6: GET – Lấy comments theo bài viết (Query Params)

| Thông tin | Chi tiết |
|-----------|---------|
| **Tên TC** | TC_GET_003 |
| **Phương thức** | GET |
| **URL** | `https://jsonplaceholder.typicode.com/comments?postId=1` |
| **Params** | `postId = 1` |
| **Mục tiêu** | Kiểm tra lọc comments bằng query parameter |

**Kết quả mong đợi:**
- Status Code: `200 OK`
- Tất cả comments trong response đều có `postId = 1`

**Test Script:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("All comments belong to postId 1", function () {
    const jsonData = pm.response.json();
    jsonData.forEach(comment => {
        pm.expect(comment.postId).to.eql(1);
    });
});

pm.test("Each comment has email field", function () {
    const jsonData = pm.response.json();
    jsonData.forEach(comment => {
        pm.expect(comment).to.have.property('email');
    });
});
```

**📸 Ảnh minh hoạ:**

![TC_GET_003](./screenshots/tc_get_003.png)

---

## 📊 Tổng Hợp Kết Quả

| STT | Test Case | Phương thức | Endpoint | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
|-----|-----------|-------------|----------|-------------------|-----------------|------------|
| 1 | TC_GET_001 | GET | `/posts` | 200 – Array 100 items | 200 ✔ | ✅ PASS |
| 2 | TC_GET_002 | GET | `/posts/1` | 200 – Object id=1 | 200 ✔ | ✅ PASS |
| 3 | TC_POST_001 | POST | `/posts` | 201 – Object mới | 201 ✔ | ✅ PASS |
| 4 | TC_PUT_001 | PUT | `/posts/1` | 200 – Object cập nhật | 200 ✔ | ✅ PASS |
| 5 | TC_DELETE_001 | DELETE | `/posts/1` | 200 – `{}` | 200 ✔ | ✅ PASS |
| 6 | TC_GET_003 | GET | `/comments?postId=1` | 200 – Array comments | 200 ✔ | ✅ PASS |

> *(Cập nhật cột "Kết quả thực tế" và "Trạng thái" sau khi chạy thực tế trên Postman)*

---

## 📁 Cấu trúc Repository

```
postman-api-testing/
│
├── README.md                    # Báo cáo chính (file này)
├── JSONPlaceholder.postman_collection.json   # Export collection Postman
└── screenshots/                 # Ảnh chụp màn hình
    ├── tc_get_001.png
    ├── tc_get_002.png
    ├── tc_post_001.png
    ├── tc_put_001.png
    ├── tc_delete_001.png
    └── tc_get_003.png
```

---

## 📤 Hướng Dẫn Export Collection

Sau khi hoàn thành tất cả test cases, export collection để nộp:

1. Click chuột phải vào Collection `JSONPlaceholder API Testing`
2. Chọn **"Export"**
3. Chọn định dạng **Collection v2.1**
4. Lưu file `JSONPlaceholder.postman_collection.json` vào repo

---

## 📚 Tài Liệu Tham Khảo

- 🎬 [Video hướng dẫn Postman cơ bản](https://www.youtube.com/watch?v=MFxk5BZulVU)
- 📖 [Tài liệu chính thức Postman](https://learning.postman.com/docs/getting-started/introduction/)
- 🌐 [JSONPlaceholder – Free Fake REST API](https://jsonplaceholder.typicode.com/)
- 📘 [Hướng dẫn viết Test Script trong Postman](https://learning.postman.com/docs/writing-scripts/test-scripts/)

---

*Báo cáo được thực hiện bởi Nhóm 6 – COUR01.LT2 – Đại học Phenikaa*
