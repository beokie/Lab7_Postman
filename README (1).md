# 🧪 Kiểm Thử API với Postman

> **Môn học:** Đánh giá và kiểm thử chất lượng phần mềm
> **Sinh viên:** Dương Trung Kiên
> **MSSV:** 22010037 


## 📌 Mục tiêu

Bài thực hành này nhằm:
- Học cách sử dụng công cụ kiểm thử **Postman**
- Thực hiện các HTTP request cơ bản (GET, POST, PUT, DELETE)
- Kiểm tra và xác thực response từ API
- Viết test script tự động trong Postman

---

## 🔗 API được sử dụng: JSONPlaceholder

**Base URL:** `https://jsonplaceholder.typicode.com`

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

<img width="1882" height="1051" alt="image" src="https://github.com/user-attachments/assets/bf4ff2a1-ce7c-4e7b-8dc6-26d2d7148c0d" />
<img width="1917" height="952" alt="image" src="https://github.com/user-attachments/assets/da9a60a4-f422-4884-b3cc-9c28fff37cc3" />



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

<img width="1917" height="1037" alt="image" src="https://github.com/user-attachments/assets/8474422a-de8b-4b9f-9533-03e762761968" />
<img width="1917" height="900" alt="image" src="https://github.com/user-attachments/assets/3bd9bd63-12fb-4e91-9bff-e18201bc9bb0" />



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

<img width="1892" height="1072" alt="image" src="https://github.com/user-attachments/assets/68a0608a-a924-4c66-8acb-8969d1546392" />
<img width="1915" height="1085" alt="image" src="https://github.com/user-attachments/assets/7de9fbb3-ed6f-43db-b02c-28a4315fcb2e" />



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

<img width="1917" height="1011" alt="image" src="https://github.com/user-attachments/assets/8d6e4385-1593-4fc4-ab76-c2360818c492" />
<img width="1906" height="1021" alt="image" src="https://github.com/user-attachments/assets/d6988ddf-3984-4608-80d5-a27f874441a9" />


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

<img width="1917" height="1011" alt="image" src="https://github.com/user-attachments/assets/8d6e4385-1593-4fc4-ab76-c2360818c492" />
<img width="1906" height="1021" alt="image" src="https://github.com/user-attachments/assets/d6988ddf-3984-4608-80d5-a27f874441a9" />

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

<img width="1906" height="1092" alt="image" src="https://github.com/user-attachments/assets/bec8fdfa-d513-4a69-b99b-0fc8c66771b5" />
<img width="1902" height="1017" alt="image" src="https://github.com/user-attachments/assets/759917d5-d78f-4b79-b551-bf794447206e" />



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


