# Bài tập lớn Hệ điều hành — Đề tài 9: Giải thuật Banker

Mô phỏng giải thuật Banker (deadlock avoidance) bằng phần mềm có giao diện, kèm quyển báo cáo.

| | |
|---|---|
| **Phần** | Deadlock |
| **Đề tài** | Giải thuật Banker |
| **Đối chứng so sánh** | Giải thuật đồ thị phân bổ tài nguyên (đề tài 10) |
| **Sản phẩm** | Quyển báo cáo + phần mềm có giao diện |
| **Nhân lực** | 8 thành viên · 4 tuần |

Kế hoạch phân công đầy đủ: [`docs/Phan-cong-De-tai-9-Giai-thuat-Banker.pdf`](docs/Phan-cong-De-tai-9-Giai-thuat-Banker.pdf)

---

## Tiến độ

| TV | Họ tên | Nhiệm vụ | Nhánh | Trạng thái |
|---|---|---|---|---|
| TV1 | Lê Minh Tài | Nhóm trưởng · hợp đồng dữ liệu · kiến trúc · Chương 4 | `tv1-kien-truc` | ✅ Xong |
| TV2 | Nguyễn Đức Khải | Chương 1 — lý thuyết Deadlock | `tv2-chuong-ly-thuyet` | ✅ Xong |
| TV3 | Đặng Hoàng Phong | Chương 2 — đặc tả thuật toán | `tv3-dac-ta-thuat-toan` | ✅ Xong |
| TV4 | Nguyễn Minh Trí | Engine — mã nguồn lõi | `tv4-engine` | ✅ Xong |
| TV5 | Trần Đức Tân | Giao diện — nhập liệu và dữ liệu | `tv5-gui-nhap-lieu` | ✅ Xong |
| TV6 | Nguyễn Thái | Giao diện — mô phỏng và trực quan | `tv6-gui-mo-phong` | ✅ Xong |
| TV7 | Văn Hoàng | Chương 3 — so sánh hai giải thuật | `tv7-chuong-so-sanh` | ✅ Xong |
| TV8 | Võ Nguyên Thảo | Chương 5 — kiểm thử và kết quả | `tv8-kiem-thu` | ✅ Xong |

**Đủ 5 chương và phần mềm đã có trên `main`.** Bộ kiểm thử: **22/22 ca đạt.**

---

## Hướng dẫn demo

Phần này dành cho buổi bảo vệ. Làm đúng thứ tự, mỗi bước nói một câu.

### Chuẩn bị — làm trước ở nhà, đừng làm trước mặt thầy

```bash
git clone https://github.com/BryannLee202/HDH_De_tai_9_Giai_thuat_Banker.git
```

```bash
pip install PyQt5 matplotlib openpyxl python-docx
```

Chạy thử một lượt cho chắc, và để lần tải thư viện đầu tiên diễn ra trước buổi demo.

### Bước 1 — Chứng minh thuật toán đúng

```bash
python -m unittest discover -s tests -v
```

Kết quả: **22/22 ca đạt**.

Nói: *"Bộ kiểm thử có 22 ca, giá trị kỳ vọng lấy từ bảng chạy tay ở Chương 2 chứ không suy đoán. Người viết ca kiểm thử là TV8, người viết mã cài đặt là TV4 — hai người khác nhau."*

### Bước 2 — Chạy engine không cần giao diện

```bash
python -m engine.demo
```

Kết quả: **AN TOÀN**, chuỗi `P1 → P3 → P0 → P2 → P4`, `Work` cuối `(10, 5, 7)`.

Nói: *"Engine không import thư viện đồ hoạ nào nên chạy được từ dòng lệnh — đó là lý do kiểm thử tự động được."*

### Bước 3 — Mở phần mềm (phần chính)

```bash
python -m gui.GUI_TV6
```

Thao tác theo đúng thứ tự này:

| Thao tác | Kết quả cần chỉ ra |
|---|---|
| Bấm **Kiểm tra & Tính Need** | Bảng Need tự sinh, khoá không cho sửa tay |
| Bấm **Chạy Từng Bước** 5 lần | Nhật ký đủ 5 vòng, badge xanh báo AN TOÀN kèm chuỗi |
| Chọn `P1`, nhập `1 0 2`, **Gửi yêu cầu** | Nền xanh — **CẤP PHÁT** |
| Chọn `P4`, nhập `3 3 0`, **Gửi yêu cầu** | Nền cam — **CHỜ**, thiếu tài nguyên rảnh |
| Chọn `P2`, nhập `7 0 0`, **Gửi yêu cầu** | Nền đỏ — **LỖI**, vượt quá Need |
| **Kết thúc tiến trình** rồi **Hoàn tác** | Available đổi rồi quay lại như cũ |

**Câu đắt giá nhất khi demo:** sau khi đã cấp cho `P1`, thử cho `P0` xin `(0 2 0)`. Hệ thống **còn đủ tài nguyên rảnh** nhưng vẫn từ chối, vì cấp vào thì trạng thái mất an toàn. Đây chính là điểm phân biệt *tránh deadlock* với *cấp phát theo tài nguyên còn rảnh*.

### Bước 4 — Số liệu so sánh cho Chương 3

```bash
python -m scripts.so_sanh_hai_giai_thuat
```

Script đo cả hai giải thuật, sinh CSV và 4 hình. Nói: *"Số liệu trong Chương 3 do chính script này đo, hạt giống ngẫu nhiên cố định nên chạy lại ra đúng bảng trong báo cáo."*

Điều đáng chỉ ra ở cuối màn hình: trên bộ dữ liệu chuẩn, **giải thuật đồ thị báo có chu trình nhưng Banker báo an toàn** — chứng minh chu trình chỉ là *điều kiện cần* khi tài nguyên có nhiều thực thể.

### Nếu máy trục trặc

Có sẵn ảnh chụp trong `docs/anh-demo/`. Mở `docs/anh-demo/giao-dien-tong-quan.png` rồi trình bày tiếp bằng ảnh.

### Bốn câu hỏi hay bị hỏi nhất

**Vì sao trạng thái không an toàn chưa chắc là deadlock?**
Không an toàn nghĩa là hệ điều hành không còn *bảo đảm được* mọi tiến trình hoàn tất trong tình huống xấu nhất. Thực tế các tiến trình có thể không xin hết `Max` và vẫn chạy trót lọt. Banker chọn cách thận trọng: từ chối luôn.

**Vì sao hệ điều hành thật không dùng Banker?**
Phải biết trước nhu cầu tối đa của mọi tiến trình — điều gần như không có trong thực tế. Ngoài ra cấp phát quá thận trọng làm giảm hiệu suất. Windows và Linux dùng thuật toán đà điểu: bỏ qua.

**Chuỗi an toàn có duy nhất không?**
Không. Với ví dụ chuẩn có **16 chuỗi hợp lệ**, chương trình liệt kê hết được bằng `tat_ca_chuoi_an_toan()`. Chương trình chọn chuỗi đầu tiên tìm được khi duyệt theo thứ tự chỉ số tăng dần.

**Vì sao độ phức tạp là `O(m·n²)` chứ không phải `O(n²)`?**
Vòng ngoài lặp tối đa `n` lần, mỗi lần quét `n` tiến trình, và mỗi phép so sánh vector tốn `m` phép tính. Lưu ý đây là **cận trên chặt**, không phải hành vi trung bình — số đo thực tế trên dữ liệu ngẫu nhiên tăng gần tuyến tính, chỉ trường hợp xấu nhất mới đúng bậc hai.


## Chạy chương trình

Cài thư viện giao diện (chỉ TV5, TV6 cần):

```bash
pip install PyQt5
```

Chạy phần mềm:

```bash
python -m gui.GUI_TV6
```

Chạy engine từ dòng lệnh, không cần giao diện:

```bash
python -m engine.demo
```

Kết quả mong đợi với bộ dữ liệu chuẩn: **AN TOÀN**, chuỗi `P1 → P3 → P0 → P2 → P4`, `Work` cuối `(10, 5, 7)`.

Chạy kiểm thử:

```bash
python -m unittest discover -s tests -v
```

Đo hiệu năng, xuất CSV cho TV7:

```bash
python -m scripts.do_hieu_nang
```

---

## API của engine — đọc trước khi viết bất kỳ dòng nào

> **Quan trọng nhất trong README này.** Đã từng có ba bộ tên hàm khác nhau cùng tồn tại trong dự án, khiến giao diện gọi một engine không hề tồn tại và chương trình bung ngay khi khởi động. Giờ tất cả đi qua **một API duy nhất**. Đừng tự tạo module engine khác.

```python
from engine.banker_types import BankerState, Verdict, LoiDuLieu
from engine.banker import (
    kiem_tra_hop_le,        # ném LoiDuLieu nếu dữ liệu sai
    kiem_tra_an_toan,       # -> SafetyResult
    yeu_cau_tai_nguyen,     # -> RequestResult
    giai_phong,
    ket_thuc_tien_trinh,
    tat_ca_chuoi_an_toan,
)

tt = BankerState.from_json_file("data/vi-du-chuan.json")
kq = kiem_tra_an_toan(tt)
print(kq.an_toan, kq.chuoi_dep())      # True  'P1 → P3 → P0 → P2 → P4'

r = yeu_cau_tai_nguyen(tt, 1, [1, 0, 2])
print(r.ket_luan, r.ly_do)             # Verdict.CAP_PHAT  'Cấp phát: ...'
```

**Kiểu dữ liệu trả về**

| Lớp | Trường |
|---|---|
| `BankerState` | `available`, `max`, `allocation`, `need` (tự tính), `total`, `n`, `m`, `copy()` |
| `SafetyResult` | `an_toan`, `chuoi`, `nhat_ky` (danh sách `StepLog`), `treo`, `chuoi_dep()` |
| `StepLog` | `buoc`, `work_truoc`, `tien_trinh`, `need`, `allocation`, `work_sau`, `bo_qua` |
| `RequestResult` | `ket_luan` (`Verdict`), `ly_do`, `nhat_ky` |
| `Verdict` | `CAP_PHAT`, `CHO`, `LOI` |

**Hai điều đừng làm**

- Đừng tự tính `Need` rồi lưu lại. `BankerState.need` tự tính từ `max − allocation` mỗi lần đọc nên không bao giờ lệch.
- Đừng import PyQt hay tkinter vào `engine/`. Engine phải chạy được từ dòng lệnh để TV8 kiểm thử tự động.

Giao diện không gọi thẳng engine mà đi qua [`gui/engine_adapter.py`](gui/engine_adapter.py) — nếu engine đổi thì chỉ phải sửa một chỗ.

---

## Cấu trúc thư mục

```
engine/    Thuật toán, không phụ thuộc giao diện
  banker_types.py      Hợp đồng dữ liệu dùng chung             TV1
  banker.py            Giải thuật Banker                       TV4
  do_thi_phan_bo.py    Giải thuật đồ thị (đối chứng Chương 3)  TV7
  demo.py              Chạy thử từ dòng lệnh                   TV4
gui/       Giao diện phần mềm
  GUI_TV6.py           Cửa sổ chính, nhập liệu, mô phỏng       TV5, TV6
  bang_yeu_cau.py      Yêu cầu, giải phóng, hoàn tác, biểu đồ  TV6
  engine_adapter.py    Cầu nối giữa engine và giao diện        TV6
  xuat_bao_cao.py      Xuất kết quả ra HTML / Excel            TV1
scripts/   Công cụ sinh số liệu và tài liệu
  do_hieu_nang.py           Đo Banker, trung bình và xấu nhất  TV4
  so_sanh_hai_giai_thuat.py Đo 2 giải thuật, vẽ 4 hình Ch.3    TV7
  tao_chuong_4_5.py         Sinh Chương 4 và Chương 5          TV1
tests/     22 ca kiểm thử tự động                              TV8
data/      3 bộ dữ liệu mẫu .json                              TV5
docs/      Đề bài, kế hoạch, kiến trúc, ảnh demo               TV1
report/    Quyển báo cáo — 5 chương, mỗi chương một thư mục
```

**Quy tắc quan trọng nhất:** mỗi thư mục có đúng một chủ sở hữu, và **chỉ chủ sở hữu được sửa file bên trong**.

File Word là file nhị phân — Git không merge được. Hai người cùng sửa một file `.docx` sẽ tạo ra xung đột không gỡ được, buộc phải bỏ hẳn một bản.

---

## Bắt đầu làm việc

### Bước 1 — Khai báo danh tính (làm một lần, trên máy của chính mình)

Email phải **trùng với email đăng ký GitHub của bạn**. Nếu sai, commit không gắn được vào tài khoản và biểu đồ đóng góp sẽ trống — đúng thứ giáo viên nhìn để đánh giá.

```bash
git config --global user.name "Họ Tên Thật"
```

```bash
git config --global user.email "email-github-cua-ban@gmail.com"
```

### Bước 2 — Tải repo về máy

```bash
git clone https://github.com/BryannLee202/HDH_De_tai_9_Giai_thuat_Banker.git
```

### Bước 3 — Sang nhánh của mình

Tám nhánh đã tạo sẵn nên **không dùng `-b`**. Repo cũng đang chặn tạo nhánh mới, gõ `git checkout -b` sẽ bị từ chối — đó không phải lỗi Git.

```bash
git checkout tv4-engine
```

### Bước 4 — Lấy bản mới nhất trước khi làm

```bash
git pull origin main
```

### Bước 5 — Làm việc rồi lưu lên

```bash
git add . && git commit -m "TV4: cai dat ham kiem_tra_an_toan" && git push
```

### Bước 6 — Tạo pull request

Vào GitHub bấm **Compare & pull request**, gán TV1 duyệt. **Không ai đẩy thẳng vào `main`** — repo đã bật ràng buộc chặn.

---

## Nếu bạn không code (TV2, TV3, TV7)

Không cần dùng dòng lệnh. Chọn một trong hai cách:

- **GitHub Desktop** — cài đặt, đăng nhập, kéo file vào rồi bấm nút.
- **Ngay trên web GitHub** — đổi nhánh sang nhánh của mình, mở đúng thư mục chương, bấm **Add file → Upload files**, kéo file `.docx` vào, gõ mô tả rồi **Commit changes**.

Cả hai cách đều tính là commit của bạn, miễn là đăng nhập bằng tài khoản của chính mình.

**Đừng nộp bằng file `.zip`.** GitHub không diff được file nén nên không ai review từng dòng hay góp ý được, và `.gitignore` đã chặn sẵn `*.zip`.

---

## Quy ước commit

Mở đầu bằng mã thành viên, viết không dấu để tránh lỗi phông trên máy khác:

```
TV3: bo sung vi du chay tay 5 vong lap
TV5: chan du lieu khi Allocation vuot qua Max
TV8: them 5 ca kiem thu bien
```

**Mỗi người tối thiểu 5 commit, rải đều 4 tuần.** Commit dồn hết vào đêm trước hạn nộp thì nhìn lịch sử là biết ngay.

---

## Quy tắc chung

- **Không ai đẩy thẳng vào `main`**, tất cả qua pull request. Chỉ TV1 được bấm Merge.
- **Không tạo nhánh mới** — 8 nhánh đã đủ, repo đang chặn.
- **Không dùng `git push --force`.** Đây là lệnh duy nhất có thể xoá vĩnh viễn lịch sử. Repo có chặn nhưng đừng tập thói quen đó.
- **Không commit video demo** — GitHub chặn file trên 100 MB. Để trên Google Drive rồi dán link vào mục cuối README.
- **Repo đang ở chế độ public.** Nhóm khác đọc được, nên **đừng đẩy quyển báo cáo hoàn chỉnh lên trước khi nộp**. Code và khung thư mục thì công khai không sao.
- Họp 2 buổi mỗi tuần, biên bản lưu ở `report/bien-ban-hop/`.
- Hạn nội bộ sớm hơn hạn nộp 3 ngày.

---

## Việc còn lại

Cả 8 phần việc chuyên môn đã xong. Ba việc cuối trước khi nộp:

**Ghép quyển — TV1.** Gộp 5 chương thành một file, thêm bìa, lời nói đầu, mục lục tự động, danh mục hình và bảng, bảng phân công, kết luận, tài liệu tham khảo, phụ lục mã nguồn. Chương 4 và Chương 5 sinh sẵn theo đúng định dạng Times New Roman 13, giãn dòng 1.5, lề 3-2-2-2.

**Video demo — TV6.** Quay 3–5 phút theo đúng thứ tự ở mục Hướng dẫn demo. Để trên Google Drive rồi dán link vào cuối README, đừng commit vào repo.

**Buổi hỏi chéo — TV8.** Bốc thăm câu hỏi cho cả nhóm, mục tiêu là ai cũng trả lời được câu ngoài phần mình phụ trách.

## Tài liệu học theo vai trò

### Dùng chung cho cả 8 người

| Tài liệu | Ở đâu |
|---|---|
| Kế hoạch phân công đầy đủ | `docs/Phan-cong-De-tai-9-Giai-thuat-Banker.pdf` — **mục 03 là phần ai cũng phải nắm** |
| Kiến trúc chương trình, 4 biểu đồ | [`docs/kien-truc.md`](docs/kien-truc.md) — GitHub tự vẽ ra hình, chụp màn hình chèn thẳng vào Word |
| Ảnh đề bài gốc | `docs/de-bai/de-bai-goc.jpg` |
| Silberschatz — *Operating System Concepts*, chương **Deadlocks** | https://www.os-book.com |
| Slide bài giảng của thầy | Nguồn sát đề nhất. Nếu ký hiệu khác sách thì **theo slide**, vì thầy chấm theo đó |

### Theo vai trò

- **TV1** — [Pull request](https://docs.github.com/en/pull-requests) · [Vẽ UML](https://app.diagrams.net) · [openpyxl](https://openpyxl.readthedocs.io) · [PyInstaller](https://pyinstaller.org/en/stable/)
- **TV2** — Silberschatz các mục đầu chương Deadlocks. Bài toán 5 triết gia nằm ở chương **Synchronization**, không phải chương Deadlocks
- **TV3** — Mã giả đã có sẵn trong chú thích của `engine/banker.py`; kết quả kỳ vọng nằm trong `tests/test_banker.py`
- **TV4** — [dataclass](https://docs.python.org/3/library/dataclasses.html) · [sao chép sâu](https://docs.python.org/3/library/copy.html) · [unittest](https://docs.python.org/3/library/unittest.html)
- **TV5** — [PyQt5](https://www.riverbankcomputing.com/static/Docs/PyQt5/) · [QTableWidget](https://doc.qt.io/qt-5/qtablewidget.html) · [JSON](https://docs.python.org/3/library/json.html)
- **TV6** — [QTimer](https://doc.qt.io/qt-5/qtimer.html) · [QSlider](https://doc.qt.io/qt-5/qslider.html) · lớp `StepLog` chính là cấu trúc cột của bảng nhật ký
- **TV7** — Silberschatz mục **Resource-Allocation Graph** · phát hiện chu trình trong đồ thị có hướng
- **TV8** — [unittest](https://docs.python.org/3/library/unittest.html) · [GitHub Issues](https://docs.github.com/en/issues)

---

## Video demo

*(TV6 dán link Google Drive vào đây)*
