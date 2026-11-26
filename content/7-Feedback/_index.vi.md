---
title: "Chia sẻ, đóng góp ý kiến"
date: 2026-11-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Tham gia chương trình **AWS First Cloud AI Journey (FCAJ)** là một dấu mốc rất ý nghĩa trong hành trình học tập và phát triển kỹ thuật của mình. Dưới đây là những cảm nhận thực tế và đóng góp ý kiến của mình sau 12 tuần trải nghiệm và cùng nhóm xây dựng dự án **PhishGuard AI**:

### Đánh giá chung

**1. Môi trường làm việc**  
Môi trường học tập và làm việc tại FCAJ — đặc biệt là trong các buổi sinh hoạt chuyên đề và weekly meetup tại Bitexco Financial Tower — rất cởi mở, năng động và giàu tinh thần chia sẻ. Các thành viên trong cộng đồng và ban điều phối luôn sẵn sàng hỗ trợ giải đáp vướng mắc kể cả ngoài giờ. Nếu chương trình có thêm một vài buổi giao lưu nhẹ nhàng giữa các nhóm ngay từ đầu khóa thì các bạn thực tập sinh sẽ dễ kết nối và học hỏi chéo lẫn nhau hơn nữa.

**2. Sự hỗ trợ của mentor / team admin**  
Đội ngũ mentor định hướng rất sát thực tế và luôn gợi mở tư duy kiến trúc thay vì đưa sẵn đáp án. Trong các buổi review thiết kế cho **PhishGuard AI**, những câu hỏi phản biện từ mentor đã giúp nhóm phát hiện sớm nhiều lỗ hổng biên quan trọng — như tách bạch xác thực DMARC của thư forward, chống trùng lặp khóa idempotency hay biện pháp bù cho đường upload thẳng S3 Presigned POST — qua đó rèn cho tụi mình tư duy của một kỹ sư bảo mật hệ thống thực thụ. Đội ngũ admin cũng chuẩn bị khung tài liệu, portal và hỗ trợ quy trình rất chu đáo.

**3. Sự phù hợp giữa công việc và chuyên ngành học**  
Là sinh viên chuyên ngành An toàn thông tin, việc thiết kế và triển khai nền tảng điều phối ứng phó lừa đảo đa kênh có người duyệt (**PhishGuard AI**) trên AWS Serverless cực kỳ sát với định hướng chuyên môn của mình. Mình được trực tiếp áp dụng các nguyên lý cốt lõi như IAM Least Privilege, Cognito RBAC, bảo toàn bằng chứng số (S3 Object Lock & KMS), che dữ liệu nhạy cảm PII và lớp phòng vệ AI Guardrails chống Prompt Injection vào một kiến trúc thực tế hoàn chỉnh.

**4. Cơ hội học hỏi & phát triển kỹ năng**  
Chương trình giúp mình nâng cấp toàn diện cả chiều sâu kỹ thuật lẫn quy trình làm việc nhóm. Bên cạnh việc thực hành thành thạo AWS SAM, Step Functions, Amazon Bedrock (RAG & Guardrails), Textract và CloudFront/WAF, mình còn học được cách viết đặc tả kỹ thuật bài bản, dự toán chi phí bằng AWS Pricing Calculator, xây dựng bộ ca kiểm thử nghiệm thu và phối hợp nhịp nhàng trong nhóm 5 thành viên.

**5. Văn hóa & tinh thần đồng đội**  
FCAJ xây dựng được văn hóa cộng đồng chia sẻ chân thành và cầu thị. Dù là trong các buổi meet-up sáng Thứ Bảy, các phiên phản biện kiến trúc hay những lúc cùng đồng đội chạy nước rút kiểm thử hệ thống, mọi người luôn tôn trọng và hỗ trợ nhau hết mình.

**6. Chính sách / phúc lợi cho thực tập sinh**  
Lộ trình thực hành với gói $200 AWS Credits giúp sinh viên có điều kiện trải nghiệm thực tế các dịch vụ lõi một cách bài bản và kiểm soát tốt chi phí trước khi bắt tay làm đồ án. Đặc biệt, cơ hội tham dự các sự kiện lớn của cộng đồng như *AWS Cloud & AI Day Hanoi 2026*, *Fireside Chat với Dr. Werner Vogels*, *Buildrathon Kickoff* và chuỗi *FCAJ Weekly Meetup* là điểm cộng rất lớn của chương trình.

---

### Một số câu hỏi khác

- **Điều bạn hài lòng nhất trong thời gian thực tập?**  
  Khoảnh khắc nhìn thấy **PhishGuard AI** từ những nét vẽ kiến trúc ban đầu trở thành một hệ thống serverless chạy thực tế hoàn chỉnh — nơi bộ luật tất định, cơ chế che PII, Bedrock RAG/Guardrails và quy trình duyệt Human-in-the-Loop trên Step Functions phối hợp trơn tru, đồng thời chứng minh được điểm rủi ro hoàn toàn bất biến trước các bẫy Prompt Injection.

- **Điều bạn nghĩ công ty/chương trình cần cải thiện cho các thực tập sinh sau?**  
  Nếu chương trình có thể cung cấp sớm tài liệu hướng dẫn cấu hình môi trường sandbox hoặc tên miền thử nghiệm cho các nhóm làm bài toán liên quan đến email (như Amazon SES Inbound), các nhóm sẽ tiết kiệm được nhiều thời gian chờ đợi xác minh DNS và mở giới hạn sandbox.

- **Nếu giới thiệu cho bạn bè, bạn có khuyên họ thực tập ở đây không? Vì sao?**  
  Chắc chắn là có. FCAJ giúp thu hẹp khoảng cách giữa kiến thức trên giảng đường và kỹ thuật triển khai cloud thực tế tại doanh nghiệp. Bạn sẽ được thử thách tự tay thiết kế, tối ưu chi phí, bảo mật và vận hành một hệ thống hoàn chỉnh từ đầu đến cuối dưới sự dẫn dắt của các chuyên gia AWS và cộng đồng giàu kinh nghiệm.

---

### Đề xuất & mong muốn

- **Bạn có đề xuất gì để cải thiện trải nghiệm trong kỳ thực tập?**  
  Tổ chức thêm một buổi "Architecture & Security Clinic" ở giữa kỳ để các nhóm có khoảng 15 phút review chéo lẫn nhau về phân quyền IAM, bảng dự toán chi phí và các trường hợp biên trước khi bước vào giai đoạn chạy nước rút MVP.

- **Bạn có muốn tiếp tục chương trình này trong tương lai?**  
  Có, mình rất mong muốn tiếp tục gắn bó với cộng đồng FCAJ — vừa để đào sâu hơn mảng Cloud Security & AI Security, vừa có cơ hội quay lại hỗ trợ, chia sẻ kinh nghiệm cho các bạn sinh viên khóa sau.

- **Góp ý khác (tự do chia sẻ):**  
  Xin gửi lời cảm ơn chân thành đến ban tổ chức FCAJ, các anh chị mentor, các diễn giả khách mời cùng 4 người đồng đội trong nhóm đã đồng hành suốt 12 tuần đáng nhớ. Những thói quen kỹ thuật, tư duy bảo mật và kết nối cộng đồng tích lũy được tại đây sẽ là hành trang quý giá cho chặng đường nghề nghiệp của mình sau này.
