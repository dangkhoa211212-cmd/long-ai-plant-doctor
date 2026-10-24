# LONG AI PLANT DOCTOR V2 — triển khai trên Android

## Chức năng trong bản V2
1. Chụp ảnh bằng camera sau hoặc tải tối đa 4 ảnh.
2. Nhập cây/giai đoạn/vùng; AI có thể đề xuất nhận diện cây khi đã kết nối API thật.
3. Chẩn đoán phân biệt sâu, rầy, nấm bệnh và nguyên nhân phi sinh học từ ảnh; kết quả có giới hạn và yêu cầu kiểm tra thêm.
4. Khu vực ghi thông tin thuốc từ nhãn để kỹ sư đối chiếu. Không tự tạo công thức phối trộn.
5. Ghi chú, góp ý, lưu cục bộ và xuất hồ sơ JSON.

## Xuất bản PWA bằng GitHub Pages trên Android
1. Tải ZIP và giải nén trên điện thoại.
2. Đăng nhập https://github.com/ → `+` → `New repository` → tên `long-ai-plant-doctor` → `Create repository`.
3. Vào `Add file` → `Upload files`, tải `index.html`, `manifest.webmanifest`, `sw.js`, `icon.svg` lên root repo rồi `Commit changes`.
4. Vào `Settings` → `Pages` → `Deploy from a branch` → `main` → `/(root)` → `Save`.
5. Mở URL GitHub Pages bằng Chrome Android → menu `⋮` → `Thêm vào màn hình chính`/`Cài đặt ứng dụng`.

## Kết nối AI thị giác thật
1. Mở https://dash.cloudflare.com/ và tạo Worker.
2. Dán nội dung `worker.js` vào trình chỉnh sửa Worker rồi Deploy.
3. Trong `Settings` → `Variables and Secrets`, tạo Secrets:
   - `OPENAI_API_KEY`: khóa API của tài khoản OpenAI API.
   - `APP_ACCESS_CODE`: chuỗi bí mật do bạn tự đặt.
   - Có thể đặt biến `OPENAI_MODEL` thành model hỗ trợ vision trong tài khoản, ví dụ `gpt-4.1-mini` nếu còn được cung cấp.
4. Lấy URL Worker, mở PWA và nhập URL cùng `APP_ACCESS_CODE`.
5. Tích ô đồng ý gửi ảnh rồi bấm phân tích.

Lưu ý:
- ChatGPT và API là hai dịch vụ khác nhau; API có thể tính phí. Kiểm tra giá/hạn mức hiện tại trước khi chạy.
- Không bao giờ đưa khóa API vào HTML hoặc GitHub.
- Worker mẫu dùng CORS `*` cho dễ thử nghiệm. Trước khi sử dụng lâu dài, đổi `Access-Control-Allow-Origin` thành chính xác origin GitHub Pages và bổ sung giới hạn tốc độ/quota.
- Mã truy cập đơn giản không thay thế hệ thống tài khoản bảo mật.
- Ảnh được gửi tới nhà cung cấp AI để xử lý; chỉ bật gửi khi đồng ý.

## Dữ liệu thuốc BVTV
Bản này chưa tích hợp cơ sở dữ liệu đăng ký thuốc có thể xác minh tự động. Không dùng AI làm nguồn pháp lý duy nhất. Trước khi khuyến nghị sản phẩm, phải xác minh danh mục và nhãn hiện hành, đúng cây trồng + đúng đối tượng gây hại, liều, số lần sử dụng và thời gian cách ly. Không đề xuất phối trộn nếu nhãn không cho phép hoặc chưa có bằng chứng tương thích.

## Chất lượng chẩn đoán
- Ảnh nên có toàn cây, mặt trên/dưới lá, sâu/côn trùng hoặc thân/gốc.
- Cùng một triệu chứng có thể do nấm, vi khuẩn, virus, úng rễ, dinh dưỡng, thuốc hoặc thời tiết.
- Kết quả AI là chẩn đoán phân biệt hỗ trợ, không phải xét nghiệm xác nhận.
