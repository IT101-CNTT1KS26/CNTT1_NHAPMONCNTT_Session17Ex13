

### Phần A – Thiết lập Profile và Bookmark chuẩn Dev

1. **Xung đột khi dùng chung 1 Profile cho cả GitHub cá nhân và GitHub công ty:**
* **Hiện tượng:** Sẽ xảy ra **Session Conflict (Xung đột phiên đăng nhập)** và trộn lẫn các token xác thực (`Cookies` / `Tokens`). Trình duyệt lưu chung thông tin phiên làm việc của cả hai tài khoản GitHub trên cùng một không gian lưu trữ. Khi bạn thao tác git hoặc truy cập vào nền tảng, hệ thống có thể bị nhầm lẫn tài khoản, dẫn đến việc commit nhầm code công ty bằng tài khoản cá nhân (hoặc ngược lại), hoặc liên tục bị văng đăng xuất (Logout) do cookie bị ghi đè lẫn nhau.


2. **Mô hình 3 Profile chuẩn Developer:**

|  | Tên đặt Profile | Mục đích sử dụng |
| --- | --- | --- |
| **1** | `Personal` | Dùng cho mục đích cá nhân, giải trí, mạng xã hội, và tài khoản GitHub cá nhân. |
| **2** | `Company_Work` | Dùng cho công việc tại công ty: tài khoản GitHub công ty, email công ty, Jira, Slack, công cụ nội bộ. |
| **3** | `Sandbox/Test` | Dùng để kiểm thử ứng dụng, test tính năng, mở trang web lạ hoặc đăng nhập các tài khoản phụ. |

3. **Cấu trúc Bookmark chuẩn Dev:**
* `01_Dev_Docs`
* MDN Web Docs (`[https://developer.mozilla.org](https://developer.mozilla.org)`)
* React Documentation (`[https://react.dev](https://react.dev)`)


* `02_Work_Tools`
* GitHub Company (`[https://github.com/company](https://github.com/company)`)
* Jira Board (`[https://company.atlassian.net](https://company.atlassian.net)`)


*  `03_Testing_Monitoring`
* Sentry (`[https://sentry.io](https://sentry.io)`)
* Postman Web (`[https://go.postman.co](https://go.postman.co)`)




4. **Cho đồng nghiệp mượn máy 5 phút:**
* Sử dụng **Guest Profile (Khách)** hoặc mở một **Sandbox Profile**. Chế độ *Guest* hoàn toàn cách ly, không lưu lại lịch sử duyệt web, cookie hay dữ liệu cá nhân của bạn, và sẽ tự động xóa sạch toàn bộ phiên làm việc ngay khi đóng cửa sổ.



---

### Phần B – Chọn đúng Extension hỗ trợ

1. **Xử lý đường link API JSON thô:**
* **Nhóm Extension:** Thuộc nhóm **Developer Suite**.
* **Tên tiện ích cụ thể:** **JSON Formatter** hoặc **JSON Viewer**.
* **Vì sao 2 nhóm còn lại không phù hợp:**
* *Productivity Suite* (như quản lý tab, ghi chú) chỉ hỗ trợ tăng năng suất chung chứ không có khả năng phân tích cú pháp (parse) và làm đẹp cấu trúc JSON thô.
* *Security Suite* (như trình quản lý mật khẩu, chặn quảng cáo) chuyên dùng để bảo mật hoặc chặn mã độc, không có tính năng định dạng dữ liệu API.




2. **Cấu hình quyền Site Access:**
* **Lựa chọn:** Cấu hình quyền là **On click** (hoặc chỉ định trên trang cụ thể).
* **Giải thích theo quy tắc quản trị Extensions:** Theo quy tắc bảo mật, việc cấp quyền *On all sites* cho phép extension đọc và thay đổi dữ liệu trên mọi trang web bạn truy cập (bao gồm cả trang ngân hàng hoặc hệ thống nội bộ chứa dữ liệu nhạy cảm), gây nguy cơ rò rỉ dữ liệu (*Data Privacy Risk*). Cấu hình *On click* giúp bạn chủ động kiểm soát thời điểm extension được phép kích hoạt.



---

### Phần C – Phân tích luồng HTTP Request

1. **4 giai đoạn xử lý khi gõ `test.internal-tool.com` và nhấn Enter:**
1. **DNS Lookup (Phân giải tên miền):** Trình duyệt tra cứu địa chỉ IP của tên miền từ bộ nhớ đệm hoặc qua DNS Server.
2. **TCP Handshake (Thiết lập kết nối TCP):** Thực hiện bắt tay 3 bước (SYN, SYN-ACK, ACK) giữa trình duyệt và máy chủ.
3. **TLS/SSL Handshake (Thiết lập kết nối bảo mật):** Trao đổi chứng chỉ số và thiết lập kênh mã hóa an toàn (nếu dùng HTTPS).
4. **HTTP Request & Response (Gửi yêu cầu và nhận phản hồi):** Trình duyệt gửi gói tin HTTP Request tới server và nhận về mã nguồn (HTML/CSS/JS).


2. **So sánh 2 mã trạng thái và hướng debug:**

| Mã trạng thái | Ý nghĩa | Lỗi nằm ở đâu? | Hướng debug ban đầu |
| --- | --- | --- | --- |
| **404 Not Found** | Không tìm thấy tài nguyên hoặc đường dẫn URL yêu cầu trên máy chủ. | Phía Client (gõ sai URL, sai đường dẫn endpoint) hoặc Server (chưa cấu hình route đó). | Kiểm tra lại URL trên thanh địa chỉ hoặc endpoint trong Network tab; kiểm tra lại cấu hình route ở phía backend/frontend router. |
| **500 Internal Server Error** | Lỗi máy chủ nội bộ (Server gặp sự cố bất ngờ khi xử lý request). | Hoàn toàn thuộc về phía Server (lỗi code backend, lỗi kết nối database, exception chưa bắt). | Mở Server Console / Log hoặc kiểm tra tab Response/Preview trong Network tab để đọc thông báo lỗi chi tiết từ backend. |

3. **Tab DevTools để xem mã trạng thái HTTP:**
* **Tab lựa chọn:** Tab **Network**.
* **Vì sao không chọn tab khác:** *Elements* dùng để sửa DOM/CSS, *Console* dùng để xem log/chạy lệnh JS, *Application* dùng để quản lý Storage (Cookies, Local Storage, Cache). Chỉ có **Network** mới ghi lại toàn bộ các HTTP/HTTPS requests/responses kèm theo mã trạng thái (Status Code).



---

### Phần D – Chẩn đoán Cache và Cookies

1. **Dev sửa CSS nhưng F5 vẫn thấy giao diện cũ:**
* **Nguyên nhân:** Trình duyệt đã lưu tệp CSS cũ trong bộ nhớ đệm (**Browser Cache**), nên khi bấm F5 nó ưu tiên đọc file từ cache cục bộ thay vì tải bản mới từ server.
* **Khắc phục:** Thực hiện **Hard Reload** (`Ctrl + Shift + R` trên Windows hoặc `Cmd + Shift + R` trên Mac) để xóa cache riêng cho trang đó và tải lại toàn bộ tài nguyên mới nhất. Hoặc tick chọn **Disable cache** trong tab Network khi DevTools đang mở.


2. **Cookie `session_id` không có dấu tích ở cột `HttpOnly`:**
* **Rủi ro bảo mật:** Các đoạn mã JavaScript độc hại chạy trên trình duyệt (thông qua lỗ hổng XSS) có thể dễ dàng đọc được giá trị của cookie `session_id` bằng lệnh `document.cookie`.
* **Tên loại tấn công:** **Cross-Site Scripting (XSS)** dẫn đến đánh cắp phiên làm việc (**Session Hijacking**).


3. **Thao tác xóa phiên đăng nhập nhưng giữ nguyên Cache ảnh/CSS:**
* Mở tab **Application** (hoặc Storage) > Chọn mục **Cookies** ở thanh sidebar bên trái > Chọn domain của trang web > Nhấp chuột phải và xóa thủ công riêng cookie `session_id` (hoặc token định danh phiên) thay vì bấm Clear browsing data toàn bộ.



---

### Phần E – Debug bằng Inspect Element và Network Tab

1. **Sửa màu nút "Đăng nhập" trực tiếp trên trình duyệt:**
* Click chuột phải vào nút "Đăng nhập" -> Chọn **Inspect** (Kiểm tra).
* Tại tab **Elements**, chọn đúng thẻ HTML của nút đó.
* Nhìn sang khung **Styles** bên cạnh, tìm thuộc tính màu (`background-color`). Click vào ô màu nhỏ bên cạnh để chọn hoặc thử nghiệm mã màu trực tiếp trên giao diện trước khi báo lại cho Dev.


2. **Debug request API hiển thị dòng màu đỏ trên tab Network (Quy trình 4 Bước):**
* **Bước 1: Tái hiện & Quan sát (Reproduce & Observe):** Thao tác lại hành động gây ra lỗi trên giao diện và quan sát dòng request bị lỗi (hiện màu đỏ) trong tab Network.
* **Bước 2: Kiểm tra Request/Response (Inspect Request/Response):** Click vào request đó, xem kỹ **Status Code** (mã lỗi 4xx/5xx) và kiểm tra tab **Payload** (xem dữ liệu gửi lên có đúng định dạng không) cùng tab **Response / Preview** (đọc thông báo lỗi chi tiết từ server).
* **Bước 3: Khoanh vùng nguyên nhân (Isolate the cause):** Xác định lỗi do client (gửi thiếu tham số, sai token) hay do server (lỗi xử lý logic).
* **Bước 4: Kiểm chứng giải pháp (Verify solution):** Sau khi sửa code hoặc yêu cầu backend fix, thực hiện gửi lại request (nhấn chuột phải chọn *Replay XHR*) để kiểm tra xem request đã chuyển sang màu xanh (thành công) chưa.


3. **2 thao tác lọc API và tránh dữ liệu cũ trên tab Network:**
1. Bấm vào bộ lọc **Fetch/XHR** trên thanh công cụ của tab Network để chỉ hiển thị các lời gọi API (ẩn đi ảnh, CSS, font).
2. Tick chọn ô **Disable cache** (Vô hiệu hóa bộ nhớ đệm) để ép trình duyệt luôn tải dữ liệu mới nhất từ server mỗi khi gửi request.