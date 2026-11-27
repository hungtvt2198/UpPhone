# UpPhone – Phone Farm Shopee Video Uploader

> **Phiên bản mới nhất:** **`v1.0.18`** – sửa triệt để lỗi văng app đột ngột `Qt FATAL: QThread: Destroyed while thread is still running` (quản lý tập trung vòng đời mọi luồng nền qua `crash_guard`, kiểm tra cả `isRunning()` & `wait(0)`, bảo vệ tín hiệu muộn trên hộp thoại và thoát tiến trình an toàn), gia cố kiểm tra biên bộ nhớ/ngưỡng gói `scrcpy-server`, và sửa lỗi `KeyError` giao diện (`MultiSerialComboBox`, `AppsScriptCodeDialog`).

## Cài đặt & Cập nhật
### Cách 1: Dành cho người dùng (File cài đặt 1 chạm)
1. Tải và chạy **`UpPhone_Setup.exe`** (từ trang Releases `https://github.com/hungtvt2198/UpPhone/releases`).
2. Bấm **Yes** khi Windows hỏi quyền Quản trị viên (UAC) → chọn thư mục cài đặt (mặc định **`C:\Program Files\UpPhone`**) → bấm **Cài đặt ngay**.
3. Mỗi khi mở ứng dụng, màn hình khởi động (**Load App**) sẽ tự động kiểm tra phiên bản mới nhất: nếu có bản mới sẽ tự động tải trên thanh tiến trình và chạy bộ cài đặt nâng cấp; nếu đã ở bản mới nhất sẽ vào thẳng ứng dụng. Bạn cũng có thể bấm nút **Update** (*Check for Update*) ở góc dưới cùng thanh chọn tab bên trái bất cứ lúc nào (mọi workflow, ảnh mẫu, ghi chú và số máy trong `C:\Program Files\UpPhone` đều được giữ nguyên).

### Cách 2: Chạy từ mã nguồn (Dành cho phát triển)
1. `conda activate E:\UpPhone\venup` rồi `pip install -r requirements.txt`
2. Giải nén **scrcpy-win64** (có kèm adb) vào `tools/`. Đặt **ADBKeyboard.apk** vào `tools/` (để nhập tiếng Việt có dấu / emoji).
3. Bật *Gỡ lỗi không dây* trên điện thoại (hoặc `adb tcpip 5555` qua USB một lần).
4. Chạy `run.bat` hoặc `python main_ui.py` (đóng gói lại bộ cài bằng `python packaging\build_installer.py`).

## Giao diện (Soft Fluent – Light / Dark Mode)
- Màn hình khởi động tối giản đồng bộ giao diện Sáng / Tối; nút chuyển **Light / Dark Mode** nằm ngay phía trên nút **Update** ở thanh trái (Light Mode đạt độ tương phản cao, tách biệt rõ từng thẻ & bảng).
- Hộp thoại tải bản cập nhật và trình cài đặt **`UpPhone_Setup.exe`** thiết kế đồng bộ Soft Fluent, hiển thị dung lượng, tốc độ tải (`MB/s`), thời gian ước tính và tiến độ trực quan.
- Thanh icon bên trái chuyển giữa 5 trang:

| Icon | Trang | Nội dung |
|---|---|---|
| ☰ | **Dữ liệu** | Bảng các dòng Excel (Pill Badge trạng thái trực quan), chạy / hẹn giờ / dừng / xóa |
| 🖧 | **Thiết bị đã kết nối** | Danh sách máy trong `adb devices` kèm thẻ thống kê trực tuyến `USB / WiFi`, ghi chú, Mirror |
| 📱 | **Phone control** | Lưới màn hình trực tiếp ~0 ms (tự kết nối lại khi rớt mạng) & điều khiển nhóm 20+ máy mượt mà |
| ⚡ | **Workflow** | Thiết kế workflow dạng node (đường nối Orthogonal góc vuông 90° kèm 4 nhóm màu ngữ nghĩa) |
| 🕘 | **Nhật ký chung** | Log của tất cả các máy |

### Trang Dữ liệu
- **Nhập Google Sheet**: dán trực tiếp đường link Google Sheet theo ngày, tự bỏ qua các tab tổng hợp (`Summary`, `All Channels`, `Data Master`), đưa các tab có dữ liệu lên đầu danh sách (nhận cả các dòng đã điền video/link trước khi điền `Serial`), bấm vào cả dòng để chọn từng tab kênh muốn chạy, hiện thanh tiến độ `Đang nhập dữ liệu...` rồi tự động thoát về bảng tác vụ; chỉ giữ đúng 7 cột (`Channel`, `Serial`, `Workflow`, `Video_Path`, `Affiliate_Link_shopee`, `Title_shopee_affiliate`, `link_air`), tự chuyển đường dẫn mạng UNC sang `D:\...` và tự động ghi bù `link_air` / `"Done"` về đúng dòng & cột `link_air` trên tab con nguồn qua 1 Apps Script Web App (`/exec`) dùng chung.
- **Nhập Excel**: cho phép chọn **nhiều file Excel cùng lúc** chứa thông tin đăng video; nếu bảng đã có dữ liệu (cả khi đang rảnh lẫn đang chạy) sẽ tự cộng dồn vào cuối bảng mà không làm mất các file đã nhập trước đó (nếu add lại chính file đang mở và có dòng vừa sửa sang `Done` / có `link_air` thì tự cập nhật trực tiếp sang *Thành công*). Toàn bộ danh sách file, dòng đã add, chỉnh sửa và dấu tích được **tự động lưu & khôi phục ngay khi mở lại app nếu app bị crash hoặc PC restart** (các dòng đang chạy dở sẽ chuyển về *Chờ* để chạy tiếp). **Mẫu Excel**: lưu file template. **Xuất Excel**: xuất các dòng đang hiển thị ra file mới, kèm *Trạng thái* và *Thời gian kết thúc*. Khi nhập lại file đã xuất, trạng thái được giữ.
- **Thêm**: nhập tay 1 dòng (chọn Serial kèm ghi chú, chọn/nhập ID Kênh từ Google Sheet Data Master). **Chạy (n)** / **Hẹn giờ** / **Xóa (n)**: áp dụng cho các dòng đã tick và đang hiển thị (dấu tích ở tất cả các dòng — kể cả dòng đang chạy — luôn hiển thị và không bị khóa để có thể chọn/bỏ chọn tự do bất cứ lúc nào). **Dừng**: nút có nền riêng biệt và tự sáng nền màu đỏ nổi bật khi đang chạy; bấm **Dừng** sẽ ngắt toàn bộ tiến trình trên mọi máy ngay lập tức (hoặc hủy lịch hẹn giờ đang đếm ngược).
- **Hẹn giờ bắt đầu chạy**: chọn thời điểm cụ thể (ngày/giờ/phút + nút chọn nhanh `+15 phút`, `+30 phút`, `+1 giờ`, `+2 giờ`) hoặc đếm ngược sau X giờ Y phút; nút `Hẹn giờ` hiển thị thời gian đếm ngược trực tiếp và tự động kích hoạt chạy khi đến giờ.
- **Lọc dữ liệu**: lọc theo từ khóa, **nhiều thiết bị (Serial)** cùng lúc (tick từng máy hoặc bấm `Chọn tất cả` / `Bỏ chọn`), workflow hoặc **nhiều trạng thái** cùng lúc (không tick ô nào = tất cả). Danh sách **Serial** hiện ghi chú của máy; máy chưa có ghi chú thì hiện IP. Số đỏ trên nút là số điều kiện lọc đang bật.
- **Dừng hẳn khi dòng lỗi**: mỗi máy chạy lần lượt các dòng của mình; khi dòng nào gặp lỗi thì dòng đó dừng hẳn ở trạng thái **Lỗi** (không đợi chạy lại) và máy chuyển sang dòng kế tiếp.
- Trên mỗi dòng:
  - 🖥 cạnh serial: mở **cửa sổ Mirror** của máy (giống nút Mirror ở trang Thiết bị), tiêu đề là *Ghi chú* của máy. Bấm lại thì đưa cửa sổ lên trước.
  - 👁 cạnh video: mở video. 📁: **chọn lại file video** (khi đường dẫn trong Excel bị sai).
  - 📄 **Log**: xem workflow đang chạy đến bước nào; nếu lỗi thì hiện chi tiết lỗi. Nhấp đúp vào ô không sửa được cũng mở Log. Dòng lỗi có thêm icon ⚠ đỏ.
  - ▶ **Chạy** riêng dòng đó. Nếu máy đang chạy dòng khác thì phần mềm báo *Máy đang bận*. Khi dòng đang chạy hoặc đang chờ, nút đổi thành ■ để dừng riêng dòng đó; máy sẽ chạy tiếp dòng sau.
  - 🗑 **Xóa** dòng khỏi danh sách. File Excel không bị thay đổi. Không xóa được dòng đang chạy.
- **Sửa trực tiếp trên bảng**: bấm 1 lần vào ô **ID**, **SERIAL**, **TÊN WORKFLOW**, **ĐƯỜNG DẪN VIDEO**, **LINK AFFILIATE** hoặc **TIÊU ĐỀ** để sửa. Enter để lưu, Esc để hủy.
  - Mọi dòng trên bảng — kể cả dòng đang ở trạng thái **Đang chạy** hay đang *Chờ* — đều chỉnh sửa trực tiếp được bất kỳ lúc nào (bao gồm cả nút 📁 chọn lại file video).
  - SERIAL: gõ IP (không cần `:5555`) hoặc serial USB. Cột Ghi chú tự đổi theo máy mới.
  - TÊN WORKFLOW: chọn trong danh sách các workflow đang có.
  - TIÊU ĐỀ: Shift+Enter để xuống dòng. Chỗ sửa chỉ lưu trong phần mềm, file Excel gốc không bị thay đổi; muốn lưu ra file thì bấm **Xuất Excel**.
- Cột **STT**: số thứ tự của dòng trong phần mềm (1, 2, 3...), không phải số dòng trong Excel. Lọc dữ liệu thì số không đổi; xóa dòng thì đánh số lại. Rê chuột để xem số dòng Excel.
- Cột **Trạng thái** chỉ hiện *Thành công*, *Lỗi*, *Đang chạy* (3 chấm nhấp nhô), ngoài ra còn *Chờ* / *Đã dừng*. Chi tiết lỗi xem ở nút 📄 **Log**. Rê chuột vào dòng đang chạy để xem bước đang chạy.
- Cột **Ghi chú** hiện ghi chú của máy (nhập ở trang Thiết bị).
- Cột **LINK AIR**: ngay khi workflow lấy link (`shopee_AI_link`, `shopee_link`) lấy được link video đã đăng (`GETCLIPBOARD`), phần mềm tự hiển thị kèm nút 📋 copy nhanh **và tự động lưu thẳng vào đúng dòng trong file Excel gốc**; với workflow không lấy link, phần mềm tự điền **`Done`** vào cột `link_air` sau khi chạy xong (nếu file đang mở trong Microsoft Excel, phần mềm sẽ tự ghi bù ngay khi bạn đóng Excel). Dù do app ghi hay do người dùng tự điền **`Done`** (sau khi up tay) / `link_air`, khi **Nhập Excel** lại file đó thì mọi dòng đã có `Done` hoặc `link_air` trên tất cả các workflow đều tự động được nhận diện là **Thành công** để bỏ qua, không đăng trùng lại.

### Trang Thiết bị đã kết nối
- Liệt kê các máy đang có trong `adb devices` (USB hoặc IP), tự làm mới 10 giây/lần; bấm **Làm mới** để đọc lại ngay.
- Cột **Số serial**: máy WiFi chỉ hiện IP (không có cổng `:5555`).
- 📋 **Copy**: copy serial đang hiển thị. 🖥 **Mirror**: mở cửa sổ màn hình riêng của máy, tiêu đề *Ghi chú - Serial - Model* (bấm lại thì đưa cửa sổ lên trước).
- Cột **Ghi chú**: gõ trực tiếp (vd tên các kênh đã đăng nhập), tự lưu vào `device_notes.json`. Cùng 1 máy cắm USB hay nối IP dùng chung ghi chú.
- Ô tìm kiếm: lọc theo serial hoặc ghi chú.

#### Panel quét IP (bên phải trang Thiết bị)
- **Bước 1 – Kích hoạt cổng mạng:** nút **Bật (n)** chạy `adb tcpip 5555` cho n máy đang cắm USB rồi kết nối WiFi luôn.
  - Ô **Tự bật tcpip khi cắm USB** (mặc định bật): phone khởi động lại sẽ mất tcpip, chỉ cần cắm USB là phần mềm tự bật lại (khoảng 10 giây), sau đó rút cáp. Chỉ hoạt động khi trang Màn hình phone đang ở chế độ WiFi.
- **Bước 2 – Quét thiết bị:** nhập dải IP vào 4 ô, mỗi ô là 1 số hoặc khoảng `a-b` (vd `192 · 168 · 10 · 1-255`).
  - **Quét**: tìm các máy mở cổng 5555 trong dải và `adb connect` (255 IP khoảng 1 giây).
  - **Thêm**: lưu dải vào danh sách để lần sau bấm **Quét** trên thẻ, hoặc **Quét tất cả**.
- Ô **Tự quét khi mở phần mềm** (mặc định bật): khi PC khởi động lại, mở phần mềm là tự kết nối lại các IP đã từng kết nối và quét các dải đã lưu.

## Kết nối điện thoại (trang Màn hình phone)
- **📶 WiFi**: kết nối tới `IP:5555` ghi trong Excel. Phần mềm tự kết nối lại khi mất kết nối.
- **🔌 USB**: dùng các máy đang cắm cáp. Cột `Serial` có thể ghi:
  - serial USB (xem bằng `adb devices`, vd `R58M12ABC`), hoặc
  - giữ nguyên `IP:5555`: phần mềm đọc IP WiFi của từng máy USB để tìm máy tương ứng. Như vậy cùng một file Excel dùng được cho cả hai chế độ.
- **📶 USB → WiFi**: bật `adb tcpip 5555` cho mọi máy đang cắm USB, kết nối WiFi luôn và copy danh sách `IP:5555` để dán vào Excel.
- **Điều khiển trực tiếp & Đồng bộ nhóm 20+ máy mượt mà (Group Control độ trễ ~0 ms)**:
  - Nhấp đúp vào ô thiết bị trên lưới để phóng to màn hình điều khiển (`StreamDialog`) mở ngay trong `0 ms`. Ô nhỏ trên lưới tự phủ tối kèm bảng trạng thái **🎮 Đang điều khiển** và viền xanh lá để nhận biết rõ máy đang mở lớn.
  - Phiên chiếu màn hình giữ kết nối ổn định kể cả khi máy đứng yên không thao tác, tự động đẩy lại `scrcpy-server` & kết nối lại liên tục khi WiFi ADB chập chờn (hoặc bấm **Kết nối thiết bị** để ép nối lại tức thì, tự dọn IP đã ngoại tuyến và hiện dải cảnh báo ở đáy khung hình nếu đang mất kết nối).
  - Tối ưu giải mã H.264 với `VideoReformatter` riêng trên từng thiết bị và downscale lưới trực tiếp ở tầng C cho dàn 20–40 máy, gửi sự kiện chạm bất đồng bộ kèm gộp tọa độ rê chuột theo từng thiết bị để thao tác đồng loạt không bị giật/trễ.
  - Thanh chức năng bên phải màn hình phóng to bố trí theo **Bento Grid 4 nhóm** gồm 21 nút tiện ích: **📂 Truyền file từ PC**, **🗑 Xóa file Thư viện** (xóa sạch ảnh/video trong Thư viện và làm mới MediaStore cho **1 máy** đang mở hoặc **đồng bộ nhiều máy** đang chọn), **📦 Cài đặt APK**, xoay màn hình, chụp ảnh màn hình, tắt màn hình vật lý vẫn chiếu trên PC, đặt số máy làm hình nền...

## File Excel (template `import/shopee_upload_teamplte.xlsx`)
| Cột | Ý nghĩa | Biến trong workflow |
|---|---|---|
| **Serial** | `IP:5555` hoặc serial USB | `{{Serial}}` |
| **Workflow** | tên workflow (vd `shopee_AI`) | `{{Workflow}}` |
| **Video_Path_shopee_affiliate** | đường dẫn video trên PC | `{{Video_Path}}` = `{{localPath}}` |
| **Affiliate_Link_shopee** | link affiliate | `{{Affiliate_Link}}` = `{{link}}` |
| **Title_shopee_affiliate** | **title dùng khi đăng video** | `{{Title}}` = `{{title}}` |
| hashtag | hashtag (không tự nối vào title) | `{{hashtag}}` |
| link_air | link video đã đăng (tự lưu thẳng vào file Excel gốc; dòng có sẵn `link_air` sẽ tự đặt là *Thành công* khi nhập lại) | `{{link_air}}` |
| Title | không dùng | – |

- Cột in đậm là bắt buộc. Tên cột không phân biệt hoa/thường, thứ tự tùy ý; dòng không có Serial bị bỏ qua.
- Mỗi dòng là một lần đăng video. Các dòng của cùng một máy chạy lần lượt; các máy khác nhau chạy song song.
- Có thể thêm cột tùy ý (vd `Ghi_chu`) và dùng trong workflow dưới dạng `{{Ghi_chu}}`.
- Bấm **Mẫu Excel** để có file Excel đúng bố cục template.

## Thiết kế workflow dạng node (trang ⚡ Workflow)
- **➕ Mới**: tên workflow phải trùng với cột `Workflow`. Workflow được lưu ở `workflows/<tên>.json` (định dạng Vue Flow, giống tool gốc).
- **📥 Nhập JSON / 📤 Xuất JSON**: mở hoặc xuất workflow của tool gốc (vd `import/shopee_AI.json`, đã được nhập sẵn thành workflow `shopee_AI`).
- **Thêm node**: kéo từ bảng bên trái vào canvas. Nhấp đúp để thêm node bên phải node đang chọn và tự nối dây.
- **Nối dây**: kéo từ cổng phải (ra) sang node khác. Mỗi node có 1 dây ra; workflow chạy từ **START** theo dây.
- **FINALLY**: nhánh nối sau FINALLY luôn chạy khi kết thúc, kể cả khi lỗi hoặc bấm Dừng. Dùng để dọn dẹp, vd xóa video đã đẩy.
- **Lỗi**: mỗi node chọn *Dừng workflow* hoặc *Bỏ qua lỗi, chạy tiếp*.
- **Phím tắt**:
  - Del: xóa
  - Ctrl+C / Ctrl+V / Ctrl+D: chép / dán / nhân bản
  - Ctrl+A: chọn hết
  - F: vừa màn hình
  - Ctrl+S: lưu
  - Lăn chuột: zoom
  - Chuột giữa hoặc giữ Space + kéo: di chuyển canvas
- **Công cụ trong form node**:
  - 📍 chọn tọa độ trên ảnh chụp màn hình
  - 🔍 bấm vào phần tử để lấy XPath
  - 🧪 kiểm tra XPath
  - ✂ cắt ảnh mẫu (lưu vào `templates/`)
  - Trên ảnh chụp: chuột phải = chạm thật lên máy, rồi tự chụp lại.
- **Chạy thử** (bên phải): chạy toàn bộ, chạy từ node chọn, hoặc chạy 1 node, với biến test. Node đổi màu khi chạy: xanh dương = đang chạy, xanh lá = xong, đỏ = lỗi.
  - Node SETVARIABLE không nối dây được coi là giá trị test (chỉ dùng khi bật ô tương ứng). Khi chạy thật, giá trị lấy từ Excel.

### Biến
- Cú pháp `{{ten_bien}}`. Có sẵn: `{{ID}} {{Channel}} {{Video_Path}} {{Title}} {{Affiliate_Link}} {{hashtag}} {{Video_Name}} {{Serial}} {{Row}}`, tên cột gốc (`{{Title_shopee_affiliate}}`...) và các cột thêm trong Excel.
- Tên gọi của tool gốc được tự ánh xạ: `{{localPath}}` = Video_Path, `{{title}}` = Title (tức Title_shopee_affiliate), `{{link}}` = Affiliate_Link.
- RANDOMSTRING / SETVARIABLE tạo biến mới, vd `{{file}}` để đặt tên video khi PUSHFILE.

### Ghi chú về node
- **XPATH**: chờ tới khi thấy phần tử (tối đa *Chờ tối đa*), sau đó chạm vào giữa phần tử.
- **CLICKIMAGE**: tìm ảnh mẫu trên màn hình bằng OpenCV, khớp ≥ ngưỡng thì chạm.
- **SETCLIPBOARD / PASTE**: dùng app **Clipper** (`ca.zgrs.clipper`) nếu máy có cài.
  - Không có Clipper: PASTE gõ nội dung bằng ADBKeyboard.
- **DELETEFILE**: không cho xóa thư mục hệ thống/gốc (`/sdcard`, `DCIM/Camera`...).
- **SWITCHACCOUNT**: tự động tìm tài khoản `{{ID}}` trên màn hình *Đổi tài khoản* (nếu dòng chứa `Hiện tại` thì bấm quay lại vì đã đúng kênh, nếu là kênh khác thì bấm chuyển tài khoản).
- **VERIFYCHANNEL**: kiểm tra tên kênh đang đăng nhập trên màn hình khớp với cột `ID` trước khi tiếp tục, giúp tránh đăng nhầm kênh.

> Workflow dạng danh sách cũ được tự chuyển sang dạng node khi mở. `Shopee_Mau` chỉ là ví dụ (tọa độ giả định).
> Ảnh mẫu trong `shopee_AI` trỏ tới `D:\Tool upload video\123\template.png` – hãy chọn/cắt lại nếu máy bạn không có file này.
