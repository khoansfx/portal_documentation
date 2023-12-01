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


2. Xác định những nội dung cần đồng bộ và nội dung không cần đồng bộ với LMS để Portal có thể viết các API để LMS gọi vào và ngược lại.
   
   Ví dụ: 
   
   - Hiện tại Portal đã có API để khởi tạo student, update student nhưng chưa có Read và Delete
   
   - Các tác vụ như thêm course vào organization, bỏ course khỏi organization => hai bên sẽ cần phát triển API tương ứng
   
   - API thêm / bớt học viên vào organization
   
   - API tạo learning program, 
   
   - API enroll / unenroll cho business student và individual student
   
   - .... Cần suy nghĩ thêm về nghiệp vụ sẽ gặp


Quyền hạn của từng chức vụ ở Portal:

1. Super Admin: Đây là cấp độ cao nhất với quyền truy cập không giới hạn. Người giữ vai trò này có toàn quyền quản lý và điều chỉnh mọi khía cạnh của hệ thống mà không bị hạn chế bởi bất kỳ quy tắc nào.

2. Admin: Người quản trị này có quyền lực đầy đủ trong phạm vi các module mà họ phụ trách. Tùy thuộc vào lĩnh vực chuyên môn, có thể có nhiều loại Administrator như Quản trị Kế toán, Quản trị Nhân sự, và các lĩnh vực khác tương ứng.

3. Nhân viên (Staff): Đây là tầng lớp cơ sở trong tổ chức, với các chức vụ và trách nhiệm cụ thể. Ví dụ có thể bao gồm Nhân viên Chăm sóc Doanh nghiệp, Nhân viên Hỗ trợ Học viên, Nhân viên Xử lý Ticket, và các vai trò khác tùy thuộc vào nhu cầu và cấu trúc của tổ chức.

4. Mentor: Là những người chuyên trách về hỗ trợ chuyên môn và chấm các bài tập Project cho học viên. Họ có nhiệm vụ cung cấp sự hỗ trợ chuyên môn, định hướng và phản hồi cho học viên.
==> Hiện tại chưa phân chia Admin và staff cho từng lĩnh vực cụ thể 

Với những điều trên, và với 9 module chính hiện có ở Portal (sẽ được update thêm) bao gồm:
1. Student Management - Module quản lý học viên
2. Course Management - Module quản lý các khóa học
3. Learning Program - Module quản lý các chương trình học
4. Mentor Management - Module quản lý các Mentor
5. Student Organization Management - Module quản lý doanh nghiệp
6. Feedback Ticket Management - Module quản lý feedback của học viên
7. Learning Project - Module quản lý các bài Project của học viên
8. Mail Service - Module Dịch Vụ Mail
9. Service Key - Module quản lý các key nội bộ

==> Đây chỉ là overview các module chính được phát triển của Portal. Trên thực tế, các module này sẽ sử dụng các tính năng lẫn nhau và mỗi module có thể có rất nhiều tính năng, và ở Portal có thể giới hạn đến từng tính năng cụ thể và có các điều kiện kèm theo (ví dụ mentor chỉ có thể thấy được các project của các course mà họ được assigned).
**Ví dụ**: **Module Mentor Management** có sử dụng tính năng tracking **Project Submission** của học viên để mentor có thể thấy được những **Project Submission** được phân công cho họ.





