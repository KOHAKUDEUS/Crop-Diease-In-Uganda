# Phân Loại Bệnh Lá Sắn - Dự Án Học Sâu

Kho lưu trữ này chứa các notebook Jupyter từ khóa học **AI Lab: Deep Learning for Computer Vision** của [WorldQuant University](https://www.wqu.edu/). Dự án tập trung vào việc phân loại bệnh lá sắn bằng các kỹ thuật học sâu, đặc biệt là **transfer learning** (chuyển giao học tập) và **callbacks** (hàm gọi lại) để tối ưu hóa quá trình huấn luyện. Bộ dữ liệu được lấy từ cuộc thi [Cassava Leaf Disease Classification trên Kaggle](https://www.kaggle.com/competitions/cassava-leaf-disease-classification).

## Mục Tiêu

- Xây dựng và huấn luyện mạng nơ-ron tích chập (CNN) cho bài toán phân loại đa lớp.
- Áp dụng **transfer learning** với mô hình được huấn luyện trước để cải thiện hiệu suất trên tập dữ liệu nhỏ.
- Sử dụng **callbacks** (ví dụ: lập lịch tốc độ học, dừng sớm) để huấn luyện hiệu quả hơn.
- Đánh giá mô hình bằng các chỉ số như độ chính xác, ma trận nhầm lẫn và đường cong mất mát/độ chính xác.

## Bộ Dữ Liệu

- **Nguồn**: Bộ dữ liệu phân loại bệnh lá sắn từ Kaggle (5 lớp: 4 bệnh + khỏe mạnh).
- **Vị trí**: Lưu tại `data_p2/data_undersampled/train` (đã được lấy mẫu dưới để cân bằng các lớp).
- **Tiền xử lý**: Hình ảnh được thay đổi kích thước về 224x224, chuẩn hóa và tăng cường dữ liệu (lật, xoay ngẫu nhiên).
- **Chia dữ liệu**: Tách thành tập huấn luyện/xác thực theo tỷ lệ 80/20 bằng `random_split`.

## Cấu Trúc Kho Lưu Trữ

| Notebook | Mô Tả |
|----------|-------|
| `022-explore-dataset.ipynb` | Khám phá bộ dữ liệu, trực quan hóa phân bố lớp và tạo tập dữ liệu cân bằng bằng lấy mẫu dưới. |
| `023-multiclass-classification.ipynb` | Xây dựng và huấn luyện CNN tùy chỉnh cho phân loại đa lớp. |
| `024-transfer-learning.ipynb` | Áp dụng transfer learning với mô hình ResNet được huấn luyện trước, tinh chỉnh cho phân loại lá sắn. |
| `025-callbacks.ipynb` | Tăng cường huấn luyện với các callback như lập lịch tốc độ học, dừng sớm và lưu mô hình tốt nhất. |



## Các Tính Năng và Nhiệm Vụ Chính

### Transfer Learning (`024-transfer-learning.ipynb`)

- **Mô hình**: Sử dụng ResNet được huấn luyện trước (ví dụ: ResNet-18/50) từ `torchvision.models`.
- **Quy trình**:
  - Đóng băng các tầng tích chập để giữ lại đặc trưng từ ImageNet.
  - Thay thế đầu phân loại bằng tầng tùy chỉnh cho đầu ra 5 lớp.
  - Tinh chỉnh với hàm mất mát `CrossEntropyLoss` và bộ tối ưu `Adam` (tốc độ học=0.001).
  - Huấn luyện với  cross validation k fold
- **Kết quả**: Đạt độ chính xác xác thực ~62-72% 
- **Đầu ra**: Tóm tắt mô hình, đường cong huấn luyện, ma trận nhầm lẫn, trọng số mô hình được lưu, ma trận k fold.

### Callbacks (`025-callbacks.ipynb`)

- **Tính năng**:
  - **Lập lịch tốc độ học**: Sử dụng `StepLR` để giảm tốc độ học (ví dụ: mỗi 3 epoch giảm 0.1 lần).
  - **Dừng sớm**: Ngừng huấn luyện nếu mất mát xác thực không cải thiện (patience=3 epoch).
  - **Lưu mô hình**: Lưu mô hình tốt nhất dựa trên độ chính xác/mất mát xác thực.
- **Quy trình**: Tích hợp các callback vào vòng lặp huấn luyện của mô hình transfer learning.
- **Kết quả**: Giảm thời gian huấn luyện và tránh quá khớp; duy trì độ chính xác cao.
- **Đầu ra**: Lịch sử huấn luyện (DataFrame), biểu đồ thể hiện hiệu quả của lập lịch, ma trận nhầm lẫn.

## Kết Quả

- **CNN cơ bản**:mô hình bị overfit nặng (từ `023-multiclass-classification.ipynb`).
- **Transfer Learning**: Độ chính xác ~60-70%, hội tụ nhanh hơn (từ `024-transfer-learning.ipynb`).
- **Callbacks**: Tối ưu hóa huấn luyện, tiết kiệm ~20-30% thời gian tính toán (từ `025-callbacks.ipynb`).
- **Trực quan hóa**: Đường cong mất mát/độ chính xác và ma trận nhầm lẫn thể hiện hiệu suất từng lớp.

## Cải Tiến Trong Tương Lai

- Thử nghiệm với các mô hình được huấn luyện trước khác (ví dụ: EfficientNet, Vision Transformers).
- Thêm tăng cường dữ liệu nâng cao (ví dụ: CutMix, MixUp).

## Giấy Phép

Được cấp phép theo [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/).

- **Được phép**: Tải xuống, chia sẻ, đăng lên kho lưu trữ công khai (với ghi công cho WorldQuant University và liên kết giấy phép).
- **Bị cấm**: Sửa đổi, tạo sản phẩm phái sinh hoặc sử dụng cho mục đích thương mại.
- **Hậu quả vi phạm**: Có thể dẫn đến bị trục xuất khỏi WorldQuant University.


- [WorldQuant University](https://www.wqu.edu/) vì tài liệu khóa học.
- [Kaggle](https://www.kaggle.com/) vì bộ dữ liệu lá sắn.
- PyTorch, Torchvision và các thư viện mã nguồn mở khác đã hỗ trợ dự án này.
