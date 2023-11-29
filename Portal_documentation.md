# Mục lục:

- Đây là tài liệu nội bộ. Các danh mục trình bày trong tài liệu này bao gồm:
1. [Giới thiệu Portal](./odoo/1.Portal_documentation.md):
   
   - Danh sách module trong Portal và chức năng.
   - Sơ đồ quan hệ giữa các module và database schema

2. [Sơ đồ quy trình của Portal](./odoo/2.Business_flow_portal.md):
   
   - Danh sách các nghiệp vụ cơ bản giữa LMS và Portal
   - Sơ đồ quy trình hoạt động của các nghiệp vụ

3. [Giới thiệu Odoo](./odoo/3.Odoo_technical.md):
   
   - Giới thiệu Odoo
     - Cơ chế hoạt động cơ bản của Odoo
     - Khởi tạo một module
     - Giải thích về ORM và các abstraction của Odoo

4. [Tài liệu kỹ thuật Portal](./odoo/4.Portal_techincal.md)
   
   - Hướng dẫn cài đặt Odoo và các module cho Portal
   - Hướng dẫn setup môi trường dev cho LMS và Portal ở Window (không Docker)
     - Thông tin chi tiết về module của Portal
     - Các kiến thức kỹ thuật cần lưu ý của các module của Portal dành cho developer

5. [Tài liệu kỹ thuật chuyên sâu Odoo](./odoo/5.advanced_odoo): (sẽ update liên tục)
   
   - Các built-in Python method quan trọng của Odoo
   - Các kỹ thuật hữu ích cho việc phát triển và customize Module Odoo
   - UI/UX của Odoo:
     - QWeb template engine của Odoo
     - OWL (Odoo Web Library) framework của Odoo (pending)

## Những vấn đề cần giải quyết

1. Làm lại bảng phân quyền, quy tắc phân quyền: Phân quyền truy cập theo từng module cho từng tài khoản hay phân quyền theo từng chức vụ của nhân viên
   
   Ví dụ:
   
   - **Phân quyền truy cập theo module cho từng tài khoản:** Mỗi tài khoản Odoo sẽ chỉ được truy cập vào các module mà họ phụ trách, admin muốn tài khoản đó truy cập vào module nào thì sẽ tick vào module đó
   
   - **Phân quyền theo chức vụ của nhân viên**: sẽ phải phân định ra doanh nghiệp có những chức vụ gì (ví dụ: Super Admin, Admin, Mentor, Staff) => Mỗi chức vụ sẽ được set mặc định quyền truy cập vào các module có sẵn => Mỗi account chỉ cần tick vào chức vụ của mình là sẽ có quyền truy cập vào tất cả module đã set cho chức vụ đó.

2. Do hiện tại Portal đã integrate các API của LMS và để chạy được 2 ứng dụng cho cả 2 máy rất resource-intensive 
   
   => Develop một server nodejs nhẹ để mock test API (hoàn thành).
   
   => Cần tài liệu API specification của LMS để có thể mock response cho server nodejs

3. Xác định những nội dung cần đồng bộ và nội dung không cần đồng bộ với LMS để Portal có thể viết các API để LMS gọi vào và ngược lại.
   
   Ví dụ: 
   
   - Hiện tại Portal đã có API để khởi tạo student, update student nhưng chưa có Read và Delete
   
   - Các tác vụ như thêm course vào organization, bỏ course khỏi organization => hai bên sẽ cần phát triển API tương ứng
   
   - API thêm / bớt học viên vào organization
   
   - API tạo learning program, 
   
   - API enroll / unenroll cho business student và individual student
   
   - .... Cần suy nghĩ thêm về nghiệp vụ sẽ gặp

4. Hiện tại api-key của Sendgrid đang được hard code ở môi trường local => Cần phải store api-key này ở nơi khác