# Threat Model - Lab 6 AES-CBC Socket

## Thông tin nhóm

- Thành viên 1: Nguyễn Như Thành - 1871020536
- Thành viên 2: Trần Việt Trung - 1871020537

## Assets

Tài sản cần bảo vệ gồm plaintext, AES key, IV, ciphertext, file input, file output và các file log.  
Trong môi trường demo, key và IV vẫn là dữ liệu nhạy cảm vì chúng cho phép giải mã toàn bộ nội dung đã mã hóa.  
Log cũng cần xem xét vì có thể chứa thông tin gỡ lỗi, metadata hoặc bản tin gốc.

## Attacker model

Đối tượng tấn công giả định có thể nghe lén trong mạng LAN, chặn hoặc ghi lại gói tin TCP, sửa ciphertext, phát lại packet cũ hoặc đọc file log nếu có quyền truy cập hệ thống.  
Kẻ tấn công không cần kiểm soát máy chủ, chỉ cần nhìn thấy luồng mạng hoặc file lưu trữ là đã có thể khai thác một phần thông tin.

## Threats

Các mối đe dọa chính gồm:
- Key disclosure do key/IV được gửi plaintext trên kênh khóa.
- Tampering do ciphertext bị sửa trên kênh dữ liệu.
- Replay attack do packet cũ có thể được phát lại mà hệ thống chưa kiểm tra nonce hoặc timestamp.
- Log leakage do key, IV hoặc bản tin gốc có thể xuất hiện trong file log.
- No authentication do Receiver chưa xác thực danh tính Sender.

## Mitigations

Các biện pháp giảm thiểu gồm:
- Không gửi key plaintext trong hệ thống thật.
- Dùng TLS hoặc cơ chế trao đổi khóa an toàn như Diffie-Hellman hoặc public key infrastructure.
- Dùng AES-GCM thay cho AES-CBC nếu cần cả bí mật và toàn vẹn.
- Không ghi key thật vào log trong môi trường thật.
- Thêm nonce hoặc timestamp để giảm replay.
- Thêm xác thực Sender bằng chữ ký số, MAC hoặc cơ chế đăng nhập.

## Residual risks

Rủi ro còn lại là hệ thống vẫn chỉ là mô phỏng học tập. Key channel chưa được bảo vệ, chưa có TLS, chưa có xác thực thực thể và chưa chống replay đầy đủ.  
Vì vậy hệ thống chỉ phù hợp cho demo trong lớp hoặc trong máy cá nhân, không thể xem là an toàn để triển khai thực tế.
