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
Lấy 191:8 = 23.875
-> Facility = 23 ("local 7")
-> Severity = 191 - (23 * 8 ) = 7 (debug)


`HEADER`

Phần `HEADER` thì gồm các phần chính sau:
- Time stamp - Thời gian mà thông báo được tạo ra. Thời gian này được lấy từ thời gian hệ thống ( Chú ý nếu như thời gian của server và thời gian của client khác nhau thì thông báo ghi trên log được gửi lên server là thời gian của máy client)
- Hostname hoặc IP


`MSG`

Phần `Message` hay `MSG` chứa một số thông tin về quá trình tạo ra thông điệp đó. Gồm 2 phần chính:
- Tag field
- Content field

*Tag field* là tên chương trình tạo ra thông báo. *Content field* chứa các chi tiết của thông báo




**2.2 Rsyslog**

Rsyslog - "The rocket-fast system for log processing" được bắt đầu phát triển từ năm 2004 bởi Rainer Gerhards rsyslog là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay rsyslog là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như : Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…

Twitter của tác giả Rsyslog [Twitter](https://twitter.com/rgerhards/)

---
<a name="3"></a>
## 3. Log tập trung

**Tác dụng của log là vô cùng to lớn vậy làm thế nào để quản lý log tốt hơn?**

Để quản lý log một cách tốt hơn, xu thế hiện nay sẽ sử dụng **log tập trun**g. Vậy log tập trung là gì? Tác dụng của nó thế nào?

Hiểu một cách đơn giản : Log tâp trung là quá trình tập trung, thu thập, phân tích... các log cần thiết từ nhiều nguồn khác nhau về một nơi an toàn để thuận lợi cho việc phân tích, theo dõi hệ thống.

**Tại sao lại phải sử dụng log tập trung?**

- Do có nhiều nguồn sinh log
  * Có nhiều nguồn sinh ra log, log nằm trên nhiều máy chủ khác nhau nên khó quản lý.
  * Nội dung log không đồng nhất (Giả sử log từ nguồn 1 có có ghi thông tin về ip mà không ghi thông tin về user name đăng nhập mà log từ nguồn 2 lại có) -> khó khăn trong việc kết hợp các log với nhau để xử lý vấn đề gặp phải.
  * Định dạng log cũng không đồng nhất -> khó khăn trong việc chuẩn hóa

- Đảm bảo tính toàn vẹn, bí mật, sẵn sàng của log.
  * Do có nhiều các rootkit được thiết kế để xóa bỏ logs.
  * Do log mới được ghi đè lên log cũ
-> Log phải được lưu trữ ở một nơi an toàn và phải có kênh truyền đủ đảm bảo tính an toàn và sẵn sàng sử dụng  để phân tích hệ thống.

**Do đó lợi ích của log tập trung đem lại là**
- Giúp quản trị viên có cái nhìn chi tiết về hệ thống -> có định hướng tốt hơn về hướng giải quyết
- Mọi hoạt động của hệ thống được ghi lại và lưu trữ ở một nơi an toàn (log server)  -> đảm bảo tính toàn vẹn phục vụ cho quá trình phân tích điều tra các cuộc tấn công vào hệ thống
- Log tập trung kết hợp với các ứng dụng thu thập và phân tích log khác nữa giúp cho việc phân tích log trở nên thuận lợi hơn -> giảm thiểu nguồn nhân lực.
</ul>


---
<a name="4"></a>
## 4. Tham khảo

- [http://en.wikipedia.org/wiki/Syslog](http://en.wikipedia.org/wiki/Syslog)
- [http://en.wikipedia.org/wiki/Rsyslog](http://en.wikipedia.org/wiki/Rsyslog)
- [https://phulc.wordpress.com/tag/su-can-thiet-cua-he-thong-quan-ly-log-tap-trung](https://phulc.wordpress.com/tag/su-can-thiet-cua-he-thong-quan-ly-log-tap-trung/)

[1]: https://tools.ietf.org/html/rfc5424
[2]: https://www.ietf.org/rfc/rfc3195.txt
[3]: https://tools.ietf.org/html/rfc6587
