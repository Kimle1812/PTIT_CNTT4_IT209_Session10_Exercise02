# Bài 2: Đóng gói Backend bằng Single-stage Dockerfile

## 1. Mục tiêu

- Xây dựng REST API đơn giản bằng Node.js và Express.
- Đóng gói ứng dụng bằng Dockerfile Single-stage.
- Build Docker Image và khởi chạy container.
- Kiểm tra ứng dụng thông qua HTTP request.

## 2. Công nghệ sử dụng

- Node.js 18
- Express 4
- Docker
- Docker Compose không bắt buộc trong bài này.

## 3. Cấu trúc thư mục

```text
ex2/
├── Dockerfile
├── package.json
├── server.js
└── README.md
```

## 4. Mô tả ứng dụng

Ứng dụng cung cấp API GET `/`.

Khi truy cập API, server trả về JSON:

```json
{
  "status": "success",
  "message": "Hello from Docker Single-stage!"
}
```

Cổng mặc định của ứng dụng trong container là `3000`.

## 5. Nội dung Dockerfile

Dockerfile sử dụng image nền `node:18-alpine`, thiết lập thư mục làm việc `/app`, sao chép mã nguồn, cài đặt dependency và khởi chạy ứng dụng bằng lệnh `CMD ["node", "server.js"]`.

Đây là Dockerfile Single-stage vì chỉ sử dụng một giai đoạn xây dựng.

## 6. Build Docker Image

Mở terminal tại thư mục chứa Dockerfile và chạy:

```bash
docker build -t hw-backend:v1 .
```

Kiểm tra image:

```bash
docker images
```

## 7. Khởi chạy Container

```bash
docker run -d --name app-single-stage -p 8080:3000 hw-backend:v1
```

Trong đó:
- `-d`: chạy container ở chế độ nền.
- `--name app-single-stage`: đặt tên container.
- `-p 8080:3000`: ánh xạ cổng 8080 của máy host tới cổng 3000 trong container.
- `hw-backend:v1`: image dùng để tạo container.

## 8. Kiểm tra hoạt động

Kiểm tra trạng thái container:

```bash
docker ps
```

Xem log:

```bash
docker logs app-single-stage
```

Gửi HTTP request:

```bash
curl -i http://localhost:8080/
```

Kết quả mong đợi:
- Container có trạng thái `Up`.
- Log hiển thị `Server listening on port 3000`.
- API trả về HTTP `200 OK`.
- Response body chứa `status` là `success` và thông điệp `Hello from Docker Single-stage!`.

## 9. Kết luận

Đã thực hiện đóng gói một ứng dụng REST API Node.js/Express bằng Dockerfile Single-stage, xây dựng image `hw-backend:v1`, khởi chạy container `app-single-stage` và kiểm tra API qua cổng 8080.
