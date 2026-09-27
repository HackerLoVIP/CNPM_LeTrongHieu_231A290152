# saleappv1 — Bán hàng trực tuyến (Bài thực hành 2 — Java Spring Boot)

Ứng dụng web bán hàng trực tuyến xây dựng bằng **Spring Boot + Spring MVC + Thymeleaf**,
dữ liệu sản phẩm/danh mục đọc từ tập tin JSON (chưa dùng cơ sở dữ liệu).

## 1. Công nghệ sử dụng

- Java 17
- Spring Boot 3.3.x (Spring Web, Spring Boot Thymeleaf)
- Maven
- Jackson (đọc dữ liệu JSON)
- HTML/CSS (Thymeleaf template)

## 2. Cấu trúc project

```
saleappv1
├── docs
│   └── usecase.puml                # Sơ đồ Use Case (PlantUML)
├── src/main/java/com/example/saleappv1
│   ├── Saleappv1Application.java
│   ├── controller
│   │   ├── HomeController.java     # Trang chủ "/"
│   │   └── ProductController.java  # /products, /products/{id}
│   ├── model
│   │   ├── Product.java
│   │   └── Category.java
│   └── service
│       └── ProductService.java     # Đọc JSON, lọc & tìm kiếm sản phẩm
├── src/main/resources
│   ├── data
│   │   ├── categories.json
│   │   └── products.json
│   ├── static/css/style.css
│   ├── templates
│   │   ├── fragments.html          # Header + Footer dùng chung (Thymeleaf fragment)
│   │   ├── index.html
│   │   ├── products.html
│   │   └── product-detail.html
│   └── application.properties
├── pom.xml
├── .gitignore
└── README.md
```

## 3. Chạy ứng dụng

```bash
mvn spring-boot:run
```

Hoặc build file jar rồi chạy:

```bash
mvn clean package
java -jar target/saleappv1-0.0.1-SNAPSHOT.jar
```

Ứng dụng chạy tại: http://localhost:8080

## 4. Các chức năng đã thực hiện (theo yêu cầu bài thực hành)

| Yêu cầu | Endpoint | Mô tả |
|---|---|---|
| 2. Trang chủ | `GET /` | Tiêu đề "BÁN HÀNG TRỰC TUYẾN" + liên kết Tất cả / Điện thoại di động / Máy tính bảng |
| 3. Danh sách sản phẩm | `GET /products` | Hiển thị hình ảnh, tên, mô tả, giá của tất cả sản phẩm |
| 4. Chi tiết sản phẩm | `GET /products/{id}` | Hình ảnh, tên, mô tả, giá, danh mục |
| 5. Lọc theo danh mục | `GET /products?categoryId=1` (hoặc `2`) | 1 = Điện thoại di động, 2 = Máy tính bảng |
| 5. Tìm theo tên | `GET /products?keyword=iphone` | Tìm gần đúng, không phân biệt hoa/thường |
| 5. Tìm theo khoảng giá | `GET /products?fromPrice=5000000&toPrice=20000000` | Có thể kết hợp với categoryId/keyword. Chấp nhận cả giá có dấu chấm ngăn cách (`5.000.000`) |
| Nâng cấp | `GET /products?page=2&size=4` | Phân trang danh sách sản phẩm (mặc định 4 sản phẩm/trang) |
| Nâng cấp | `GET /` | Trang chủ hiển thị thêm khối "Sản phẩm bán chạy" (4 sản phẩm có lượt bán cao nhất) |

Các tham số `categoryId`, `keyword`, `fromPrice`, `toPrice`, `page`, `size` có thể kết hợp cùng lúc (ví dụ:
`/products?categoryId=1&keyword=samsung&toPrice=20000000&page=2`).

## 4b. Giao diện nâng cấp (phong cách trang bán lẻ)

- Header sticky: logo, ô tìm kiếm nhanh (gõ Enter tìm theo tên), hotline.
- Menu danh mục có icon (📱 Điện thoại, 💻 Máy tính bảng), tự bôi đậm mục đang chọn.
- Banner khuyến mãi ở trang chủ + khối "Sản phẩm bán chạy" (top 4 theo lượt bán).
- Trang danh sách: breadcrumb, sắp xếp (`sort=priceAsc|priceDesc|bestseller`), badge giảm giá /
  hết hàng / "Trả góp 0%" (hiển thị với sản phẩm ≥ 5.000.000đ, mang tính minh hoạ, chưa gắn với
  cổng thanh toán thật).
- Trang chi tiết: breadcrumb đầy đủ (Trang chủ / Danh mục / Tên sản phẩm), bảng thông số kỹ thuật.
- `templates/fragments.html`: chứa 2 fragment `header` và `footer` dùng chung cho cả 3 trang, tránh
  lặp code HTML (dùng `th:replace="~{fragments :: header(...)}"`).

## 5. Sơ đồ Use Case

File `docs/usecase.puml` chứa mã nguồn PlantUML cho sơ đồ Use Case của toàn hệ thống
(bao gồm cả các chức năng chưa cài đặt trong bài thực hành này như giỏ hàng, thanh toán,
báo cáo doanh thu — dành cho phần mô tả tổng quan hệ thống). Có thể xem online tại
https://www.plantuml.com/plantuml/uml/ (dán nội dung file vào) hoặc dùng plugin PlantUML
trong VS Code / IntelliJ.

## 6. Đưa project lên GitHub

```bash
cd saleappv1
git init
git add .
git commit -m "Bai thuc hanh 2: Spring Boot - saleappv1"
git branch -M main
git remote add origin https://github.com/<ten-tai-khoan>/saleappv1.git
git push -u origin main
```

Lưu ý: file `.gitignore` đã loại trừ `target/`, các file cấu hình IDE (`.idea/`, `.classpath`,
`.project`, `.settings/`...) — không cần xoá thủ công trước khi push.

## 7. Ghi chú

- Dữ liệu sản phẩm/danh mục hiện đọc từ `src/main/resources/data/*.json` bằng Jackson —
  chưa dùng Spring Data JPA/MySQL (sẽ triển khai ở bài thực hành tiếp theo theo đề bài).
- Ảnh sản phẩm đã có sẵn trong `src/main/resources/static/images/` (8 ảnh minh hoạ tạo
  sẵn, không phụ thuộc internet khi chạy). `products.json` trỏ tới các ảnh này qua đường
  dẫn tương đối, ví dụ `/images/iphone-15.jpg`.
- Muốn dùng ảnh thật của sản phẩm: chỉ cần thay file `.jpg` cùng tên trong
  `static/images/` bằng ảnh thật (giữ nguyên tên file), hoặc đổi tên file + sửa lại
  trường `image` tương ứng trong `products.json`.
