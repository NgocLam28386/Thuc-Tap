# **Bài Tập Xây Dựng Ứng Dụng Mua Hàng Online**
# Yêu Cầu Chức Năng và Phi Chức Năng 
## Yêu Cầu Chức Năng 
### Phía Khách Hàng 
#### 1.Đăng ký/Đăng nhập:

-Đăng ký tài khoản mới với email và mật khẩu.
-Đăng nhập bằng email và mật khẩu.
-Khôi phục mật khẩu qua email.
#### 2.Quản lý thông tin cá nhân:

-Xem và cập nhật thông tin cá nhân (tên, địa chỉ, số điện thoại, email).
-Thay đổi mật khẩu.
#### 3.Mua hàng: 

-Duyệt qua danh sách sản phẩm theo danh mục.
-Xem chi tiết sản phẩm (mô tả, giá, hình ảnh, đánh giá).
-Thêm sản phẩm vào giỏ hàng.
-Xem giỏ hàng và cập nhật số lượng sản phẩm.
-Đặt hàng và chọn phương thức thanh toán (thẻ tín dụng, ví điện tử, COD).
#### 4.Hỗ trợ Chatbot: 

-Chatbot cung cấp thông tin sản phẩm và hỗ trợ khách hàng trong quá trình mua sắm.
#### 5.Xem blog: 

-Đọc các bài viết trong phần blog để biết thêm chi tiết sản phẩm.
### Phía Quản Trị 
#### 1.Đăng nhập: 

-Đăng nhập bằng email và mật khẩu quản trị viên.
#### 2.Quản lý tài khoản và phân quyền: 

-Thêm, xóa, cập nhật tài khoản người dùng.
-Phân quyền người dùng (quản trị viên, người dùng thường).
#### 3.Quản lý sản phẩm và danh mục sản phẩm: 

-Thêm mới, cập nhật hoặc xóa sản phẩm và danh mục sản phẩm.
#### 4.Quản lý đơn hàng: 

-Xem danh sách đơn hàng.
-Cập nhật trạng thái đơn hàng (đang xử lý, đã giao, đã hủy).
#### 5.Báo cáo thống kê: 

-Xem báo cáo doanh thu, doanh thu theo thời gian (ngày, tuần, tháng).
## Yêu Cầu Phi Chức Năng 
#### 1.Bảo mật: 

-Dữ liệu cá nhân và thông tin thanh toán phải được mã hóa.
-Sử dụng các biện pháp bảo mật để ngăn chặn tấn công XSS, CSRF, SQL Injection.
#### 2.Hiệu suất: 

-Hệ thống phải xử lý nhanh các yêu cầu của người dùng, thời gian phản hồi không quá 2 giây.
-Khả năng mở rộng để phục vụ đồng thời nhiều người dùng.
#### 3.Khả năng mở rộng: 

-Hệ thống phải dễ dàng mở rộng thêm các tính năng mới trong tương lai.
#### 4.Tính ổn định và độ tin cậy: 

-Hệ thống phải hoạt động liên tục, thời gian hoạt động tối thiểu 99,9%.
#### 5.Dễ sử dụng: 

-Giao diện người dùng thân thiện, dễ dàng điều hướng.
-Hướng dẫn sử dụng rõ ràng cho cả người dùng và quản trị viên.
#### 6.Khả năng tương thích: 

-Ứng dụng phải tương thích với các trình duyệt web phổ biến và các thiết bị di động.
## Thiết Kế Cơ Sở Dữ Liệu và Thiết Lập Mối Quan Hệ 
### Các Bảng Chính 
#### 1.Users (Người dùng): 

-user_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-username (VARCHAR(50), UNIQUE)
-password (VARCHAR(255))
-email (VARCHAR(100), UNIQUE)
-phone (VARCHAR(15))
-address (TEXT)
-role (ENUM('customer', 'admin'))
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
#### 2.Products (Sản phẩm): 

-product_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-name (VARCHAR(100))
-description (TEXT)
-price (DECIMAL(10, 2))
-stock (INT)
-category_id (INT, FOREIGN KEY REFERENCES Categories(category_id))
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
#### 3.Categories (Danh mục): 

-category_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-name (VARCHAR(50))
-description (TEXT)
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
#### 4.Orders (Đơn hàng): 

-order_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-user_id (INT, FOREIGN KEY REFERENCES Users(user_id))
-total_amount (DECIMAL(10, 2))
-status (ENUM('processing', 'shipped', 'delivered', 'cancelled'))
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
#### 5.OrderItems (Chi tiết đơn hàng): 

-order_item_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-order_id (INT, FOREIGN KEY REFERENCES Orders(order_id))
-product_id (INT, FOREIGN KEY REFERENCES Products(product_id))
-quantity (INT)
-price (DECIMAL(10, 2))
#### 6.Blogs (Bài viết blog): 

-blog_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-title (VARCHAR(100))
-content (TEXT)
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
#### 7.Chats (Tin nhắn): 

-chat_id (INT, PRIMARY KEY, AUTO_INCREMENT)
-user_id (INT, FOREIGN KEY REFERENCES Users(user_id))
-message (TEXT)
-created_at (TIMESTAMP)
-updated_at (TIMESTAMP)
### Thiết Lập Mối Quan Hệ 
-Một người dùng có thể có nhiều đơn hàng (1-n).
-Một đơn hàng có thể có nhiều chi tiết đơn hàng (1-n).
-Một sản phẩm có thể nằm trong nhiều chi tiết đơn hàng (1-n).
-Một danh mục có thể có nhiều sản phẩm (1-n).
-Một người dùng có thể viết nhiều cuộc trò chuyện (1-n).
## Công Nghệ và Công Cụ Sử Dụng 
-Ngôn ngữ: Python, JavaScript
-Framework: Django (Backend), React (Frontend)
-Cơ sở dữ liệu: PostgreSQL
-ác thực: JWT (JSON Web Tokens)
-Hosting: AWS hoặc Heroku
-Giao diện người dùng: Material-UI (React), HTML/CSS
-Công cụ khác: Docker (Containerization), Git/GitHub (Quản lý mã nguồn), WebSocket (Chat bot thời gian thực)
## Mô Tả Chi Tiết Từng Tính Năng 
### Phía Khách Hàng 
#### Đăng nhập/Đăng ký: 
-Sử dụng biểu mẫu để đăng ký hoặc đăng nhập tài khoản.
-Khôi phục mật khẩu qua email nếu quên mật khẩu.
#### Quản lý thông tin cá nhân: 
-Truy cập trang hồ sơ cá nhân để cập nhật thông tin như tên, địa chỉ, số điện thoại, email.
-Đổi mật khẩu tài khoản.
#### Duyệt sản phẩm: 
-Hiển thị danh sách sản phẩm.
-Sử dụng bộ lọc để tìm kiếm sản phẩm theo danh mục.
-Xem chi tiết sản phẩm bao gồm mô tả, giá, hình ảnh và đánh giá.
#### Mua hàng/Đặt hàng: 
-Thêm sản phẩm vào giỏ hàng.
-Xem và chỉnh sửa giỏ hàng.
-Xác nhận đặt hàng và thanh toán qua các phương thức như thẻ tín dụng, ví điện tử, hoặc COD.
#### Chat bot: 
-Gửi tin nhắn tới chat bot để hỏi thông tin về sản phẩm.
#### Xem blog: 
-Đọc các bài viết giới thiệu sản phẩm và các chương trình ưu đãi.
### Phía Quản Trị 
#### Đăng nhập: 
-Quản trị viên đăng nhập để truy cập trang quản trị.
#### Quản lý tài khoản và phân quyền: 
-Tạo mới, chỉnh sửa hoặc xóa người dùng tài khoản.
-Phân quyền sử dụng cho từng tài khoản.
#### Quản lý sản phẩm và danh mục sản phẩm:
-Thêm mới, cập nhật hoặc xóa sản phẩm và danh mục sản phẩm.
#### Quản lý đơn hàng: 
-Xem danh sách đơn hàng.
-Cập nhật trạng thái đơn hàng.
#### Báo cáo thống kê: 
-Xem báo cáo doanh thu và doanh thu bán hàng theo thời gian.