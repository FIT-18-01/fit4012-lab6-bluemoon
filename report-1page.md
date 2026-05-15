# Report 1 page - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Nguyễn Như Thành - 1871020536
- Thành viên 2: Trần Việt Trung - 1871020537

## Mục tiêu

Mục tiêu của bài lab là xây dựng một hệ thống gửi và nhận dữ liệu qua TCP socket với cơ chế mã hóa AES-CBC.  
Nhóm tách luồng truyền thành hai kênh: kênh khóa để gửi key và IV, và kênh dữ liệu để gửi ciphertext.  
Sender tạo plaintext từ chuỗi nhập tay hoặc từ file, mã hóa bằng AES-CBC với PKCS#7 padding, rồi gửi sang Receiver.  
Receiver nhận gói tin, kiểm tra header độ dài, giải mã ciphertext và ghi output ra file hoặc màn hình.  
Qua bài này, nhóm cũng kiểm thử các trường hợp đúng và sai để thấy rõ giới hạn bảo mật của việc gửi key/IV ở dạng plaintext.

## Phân công thực hiện

Nguyễn Như Thành phụ trách chính phần Sender, tạo packet, log gửi và chạy demo luồng gửi.  
Trần Việt Trung phụ trách chính phần Receiver, nhận packet, giải mã và ghi kết quả đầu ra.  
Cả hai cùng xây dựng `aes_socket_utils.py`, viết test, hoàn thiện log minh chứng, báo cáo và threat model.

## Cách làm

Nhóm dùng thư viện `Crypto.Cipher.AES` ở chế độ CBC với key dài 16 hoặc 32 byte và IV dài 16 byte.  
Dữ liệu trước khi mã hóa được padding theo PKCS#7 để luôn chia hết cho block size 16 byte.  
Key channel đóng gói theo định dạng `[key_length][key][iv]`, còn data channel theo `[ciphertext_length][ciphertext]`.  
Receiver đọc đủ số byte bằng hàm nhận chính xác độ dài, sau đó parse header và kiểm tra tính hợp lệ của packet.  
Mô hình này giúp tách trách nhiệm giữa việc truyền khóa và việc truyền dữ liệu mã hóa.

## Kết quả

Kết quả chạy local cho thấy Sender gửi thành công key/IV và ciphertext, Receiver giải mã đúng bản tin gốc.  
Nhóm lưu log gửi và log nhận để làm minh chứng cho luồng hoạt động của hệ thống.  
Các test quan trọng gồm roundtrip local, padding/unpadding, key packet, data packet, wrong key và tamper.  
Các test âm tính cho thấy hệ thống không có xác thực toàn vẹn mạnh, nên ciphertext bị sửa có thể gây lỗi hoặc cho plaintext sai.

## Kết luận

Kỹ thuật quan trọng nhất là phải chuẩn hóa protocol bằng header độ dài và kiểm tra đầy đủ dữ liệu nhận được.  
Về bảo mật, AES-CBC chỉ bảo vệ tính bí mật của nội dung, không tự cung cấp xác thực hay chống sửa đổi.  
Việc gửi key và IV qua plaintext chỉ phù hợp cho mô phỏng học tập, không phù hợp với hệ thống thật.  
Muốn triển khai an toàn hơn cần cơ chế trao đổi khóa an toàn và cơ chế xác thực dữ liệu.
