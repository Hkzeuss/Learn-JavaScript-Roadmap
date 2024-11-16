# `fetch()` trong JavaScript

## Giới Thiệu

Phương thức **`fetch()`** trong JavaScript được sử dụng để gửi yêu cầu đến máy chủ và tải dữ liệu cho các trang web. Yêu cầu này có thể là một API trả về dữ liệu ở các định dạng như JSON hoặc XML. Phương thức `fetch()` trả về một **Promise**, giúp xử lý các tác vụ bất đồng bộ một cách đơn giản và dễ quản lý hơn so với các phương thức cũ như `XMLHttpRequest`.

---

## Cú Pháp

```javascript
fetch(url, options)
    .then((response) => {
        // Xử lý phản hồi khi yêu cầu thành công
    })
    .catch((error) => {
        // Xử lý lỗi khi yêu cầu thất bại
    });
```

- **`url`**: Địa chỉ URL của API hoặc tài nguyên bạn muốn yêu cầu.
- **`options`** (tuỳ chọn): Một đối tượng chứa các thiết lập như phương thức HTTP (GET, POST, PUT, DELETE), headers, body, v.v.

---

## Ví Dụ Sử Dụng `fetch()`

### Lấy Dữ Liệu Với GET Request

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1")
    .then((response) => {
        if (!response.ok) {
            throw new Error("HTTP lỗi: " + response.status);
        }
        return response.json();
    })
    .then((data) => {
        console.log("Dữ liệu nhận được:", data);
    })
    .catch((error) => {
        console.error("Lỗi:", error);
    });
```

### Kết Quả

```plaintext
Dữ liệu nhận được: { id: 1, title: "Tiêu đề bài viết", ... }
```

---

## Gửi Dữ Liệu Với POST Request

`fetch()` cũng hỗ trợ gửi yêu cầu **POST**, để thêm dữ liệu vào máy chủ. Bạn cần chỉ định phương thức là `POST` và cung cấp body dưới dạng JSON hoặc dữ liệu từ form.

### Ví Dụ Gửi Dữ Liệu

```javascript
fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify({
        title: "Bài viết mới",
        body: "Nội dung bài viết",
        userId: 1,
    }),
})
    .then((response) => response.json())
    .then((data) => {
        console.log("Bài viết đã được tạo:", data);
    })
    .catch((error) => {
        console.error("Lỗi:", error);
    });
```

### Kết Quả

```plaintext
Bài viết đã được tạo: { id: 101, title: "Bài viết mới", ... }
```

---

## Kết Hợp Với Async/Await

Dễ dàng kết hợp `fetch()` với **Async/Await** để làm mã bất đồng bộ trở nên rõ ràng và dễ quản lý hơn.

### Ví Dụ với Async/Await

```javascript
async function fetchData() {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        if (!response.ok) {
            throw new Error(`HTTP lỗi! Trạng thái: ${response.status}`);
        }
        const data = await response.json();
        console.log("Dữ liệu nhận được:", data);
    } catch (error) {
        console.error("Lỗi:", error);
    }
}

fetchData();
```

### Kết Quả

```plaintext
Dữ liệu nhận được: { id: 1, title: "Tiêu đề bài viết", ... }
```

---

## Các Tùy Chọn trong `fetch()`

1. **Method**: Phương thức HTTP như `GET`, `POST`, `PUT`, `DELETE`.
2. **Headers**: Các header HTTP, ví dụ như `Content-Type`, `Authorization`.
3. **Body**: Dữ liệu gửi đi (dùng trong yêu cầu `POST`, `PUT`).
4. **Mode**: Cách kiểm soát CORS (Cross-Origin Resource Sharing).

### Ví Dụ Cấu Hình Headers và Body

```javascript
fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer token123",
    },
    body: JSON.stringify({
        title: "Bài viết mới",
        body: "Nội dung bài viết",
        userId: 1,
    }),
})
    .then((response) => response.json())
    .then((data) => console.log("Dữ liệu đã được gửi:", data))
    .catch((error) => console.error("Lỗi:", error));
```

---

## Xử Lý Lỗi

Khi sử dụng `fetch()`, nếu có lỗi xảy ra (ví dụ: lỗi kết nối), phương thức này sẽ không tự động ném lỗi. Thay vào đó, bạn phải kiểm tra **`response.ok`** để đảm bảo rằng phản hồi từ máy chủ là thành công (mã trạng thái HTTP từ 200 đến 299).

### Ví Dụ Kiểm Tra Lỗi

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1000")
    .then((response) => {
        if (!response.ok) {
            throw new Error(`Lỗi HTTP: ${response.status}`);
        }
        return response.json();
    })
    .then((data) => {
        console.log("Dữ liệu nhận được:", data);
    })
    .catch((error) => {
        console.error("Lỗi:", error);
    });
```

### Kết Quả

```plaintext
Lỗi HTTP: 404
```

---

## Kết Luận

Phương thức **`fetch()`**:

1. Là cách hiện đại và đơn giản để gửi yêu cầu HTTP và làm việc với các API.
2. Trả về một **Promise**, giúp dễ dàng xử lý các tác vụ bất đồng bộ.
3. Hỗ trợ nhiều phương thức HTTP như `GET`, `POST`, `PUT`, `DELETE`.
4. Có thể kết hợp với **Async/Await** để làm mã bất đồng bộ dễ hiểu hơn.

Dù `XMLHttpRequest` vẫn có thể được sử dụng, nhưng **`fetch()`** là lựa chọn phổ biến và khuyến nghị trong các ứng dụng JavaScript hiện đại.