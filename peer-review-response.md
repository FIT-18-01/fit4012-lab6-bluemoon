# Peer Review Response - Lab 6 AES-CBC Socket

## Nhóm

- Nguyễn Như Thành - 1871020536
- Trần Việt Trung - 1871020537

## Phản hồi nhận xét

### 1. Về thiết kế kênh khóa

Nhận xét rằng việc gửi key và IV qua plaintext là chưa an toàn là đúng. Nhóm thống nhất đây chỉ là mô hình mô phỏng cho bài lab, không phải cách triển khai thực tế. Nếu làm phiên bản sản phẩm, nhóm sẽ dùng TLS hoặc cơ chế trao đổi khóa an toàn hơn.

### 2. Về xác thực toàn vẹn

Nhận xét rằng AES-CBC không tự bảo vệ toàn vẹn dữ liệu cũng đúng. Nhóm đã bổ sung phần threat model và kết luận để nêu rõ giới hạn này. Trong môi trường thật, nhóm sẽ ưu tiên AES-GCM hoặc thêm MAC để phát hiện sửa đổi dữ liệu.

### 3. Về log và kiểm thử

Nhận xét về việc cần có log minh chứng và test cho các tình huống sai là hợp lý. Nhóm đã giữ log chạy sender/receiver và thêm các test cho padding, key channel, data channel, wrong key, tamper và local integration.

## Kết luận

Nhóm chấp nhận các góp ý và xem đây là các điểm cần cải thiện nếu phát triển hệ thống ngoài phạm vi bài lab.
