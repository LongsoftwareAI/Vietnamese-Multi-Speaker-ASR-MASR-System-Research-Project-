# Vietnamese Multi-Speaker ASR (MASR) System 🎙️🇻🇳

## 📖 Giới thiệu (Overview)
Dự án này đề xuất và phát triển một hệ thống Nhận dạng giọng nói tự động đa người nói (Multi-speaker Automatic Speech Recognition - MASR) tối ưu hóa riêng cho tiếng Việt. [cite_start]Bằng cách cải tiến kiến trúc dựa trên Whisper, tích hợp **Time-Synchronous Rotary Positional Encoding (TS-ROPE)** và **Hyper-SD**, hệ thống có khả năng nhận diện đồng thời "ai nói, khi nào, và nói gì" trong các điều kiện hội thoại phức tạp, đa vùng miền và có tiếng nói chồng lấn (overlapping speech)[cite: 4, 6, 8].

## ✨ Tính năng nổi bật (Key Features)
* [cite_start]**End-to-end Pipeline:** Luồng xử lý hoàn chỉnh từ VAD, Diarization, Verification đến ASR[cite: 91, 92, 94, 95, 96].
* [cite_start]**Xử lý hội thoại chồng lấn (Overlap Handling):** Mô hình được fine-tune với kỹ thuật *synthetic overlap augmentation* (trộn dữ liệu nhân tạo) để tăng cường độ mạnh mẽ trong thực tế[cite: 89].
* [cite_start]**Speaker Diarization độ chính xác cao:** Sử dụng đặc trưng nhúng từ **WavLM** kết hợp với thuật toán **Spectral Clustering** (Phân cụm phổ)[cite: 166].
* [cite_start]**Cơ chế kiểm chứng chéo (Cross-Reference Verification):** Tăng độ tin cậy bằng cách đối chiếu mức độ đồng thuận giữa quyết định của module MASR và cụm WavLM[cite: 168, 188].
* [cite_start]**ASR tiếng Việt tối ưu:** Tích hợp mô hình **PhoWhisper-Large** mang lại kết quả nhận dạng văn bản tiếng Việt vượt trội và mạch lạc[cite: 170].

## 🛠️ Kiến trúc Hệ thống (System Architecture)
[cite_start]Hệ thống xử lý âm thanh đầu vào qua 4 module chính[cite: 111]:
1. **Pre-processing (VAD & Cut/Merge):** Áp dụng Energy-based VAD (dựa trên năng lượng) để loại bỏ khoảng lặng. [cite_start]Áp dụng cơ chế chia nhỏ gối đầu (overlapping cut) hoặc gộp audio để tạo đầu vào ổn định (tối đa 30s)[cite: 114, 125].
2. [cite_start]**Diarization:** Trích xuất "vân tay giọng nói" (vector 512 chiều) thông qua mạng WavLM và xác định ranh giới người nói bằng Spectral Clustering[cite: 166].
3. **Verification:** Đánh giá mức độ đồng thuận. [cite_start]Khi MASR không đạt ngưỡng tin cậy, hệ thống tự động điều chỉnh dựa trên mô hình cơ sở WavLM[cite: 168, 190].
4. [cite_start]**ASR & Output:** Giải mã nội dung bằng PhoWhisper-Large và xuất kết quả trực quan dưới định dạng TXT, SRT hoặc JSON kèm mốc thời gian[cite: 170, 171].

## 🗂️ Cấu trúc Repo (Repository Structure)
Dự án được chia thành các quy trình thực nghiệm rõ ràng thông qua các notebook:
* `MASR Fine-tuning on ViMD.ipynb`: Khung huấn luyện chính (fine-tuning) hệ thống MASR trên bộ dữ liệu tiếng Việt đa vùng miền với cơ chế tạo overlap tự động.
* `AMI_training epoch.ipynb`: Huấn luyện và chuẩn hóa khả năng diarization trên bộ dữ liệu chuẩn AMI Meeting Corpus.
* `Bản sao của eval_pipeline_vimd_on_val_test.ipynb`: Script chạy đánh giá hiệu năng toàn diện của pipeline trên tập validation/test, đo lường tự động các chỉ số WER, CER và DER.
* `test_model.ipynb`: Notebook hỗ trợ inference, dùng để chạy thử nghiệm pipeline trên các file audio thô thực tế và kiểm tra output.

## 💽 Dữ liệu (Datasets)
* [cite_start]**ViMD (Vietnamese Multi-Dialect Dataset):** ~102 giờ thu âm đa dạng từ 63 tỉnh thành tại Việt Nam, dùng để huấn luyện nhận dạng tiếng Việt[cite: 64, 65].
* [cite_start]**AMI Meeting Corpus:** ~100 giờ ghi âm hội thoại tiếng Anh, cung cấp nhãn speaker identity chi tiết để tối ưu hóa module Diarization[cite: 58, 59].

## 📊 Kết quả thực nghiệm (Results)
[cite_start]Đánh giá trên tập validation của ViMD.fix (1494 mẫu âm thanh)[cite: 174]:
* [cite_start]**Diarization Error Rate (DER):** 18.0% [cite: 175]
* [cite_start]**Word Error Rate (WER):** Tổng thể đạt 42.6% (giảm xuống còn 21.36% trên các đoạn hội thoại không chồng chéo)[cite: 175, 202].
* [cite_start]**Tốc độ xử lý (Inference Speed):** Đạt trung bình 7.0 giây / mẫu âm thanh[cite: 191].
