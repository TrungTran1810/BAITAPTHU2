# laptrinhmang
Phần mềm phát video trong mạng LAN
1.Giới thiệu phầm mềm
Video để ở máy phát (chạy chương trình phát video, các máy join vào group này có thể xem video đó, việc phát này giống như đang livestream trên máy phát)
2.Chức năng
1. Đọc và chia nhỏ video
•	Sử dụng thư viện như JavaCV (dựa trên FFmpeg) để đọc video và trích xuất từng khung hình hoặc từng đoạn video nhỏ.
•	Cắt video thành các đoạn ngắn, hoặc thành từng khung hình (nếu cần) và lưu tạm vào bộ nhớ hoặc lưu tạm ra đĩa.
2. Chuyển đổi dữ liệu video thành dạng byte
•	Sau khi có các đoạn video nhỏ, bạn cần chuyển đổi chúng thành chuỗi byte (mảng byte) để có thể gửi qua UDP.
•	Dùng các lớp như ByteArrayOutputStream và DataOutputStream trong Java để hỗ trợ chuyển đổi.
3.Gửi dữ liệu qua UDP
•	Sử dụng DatagramSocket và DatagramPacket để gửi các đoạn byte qua UDP.
•	Đảm bảo thiết lập đúng kích thước gói tin để tránh vấn đề phân mảnh, thường là khoảng 1400-1500 byte mỗi gói để đảm bảo gói không bị chia nhỏ trong quá trình truyền.
•	Mỗi gói tin cần bao gồm thông tin về thứ tự gói để bên nhận có thể sắp xếp lại thành video ban đầu.
4. Bên nhận video
•	Thiết lập một DatagramSocket trên máy nhận để nhận các gói tin UDP.
•	Ghép các gói tin dựa trên thứ tự đã gửi để tạo lại video.
•	Sử dụng lại thư viện JavaCV để tạo lại video từ các đoạn nhỏ hoặc từ các khung hình.
•	Dùng 1 thư viện Player trên JavaFX để hiển thị video bên máy nhận.
2 Định nghĩa giao thức cho từng chức năng
![image](https://github.com/user-attachments/assets/6a6feb31-07de-4ad3-b5f3-74226491ac51)

![image](https://github.com/user-attachments/assets/0ef2cd48-45ae-42f6-bfc0-e3d98359e5c4)


Giao tiếp giữa Server và Client:
1. Bắt đầu một phiên nhận video:

Client gửi: <SESSION_REQ>client123</SESSION_REQ>
Server trả về: <SESSION_ACCEPTED>session987</SESSION_ACCEPTED>

2 .Nhận video:

Client gửi: <FRAME_REQ>session987</FRAME_REQ>
Server gửi: <FRAME_DATA>session987|frame1|[data]</FRAME_DATA>
Client gửi xác nhận: <FRAME_ACK>session987|frame1</FRAME_ACK>

3. Dừng nhận video:

Client gửi: <SESSION_END>session987</SESSION_END>
Server ngừng phát video cho session này.

4 .Kiểm tra trạng thái:

Client gửi: <STATUS_REQ>session987</STATUS_REQ>
Server trả: <STATUS_OK>session987|FPS:30|Resolution:1920x1080</STATUS_OK>
Video minh họa
https://github.com/user-attachments/assets/8b530b83-8cd5-43ea-a15d-3097dd679bcb

