## Tổng quan về hợp ngữ và công cụ debug OllyDBG

> Tài liệu tham khảo: Internet
>
> Thực hiện: **Nguyễn Văn Thành**
>
> Cập nhập lần cuối: **18/02/2016**

### Mục lục

[1. Nghiên cứu tổng quan về hợp ngữ(Assembly) dùng trong Reverse Engineering.](#1)

 - [1.1. Thế nào là dịch ngược ?](#1.1)

 - [1.2. Dịch ngược nhằm mục đích gì ?](#1.2)

 - [1.3. Các vấn đề chính của RE](#1.3)

 - [1.4. Ứng dụng của RE](#1.3)

 - [1.5. Các kiến thức cần thiết ?](#1.4)

 - [1.6. Một số công cụ thường sử dụng](#1.5)

[2. Cài đặt và tìm hiểu về công cụ debug GUI OllyDBG](#2)

[3. Thực hành theo hướng dẫn của Tutorial](#3)

------------------------------------------------------------------------

#### 1. Nghiên cứu tổng quan về hợp ngữ(Assembly) dùng trong Reverse Engineering. <a name= "1"></a>
##### 1.1. Thế nào là dịch ngược? <a name ="1.1"></a>
 
 - Dịch ngược là quá trình cố gắng tìm hiểu cách thức chương trình làm việc dựa vào file đã biên dịch. Ban đầu lập trình viên viết code bằng ngôn ngữ lập trình bậc cao như  C, C++, Visual Basic … Sau đó chúng được dịch ra mã máy để giúp máy tính có thể hiểu được. Nhưng đối với con người thì mã máy thật sự khó hiểu, vì thế dịch ngược giúp ta hiểu được chương trình cần làm gì mà không có file code ban đầu.

##### 1.2. Dịch ngược nhằm mục đích gì ?<a name="1.2"></a>

 - Dịch ngược được sử dụng ở rất nhiều mảng trong khoa học máy tính nhưng sau đây là những mảng chính:
 <ul>
<li>Có được trong tay ý tưởng code của tác giả</li>
<li>Phá vỡ cơ chế bảo vệ phần mềm ( khè bạn bè hay kiếm $$$ chẳng hạn)</li>
<li>Học tập nghiên cứu virus hay mã độc</li>
<li>Kiểm tra chất lượng phần mềm</li>
<li>Bổ sung thêm tính năng vào chương trình</li>
 </ul>

 - Ở mảng **đầu tiên**, công việc là tái tạo lại file code ban đầu dựa vào file nhị phân. 

 - Ở mảng **thứ hai**, chúng ta phá vỡ cơ chế bảo vệ của chương trình. Điều đó có nghĩa là chúng ta gỡ bỏ tính năng thời gian dùng thử, đăng kí sử dụng hay tất cả những thứ mà những chương trình thương mại làm để bắt người dùng phải trả tiền. Đây sẽ là phần dài nhất.

 - Ở mảng **thứ ba**, là nghiên cứu virus và mã độc. Việc dịch ngược là cần thiết vì bên ngoài có rất nhiều người viết virus và họ sẽ chẳng bao giờ để lộ cách họ viết ra virus, mục tiêu của virus hay cách mà virus đạt được mục tiêu của mình (trừ khi họ bị ngu). Đây thực sự là mảng thú vị, nhưng nó lại yêu cầu lượng kiến thức lớn.

 - Mảng **thứ tư** là kiểm tra thẩm định tính an toàn cũng như lỗ hổng của phần mềm. Khi mà phải làm việc với một chương trình lớn (ví dụ như hệ điều hành Windows) thì việc dịch ngược giúp đảm bảo không có những lỗ hổng nghiêm trọng hay làm khó các **cracker** cố gắng crack phần mềm.

 - Mảng **cuối cùng** là thêm những tính năng mới vào phần mềm. Theo ý kiến riêng, tôi nghĩ đây là một mảng khá là vui. Kiểu như bạn không thích bức ảnh trong phần mềm, hãy thay đổi nó. Hay như thêm tính năng mã hóa nội dung văn bản vào chương trình soạn thảo. Thậm chí là trêu tức nhân viên bằng các hộp thoại khi họ đang làm việc với chương trình Windows calculator.

#####1.3. Các vấn đề chính của RE<a name="1.3"></a>

 - Xét trong vấn đề nhỏ được bàn bạc trong bài viết này là RE phần mềm. Với mỗi một nền tảng, hệ điều hành khác nhau ta có các chương trình được xây dựng khác nhau, thực thi theo cách khác nhau. Điều này tạo ra sự phức tạp trong quá trình thực hiện công việc RE.

 - Với đặc trưng của mỗi nền tảng, ta có những hướng khác nhau để tiếp cận, những công cụ khác nhau để trợ giúp cho công việc.

 - Với các chương trình trên Window ta sử dụng những công cụ khác, các chương trình trên Linux ta dùng công cụ khác.

 - Những chương trình chạy trên nền tảng .Net Framework và các chương trình được viết bằng python sẽ có những công cụ RE khác nhau.

#####1.4. Ứng dụng của RE<a name="1.3"></a>
 
 -  Có thể kể ra một số công việc cần sử dụng đến RE như Phân tích Malware (viruses, trojans, worms...) để biết được chúng lây lan ra sao, lấy cắp thông tin cá nhân như thế nào? Để xóa chúng khỏi hệ thống, hay điều tra thông tin về người phát tán, cần thực hiện những hành động điều tra gì?

 - Ngoài ra một ứng dụng khác của RE, tương tự như ví dụ đưa ra ở đầu bài viết, đó là khi bạn có một phần mềm thương mại, cần phải trả phí để sử dụng mà bạn không đủ khả năng để chi trả hoặc thậm chí là không muốn trả khoản phí đó. Bạn có thể RE để tìm hiểu cách thức hoạt động của module thực hiện công việc bạn cần quan tâm để viết lại 1 chương trình khác thực hiện công việc đó, hay tìm hiểu xem module kiểm tra key của phần mềm đó hoạt động như thế nào.

 - Bên cạnh đó RE còn là một sở thích, thỏa mãn tính tò mò của bạn về cách thức hoạt động của một hệ thống nào đó.

#####1.6. Một số công cụ thường sử dụng<a name="1.6"></a>

######1. OllyDBG

 - OllyDBG hay còn gọi tắt là **Olly** là công cụ **debug** rất phổ biến. Nhờ giao diện trực quan và dễ sử dụng nên **Olly** phù hợp với người dùng ở mọi trình độ khác nhau.


######2. .Net Reflector

 - Nếu như ta có Olly để debug các chương trình window 32-bit thì các chương trình chạy trên nền tảng **.NET Framework** ta có công cụ **.Net Reflector**.

 - Trong bài viết tiếp theo tôi sẽ giới thiệu về tính năng và cách sử dụng của **.Net Refector**.

######3. IDA

 - IDA là công cụ disassembler và debuger đa nền tảng, có thể sử dụng trên nền tảng Windows, Linux or Mac OS X. Đây là tính năng ưu việt hơn hẳn so với Olly và .Net Reflector- chỉ sử dụng cho 1 nền tảng nhất định.

######4. Hex Editor

 - Trong quá trình thực hiện công việc **Reverse**, ta cần đến các công cụ **Hex Editor** để đọc và chỉnh sửa các file dưới định dạng hex.

 - Có rất nhiều Hex Editor, mỗi người thường lựa chọn 1 công cụ phù hợp với điều kiện và sử dụng quen tay.

######5. Disassemblers

 - **Disassemblers** sẽ cố hiển thị ngôn ngữ máy trong các file binary một cách dễ hiểu. Chúng cũng tìm ra và hiển thị những hàm, tham số truyền vào, string… Tất cả những điều đó khiến việc đọc mã trở nên dễ dàng với con người hơn. Có rất nhiều **disassemble**, một số loại dành cho những thứ riêng biệt (ví dụ: Binary được viết bởi Delphi). Rồi bạn sẽ thoải mái làm việc với chúng. Bản thân tôi thấy làm việc với IDA ( bản miễn phí ở đấy  http://www.hex-rays.com ) 

######6. Debuggers

 - Trình gỡ rối  (debugger) là thứ tối quan trọng với người dịch ngược. Đầu tiên chúng phân tích binary giống như **Disassembler**. Trình gỡ rối cho phép chương trình chạy từng dòng code, chạy từng dòn một và hiển thị kết quả. Đây là chìa khóa để khám phá ra chương trình hoạt động như thế nào. Cuối cùng, một số trình gỡ rối cho phép chỉnh sửa mã trực tiếp. 

######7. PE and resource viewers/editors

 - Tất cả những binary được thiết kế để chạy trên máy tính Windows ( hay cả Linux ) đều có những phần dữ liệu cụ thể ở đầu mỗi file. Chúng giúp cho hệ thống khở tạo chạy chương trình. Chúng cho OS biết dung lượng bộ nhớ mà chúng cần, những **DLL** mà chương trình cần. Chúng được gọi là **Portable Executable**, tất cả chương trình chạy trên Windows đều phải dùng loại này.

 - Trong môi trường dịch ngược, cấu trúc sắp xếp các bytes là điều rất quan trọng, nó giúp người dịch ngược có thêm thông tin cần thiết. Thậm chí nhiều khi bạn muốn thay đổi những thông tin này để làm cho chương trình chạy khác so với ban đầu hoặc thay đổi nó như chương trình ban đầu. Hiện nay có rất nhiều phần mềm xem và chỉnh sửa PE.

 - Hầu hết các file đều có phần “resource”, nơi chứa hình ảnh, item trong dialog, icon, text… Thỉnh thoảng bạn có thể chỉnh sửa những thứ này.

######8. System Monitoring tools

 - Khi bạn dịch ngược thỉnh thoảng những công cụ này sẽ phát huy tác dụng (rõ ràng nhất là khi nghiên cứu virus và malware), chúng hiển thị sự thay đổi mà chương trình gây ra đối với hệ thống, có thẻ là **key** trong **registry** được tạo ra hay được truy xuất, hay file **.ini** được tạo, hay những tiến trình ẩn được khởi tạo. Một số công cụ được biết đến như là **procmon**, **regshot**, and **process hacker**. Chúng ta sẽ bàn luận sau.

######9. Miscellaneous tools and information

 - Đây là những công cụ được sưu tầm trong quá trình làm việc như  scripts, unpackers, packer identifiers … Windows API cũng là một phần thông tin quan trọng. Nó cực kì hữu ích, cho chép chúng ta biết hàm API đó có tác dụng gì.

######Có một số công cụ đưa ra để gợi ý cho mọi người như : 010 Hex Editor (phần mềm trả phí), Winhex (phần mềm miễn phí), CFF Explorer VIII (1 chương trình nằm trong bộ công cụ Explorer Suite)

####2. Cài đặt và tìm hiểu về công cụ debug GUI OllyDBG<a name="2"></a>

 - OllyDBG hay còn gọi tắt là Olly là công cụ debug rất phổ biến. Nhờ giao diện trực quan và dễ sử dụng nên Olly phù hợp với người dùng ở mọi trình độ khác nhau.

Mọi người có thể download lại địa chỉ: http://www.ollydbg.de/

![WHITEHAT](http://forum.whitehat.vn/attachment.php?attachmentid=965&stc=1)

######Cửa sổ OllyDBG được chia ra làm 5 cửa sổ con :

1: Disassembler window: Các đoạn mã của chương trình dưới dạng code assembly và các comment tại các dòng code đó.
2: Register window: Các thanh ghi và giá trị của chúng.
3: Tip window: Các thông tin bổ sung cho 1 dòng code. Các thông tin này khá hữu ích trong quá trình debug.
4: Dump window: Cho phép người sử dụng xem và chỉnh sửa các giá trị trong bộ nhớ của chương trình đang được debug.
5: Stack window: Thông tin về stack của chương trình.

######Ngoài 5 cửa sổ trên thì Olly còn có một số cửa sổ khác. Để ý trên thanh menu có các chức năng:

![WHITEHAT](http://forum.whitehat.vn/attachment.php?attachmentid=966&stc=1)

 - Click vào nút **L** ta thấy cửa số **Log data** của Olly. Chứa các thông tin về các module, các import library và các plugins được load cùng chương trình tại thời điểm chương trình được load vào Olly.

 - Click vào nút **E** ta thấy cửa sổ **Executable** modules, danh sách các file thực thi được chương trình sử dụng.

 - Click vào nút **M** ta thấy cửa sổ **Memory Map**, chứa thông tin về bộ nhớ được chương trình của ta sử dụng.

 - Click vào nút **T** là cửa sổ **Threads**, liệt kê các thread của chương trình.

 - Click vào nút **W** là **Windows**.

 - Nút **H** là cửa sổ **Handles**.

 - Nút **/** là **Patches**, cửa sổ chứa các thông tin về những câu lệnh ta đã sửa ở trong chương trình.

 - Nút **K** là cửa sổ **Call Stack**.

 - Nút **B** là cửa sổ **Breakpoints**, hiển thị các **breakpoint** ta đặt trong chương trình.

 - Nút **R** - **References** chứa thông tin về kết quả cho chức năng tìm kiếm trong Olly.

######Những chức năng không thể thiếu đó là các chức năng phục vụ cho công việc debug: 

![WHITEHAT](http://forum.whitehat.vn/attachment.php?attachmentid=967&stc=1)

 - Đặt Breakpoint: **F2**

 - Run: **F9**

 - Step into: **F7**

 - Step over: **F8**

 - Restart: **Ctrl + F2**

