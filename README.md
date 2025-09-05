# 🔋 Mạch Sạc Pin sử dụng IC S-8254A

Mạch được thiết kế bằng phần mềm **KiCad**, bao gồm đầy đủ sơ đồ nguyên lý, layout PCB.
---

---

## 🔧 Thông Số Kỹ Thuật

| Thông số             | Giá trị                            |
|----------------------|-------------------------------------|
| Cấu hình pin         | 3S (3 cell Li-ion nối tiếp)         |
| Loại cell            | 18650 Li-ion                        |
| Cấu hình đề xuất     | 3S2P (6 cell: 2 song song × 3 nối tiếp) |
| Điện áp danh định    | ~11.1V                              |
| Điện áp tối đa       | 12.6V                               |
| IC sử dụng           | S-8254A (ABLIC)                     |
| Điện áp hoạt động    | Tối đa ~13V                         |
| Dòng tải tối đa      | Phụ thuộc vào MOSFET được chọn      |
| Phần mềm thiết kế    | KiCad                               |

---

## 🧩 Sơ Đồ Mạch & Layout PCB

- ✅ Sơ đồ nguyên lý đầy đủ (file `.kicad_sch`)
<img width="1334" height="817" alt="image" src="https://github.com/user-attachments/assets/ce86611c-27be-406f-a3d7-c36cd3374fad" />


- ✅ Bo mạch 2 lớp (file `.kicad_pcb`)
  
- Top:
  
<img width="1334" height="622" alt="image" src="https://github.com/user-attachments/assets/3b4105f3-de26-4d24-a7c9-a1e4cfbd8a9d" />

- Bottom:
  
<img width="1328" height="620" alt="image" src="https://github.com/user-attachments/assets/ed4436ab-9840-4b36-ba64-9b75b3bb097b" />

- 3D model:
  
<img width="1349" height="627" alt="image" src="https://github.com/user-attachments/assets/782fbc37-371a-46e2-b214-8ed15703fc21" />



---

## 🧾 Danh Sách Linh Kiện (BOM)

| Linh kiện   | Giá trị     | Chức năng                         |
|-------------|-------------|-----------------------------------|
| IC          | S-8254A     | Điều khiển bảo vệ pin             |
| R_SENSE     | 10 mΩ       | Cảm biến dòng (shunt)             |
| R_VSS       | 51 Ω        | Lọc nguồn VDD                     |
| C_VSS       | 2.2 μF      | Tụ lọc nguồn                      |
| R_VC1-3     | 1 kΩ        | Điện trở cảm biến điện áp cell    |
| C_VC1-3     | 0.1 μF      | Tụ lọc nhiễu đầu vào VCx          |
| C_CCT       | 0.1 μF      | Trễ bật MOSFET sạc (~2s)          |
| C_CDT       | 0.1 μF      | Trễ bật MOSFET xả (~2s)           |
| R_COP       | 1 MΩ        | Điện trở cổng FET sạc             |
| R_DOP       | 5.1 kΩ      | Điện trở cổng FET xả              |
| R_VMP       | 5.1 kΩ      | Pull-up cho chân VMP              |
| R_CTL       | 1 kΩ        | Input cho điều khiển CTL          |
| R_SEL       | 100 kΩ      | Chọn chế độ hoạt động (Mode)      |
| MOSFET      | IRF8721 (hoặc tương đương) | Công tắc sạc/xả      |

---

## 🧮 Cách Tính Linh Kiện

### 1. Bộ lọc nguồn (VSS):

> Điều kiện: R_VSS × C_VSS ≥ 112 μF·Ω 
→ Dùng 51Ω và 2.2μF → đạt 112.2 µF·Ω ✅

---

### 2. Bộ lọc cảm biến điện áp (VC1–VC3):

> R_VC × C_VC = 1kΩ × 0.1μF = 100 µF·Ω 
→ Gần với khuyến nghị 112 µF·Ω → OK  
→ Có thể tăng C lên 0.22μF nếu muốn lọc nhiễu tốt hơn.

---

### 3. Tụ trễ ngắt:

| Tín hiệu | Tụ        | Tác dụng             |
|----------|-----------|----------------------|
| C_CCT    | 0.1 μF    | Trễ đóng MOSFET sạc  |
| C_CDT    | 0.1 μF    | Trễ đóng MOSFET xả   |

---

### 4. Điện trở cảm biến dòng (R_SENSE):

> Dùng để phát hiện quá dòng hoặc ngắn mạch.

Ví dụ:
- Dòng xả tối đa: **10 A**
- Ngưỡng phát hiện: **100 mV**

Công thức:

R_SENSE = V / I = 0.1V / 10A = 0.01Ω = 10 mΩ

→ Công suất hao phí:

P = I² × R = 10² × 0.01 = 1W → nên dùng R công suất ≥ 2W

---

## 🧠 Ghi chú về Chân SEL

- Chân SEL dùng để **chọn chế độ hoạt động nội bộ** của IC (ngưỡng bảo vệ, thời gian trễ...)
- Nếu bạn dùng IC phiên bản cố định (thường thấy khi mua lẻ), chỉ cần kéo chân SEL về GND bằng điện trở 100kΩ
- Nếu dùng loại cho phép chọn cấu hình, thì tạo mạch chia áp để thiết lập điện áp SEL theo yêu cầu datasheet

---


## 📘 Tài Liệu Tham Khảo

- [📄 Datasheet S-8254A – ABLIC](https://www.ablic.com/en/doc/datasheet/battery_protection/s8254_e.pdf)
