# Một số hiểu biết cơ bản về log

<a name="index"></a>
[Mục lục](#index)

- [1. Khái niệm cơ bản về log](#1)
- [2. Syslog và Rsyslog](#2)
- [3. Log tập trung](#3)
- [Tài liệu tham khảo](#4)

---
<a name="1"></a>
## 1. Khái niệm về log
*Log là gì? Log để làm gì?*

Trước hết Bạn là người quản trị mạng của một doanh nghiệp, trong hệ thống mạng của bạn có một máy chủ chứa dữ liệu rất quan trọng. Một buổi tối bạn để máy chủ đó chạy suốt đêm nhưng khi về đến nhà bạn truy cập vào máy chủ thì báo lỗi từ chối dịch vụ do không thể kết nối, buổi sáng bạn vội vã đến xem xét tình hình thì thấy một số dữ liệu đã bị mất và vấn đề lúc này là xem ai đã gây ra vấn đề trên. Vậy phải làm thế nào để điều tra xử lý, hay đơn giản là tìm nguyên nhân để khắc phục hậu quả vừa xảy ra. Log sẽ giúp bạn làm việc này.

<img src="https://lh6.googleusercontent.com/-iMgGDt5jbEE/UYzNUUHrCXI/AAAAAAAAAaA/6bpFV0uCO5s/w712-h534-no/logstack.jpg">

nguồn: https://plus.google.com/+RainerGerhards/posts

*Vậy nên tác dụng của log là:*

- Log **ghi lại liên tục** các thông báo về hoạt động của cả hệ thống hoặc của các dịch vụ được triển khai trên hệ thống và file tương ứng. Log file thường là các file văn bản thông thường dưới dạng “clear text” tức là bạn có thể dễ dàng đọc được nó, vì thế có thể sử dụng các trình soạn thảo văn bản (vi, vim, nano...) hoặc các trình xem văn bản thông thường (cat, tailf, head...) là có thể xem được file log.
- Các file log có thể nói cho bạn bất cứ thứ gì bạn cần biết, để giải quyết các rắc rối mà bạn gặp phải miễn là bạn biết ứng dụng nào, tiến trình nào được ghi vào log nào cụ thể.
- Trong hầu hết hệ thống Linux thì **`/var/log`** là nơi lưu lại tất cả các log.

Như đã nói ở trên, tác dụng của log là vô cùng to lớn, nó có thể giúp quản trị viên **theo dõi hệ thống của mình tôt hơn, hoặc giải quyết các vấn đề gặp phải** với hệ thống hoặc service. Điều này đặc biệt quan trọng với các hệ thống cần phải online 24/24 để phục vụ nhu cầu của mọi người dùng.


---
