# N04 – Khử nhiễu ảnh dựa trên ảnh thật (Bilateral filter + Immerkær)

Bài tập lớn môn **Xử lý ảnh & Thị giác máy tính (121036)** – Nhóm N04.

Toàn bộ quá trình thực nghiệm (E1–E4), lý thuyết, nhận xét, bảng số liệu và hình ảnh nằm trong notebook `N04_Project.ipynb`.
Mọi ảnh và biểu đồ hiển thị ngay dưới ô code sinh ra nó; notebook không ghi file nào ra ngoài.

## 1. Cấu trúc thư mục

```
xla-tgmt-project/
├── N04_Project.ipynb        # notebook chính (chạy từ đầu đến cuối)
├── Computer_Vision 32.pdf   # hướng dẫn bài tập lớn
├── anh/                     # dữ liệu ảnh thật (NIND, CC BY-SA 4.0)
│   ├── Nguon.txt            # nguồn dữ liệu
│   ├── Ground_Truth/        # ảnh sạch: <ten_canh>_ISO200.jpg
│   ├── ISO_Low/             # ảnh nhiễu nhẹ  (ISO 400–800)
│   ├── ISO_Medium/          # ảnh nhiễu vừa  (ISO 1250–3200)
│   └── ISO_High/            # ảnh nhiễu nặng (ISO 6400)
```

Ảnh nhiễu và ảnh sạch được ghép cặp theo **tên cảnh** (phần trước `_ISO`). Ví dụ
`ISO_High/NIND_gnome_ISO6400.jpg` ghép với `Ground_Truth/NIND_gnome_ISO200.jpg`.

## 2. Cài đặt

Cần Python 3.10 trở lên.

```bash
pip install numpy pandas matplotlib opencv-python scikit-image scipy jupyter
```

## 3. Chạy lại toàn bộ thí nghiệm

Cách 1 – mở notebook bằng Jupyter / VS Code và chọn **Run All**.

Cách 2 – chạy bằng dòng lệnh (không cần mở giao diện):

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=2400 N04_Project.ipynb
```

Thời gian chạy khoảng 6–8 phút trên máy tính cá nhân (phần lâu nhất là khảo sát tham số E2 và E4-B).
Seed ngẫu nhiên được cố định (`SEED = 42`) nên kết quả lặp lại được.

## 4. Các thí nghiệm trong notebook

| Mục    | Nội dung                                                                                     |
| ------ | -------------------------------------------------------------------------------------------- |
| 3      | Tải dữ liệu, tính σ_true = std(noisy − clean), phân tầng nhẹ / trung bình / nặng             |
| 4      | Cài đặt Gaussian (baseline), Bilateral (P), Immerkær, Bilateral thích nghi (biến thể)        |
| 5 (E1) | Baseline Gaussian theo tầng + 3 ảnh thất bại tiêu biểu                                       |
| 6 (E2) | Khảo sát ksize, σ (Gaussian); d, σ_s, σ_r (Bilateral); hệ số k (biến thể) – chỉ trên tập dev |
| 7      | So sánh 3 phương pháp trên tập eval theo tầng (PSNR, SSIM, thời gian)                        |
| 8 (E3) | Ablation: tắt range kernel, tắt Immerkær                                                     |
| 9 (E4) | Điểm gãy                                                                                     |
| 10     | Thảo luận, đối chiếu dự đoán, kết luận, tài liệu tham khảo                                   |
| 11     | Tổng hợp lại các bảng và hình quan trọng nhất để tra cứu khi viết báo cáo                    |

## 5. Nguồn dữ liệu

B. Brummer and C. De Vleeschouwer, "Natural Image Noise Dataset," CVPR Workshops, 2019.
Tải từ https://commons.wikimedia.org/wiki/Natural_Image_Noise_Dataset (giấy phép CC BY-SA 4.0).
