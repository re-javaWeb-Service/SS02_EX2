Bài tập 2: Tối ưu Prompt - Bảo mật thông tin nhạy cảm

1. Nội dung Prompt sau khi tối ưu
   Vai trò: Bạn là một Chuyên gia Phát triển Phần mềm (Senior Java Developer) và Chuyên gia Bảo mật Ứng dụng.

Nhiệm vụ: Viết class VNPayPaymentService bằng ngôn ngữ Java để xử lý nghiệp vụ thanh toán trực tuyến qua cổng VNPay.

Yêu cầu kỹ thuật và bảo mật (Bắt buộc tuân thủ 100%):

Kiến trúc & Clean Code: Viết mã rõ ràng, dễ bảo trì, áp dụng các nguyên tắc Clean Code và Lập trình Hướng đối tượng (OOP).
Bảo mật (Zero Trust): TUYỆT ĐỐI KHÔNG ĐƯỢC phép hardcode bất kỳ thông tin nhạy cảm nào như API Key (HASH_SECRET) hay URL trực tiếp của VNPay vào trong mã nguồn.
Quản lý cấu hình: BẮT BUỘC sử dụng hàm System.getenv() để nạp các giá trị khóa bí mật này (ví dụ: VNPAY_HASH_SECRET, VNPAY_URL) từ biến môi trường của hệ thống.
An toàn vận hành (Fail-fast): Bắt buộc phải viết logic kiểm tra (validation) ngay trong constructor hoặc khối khởi tạo (init block). Nếu biến môi trường bị thiếu (giá trị trả về là null hoặc chuỗi rỗng ""), phải ném ra ngoại lệ IllegalStateException ngay lập tức kèm theo thông báo lỗi rõ ràng để ngăn chặn ứng dụng khởi tạo với trạng thái thiếu an toàn.
Hãy cung cấp mã nguồn hoàn chỉnh cho class này.

2. Đoạn mã nguồn Java hoàn chỉnh
   Dưới đây là kết quả mã nguồn Java sinh ra từ Prompt tối ưu trên, tuân thủ nghiêm ngặt các nguyên tắc bảo mật và kỹ thuật đã đề ra:

package com.example.payment.service;

/\*\*

- Service xử lý nghiệp vụ thanh toán trực tuyến qua VNPay.
- Áp dụng nguyên tắc bảo mật Zero Trust: không hardcode thông tin cấu hình nhạy cảm.
  \*/
  public class VNPayPaymentService {

      private final String vnpUrl;
      private final String hashSecret;
      private final String vnpTmnCode; // Thêm mã Terminal ID thường dùng trong VNPay

      /**
       * Constructor khởi tạo dịch vụ thanh toán VNPay.
       * Áp dụng nguyên tắc Fail-fast: Kiểm tra các biến môi trường cấu hình ngay khi khởi tạo.
       *
       * @throws IllegalStateException nếu thiếu bất kỳ cấu hình môi trường bắt buộc nào.
       */
      public VNPayPaymentService() {
          // Nạp cấu hình từ biến môi trường hệ thống
          this.vnpUrl = System.getenv("VNPAY_URL");
          this.hashSecret = System.getenv("VNPAY_HASH_SECRET");
          this.vnpTmnCode = System.getenv("VNPAY_TMN_CODE");

          // Logic kiểm tra (validation) các biến môi trường
          validateEnvironmentVariables();
      }

      /**
       * Kiểm tra tính hợp lệ của các biến cấu hình từ môi trường.
       */
      private void validateEnvironmentVariables() {
          if (this.vnpUrl == null || this.vnpUrl.trim().isEmpty()) {
              throw new IllegalStateException("Lỗi cấu hình bảo mật: Biến môi trường 'VNPAY_URL' bị thiếu hoặc rỗng.");
          }
          if (this.hashSecret == null || this.hashSecret.trim().isEmpty()) {
              throw new IllegalStateException("Lỗi cấu hình bảo mật: Biến môi trường 'VNPAY_HASH_SECRET' bị thiếu hoặc rỗng.");
          }
          if (this.vnpTmnCode == null || this.vnpTmnCode.trim().isEmpty()) {
              throw new IllegalStateException("Lỗi cấu hình bảo mật: Biến môi trường 'VNPAY_TMN_CODE' bị thiếu hoặc rỗng.");
          }
      }

      /**
       * Phương thức xử lý nghiệp vụ tạo URL thanh toán (Minh họa logic sử dụng cấu hình)
       *
       * @param orderId Mã đơn hàng
       * @param amount Số tiền cần thanh toán
       * @return URL thanh toán để chuyển hướng (redirect) người dùng
       */
      public String createPaymentUrl(String orderId, long amount) {
          // ... (Logic xây dựng tham số và mã hóa tạo URL sử dụng hashSecret, vnpTmnCode, vnpUrl) ...

          // Ví dụ minh họa sử dụng các biến cấu hình để không bị cảnh báo unused variable
          System.out.println("Đang tạo URL thanh toán gửi tới: " + this.vnpUrl);
          System.out.println("Mã TmnCode sử dụng: " + this.vnpTmnCode);

          // Trả về một URL giả định phục vụ demo
          return this.vnpUrl + "?orderId=" + orderId + "&amount=" + amount + "&hash=DEMO_HASHED_VALUE";
      }

  }
