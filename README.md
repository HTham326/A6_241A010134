# 📱 LAB A6 – CONTROLS & VALIDATION | ĐĂNG KÝ HỌC PHẦN

> **INT4211 – Lập trình di động** · Android (Java + XML) · Lab A6  
> Ứng dụng biểu mẫu đăng ký học phần với kiểm tra dữ liệu, màn hình xác nhận và hai chức năng nâng cao.

## 📌 Mục lục

1. [Thông tin sinh viên và dự án](#-thông-tin-sinh-viên-và-dự-án)
2. [Giới thiệu](#-giới-thiệu)
3. [Chức năng chính](#-chức-năng-chính)
4. [Hai bài nâng cao](#-hai-bài-nâng-cao)
5. [Công nghệ và môi trường](#-công-nghệ-và-môi-trường)
6. [Cấu trúc dự án](#-cấu-trúc-dự-án)
7. [Vai trò các file chính](#-vai-trò-các-file-chính)
8. [Luồng xử lý và kiểm tra dữ liệu](#-luồng-xử-lý-và-kiểm-tra-dữ-liệu)
9. [Bảng đối chiếu Android ↔ Flutter F3](#-bảng-đối-chiếu-android--flutter-f3)
10. [Cài đặt và chạy ứng dụng](#-cài-đặt-và-chạy-ứng-dụng)
11. [Kiểm thử](#-kiểm-thử)
12. [Lịch sử Git và tài liệu](#-lịch-sử-git-và-tài-liệu)

## 👩‍🎓 Thông tin sinh viên và dự án

| Nội dung | Thông tin |
|---|---|
| Họ và tên | **Nguyễn Thị Hồng Thắm** |
| Mã số sinh viên | **241A010134** |
| Lớp | **CNTT – LTDD** |
| Trường | Đại học Văn Hiến |
| Môn học | Lập trình di động (INT4211) |
| Bài thực hành | **Lab A6 – Controls & Validation** |
| Tên project | `A6_241A010134` |
| Package / Application ID | `vn.edu.vhu.ltdd.a6controls` |
| Ngôn ngữ | Java |
| Giao diện | Android XML, Material Components |
| Repository | [HTham326/A6_241A010134](https://github.com/HTham326/A6_241A010134) |

## 📝 Giới thiệu

Lab A6 xây dựng ứng dụng **Đăng ký học phần** gồm hai màn hình:

- **Màn hình đăng ký (`MainActivity`)**: nhập thông tin sinh viên, chọn học phần, hệ đào tạo, buổi học, tùy chọn thông báo và ưu tiên.
- **Màn hình xác nhận (`ConfirmActivity`)**: hiển thị bản tóm tắt thông tin đã đăng ký và cho phép quay lại biểu mẫu để chỉnh sửa.

Bài thực hành giúp làm quen với các nhóm điều khiển nhập liệu và lựa chọn trong Android, lắng nghe sự kiện, kiểm tra hợp lệ của biểu mẫu, truyền dữ liệu giữa hai `Activity` bằng `Intent` và xử lý giao diện Material 3.

## ✨ Chức năng chính

| Thành phần | ID / phương thức | Chức năng |
|---|---|---|
| Ô nhập họ tên | `edtHoTen` | Nhập họ và tên sinh viên |
| Ô nhập MSSV | `edtMssv` | Nhập mã số sinh viên |
| Danh sách học phần | `spMonHoc` | Chọn một học phần từ `arrays.xml` qua `ArrayAdapter` |
| Nhóm hệ đào tạo | `rgHeDaoTao` | Chọn **Chính quy** hoặc **Vừa làm vừa học** |
| Ba ô chọn buổi học | `cbSang`, `cbChieu`, `cbToi` | Chọn một hoặc nhiều buổi học |
| Công tắc thông báo | `swThongBao` | Bật/tắt nhận thông báo qua email |
| Nút bật/tắt ưu tiên | `tgUuTien` | Chọn trạng thái ưu tiên |
| Xác nhận | `btnXacNhan` | Kiểm tra form và mở màn hình xác nhận nếu hợp lệ |
| Làm lại | `btnLamLai` | Xóa thông tin đã nhập và đưa các lựa chọn về mặc định |
| Quay lại chỉnh sửa | `btnQuayLai` | Đóng màn hình xác nhận bằng `finish()` |

**Danh sách học phần** được khai báo trong `res/values/arrays.xml`: Lập trình trên các thiết bị di động, Cơ sở dữ liệu, Trí tuệ nhân tạo và Kiểm thử phần mềm.

### Quy tắc kiểm tra dữ liệu

1. **Họ tên:** không được để trống; lỗi hiển thị ngay trên `EditText` bằng `setError()`.
2. **MSSV:** phải có độ dài **10 ký tự**; nếu sai, hiển thị `setError()`.
3. **Hệ đào tạo:** bắt buộc chọn một `RadioButton`; nếu chưa chọn, hiển thị `Toast`.
4. **Buổi học:** phải chọn ít nhất một trong ba `CheckBox`; nếu chưa chọn, hiển thị `Toast`.

> **Lưu ý về mã nguồn:** Phương thức kiểm tra MSSV hiện tại dùng `mssv.length() != 10`, tức là kiểm tra **độ dài 10 ký tự**, chưa kiểm tra tất cả ký tự có phải chữ số hay không. Đây là mô tả đúng với code trong project.

## 🚀 Hai bài nâng cao

### NC2 – Đếm số buổi đã chọn

Ứng dụng hiển thị `TextView` **`tvSoBuoi`** ngay dưới ba `CheckBox`, mặc định là **“Đã chọn: 0 buổi”**. Cả ba ô chọn sử dụng chung một `CompoundButton.OnCheckedChangeListener`. Khi một lựa chọn thay đổi, hàm `capNhatSoBuoi()` đếm lại số CheckBox đang được chọn rồi cập nhật nội dung hiển thị. Khi bấm **Làm lại**, số buổi trở về 0.

```java
CompoundButton.OnCheckedChangeListener buoiListener =
        (buttonView, isChecked) -> capNhatSoBuoi();
cbSang.setOnCheckedChangeListener(buoiListener);
cbChieu.setOnCheckedChangeListener(buoiListener);
cbToi.setOnCheckedChangeListener(buoiListener);
```

```java
private void capNhatSoBuoi() {
    int soBuoi = 0;
    if (cbSang.isChecked()) soBuoi++;
    if (cbChieu.isChecked()) soBuoi++;
    if (cbToi.isChecked()) soBuoi++;
    tvSoBuoi.setText("Đã chọn: " + soBuoi + " buổi");
}
```

**Kết quả:** bộ đếm cập nhật theo thao tác chọn/bỏ chọn, nhận giá trị từ **0 đến 3**.

### NC3 – Chuyển đổi chế độ sáng/tối

Ứng dụng bổ sung `MaterialSwitch` **`swDarkMode`** với nhãn **“Chế độ tối”**. Khi bật, chương trình gọi `AppCompatDelegate.setDefaultNightMode(MODE_NIGHT_YES)`; khi tắt, gọi `MODE_NIGHT_NO`. Theme dự án sử dụng `Theme.Material3.DayNight.NoActionBar` để hỗ trợ hai chế độ hiển thị.

```java
swDarkMode.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        AppCompatDelegate.setDefaultNightMode(
                AppCompatDelegate.MODE_NIGHT_YES);
    } else {
        AppCompatDelegate.setDefaultNightMode(
                AppCompatDelegate.MODE_NIGHT_NO);
    }
});
```

**Kết quả:** có thể thay đổi giao diện sáng/tối trực tiếp trên màn hình đăng ký. Trạng thái theme không bị đưa về mặc định trong hàm `lamLai()`.

## 🛠️ Công nghệ và môi trường

| Công nghệ / cấu hình | Giá trị trong project |
|---|---|
| Ngôn ngữ | Java (source/target compatibility Java 11) |
| Công cụ phát triển | Android Studio |
| Hệ thống build | Gradle Kotlin DSL (`.gradle.kts`) |
| Android Gradle Plugin | `9.4.1` |
| `compileSdk` | 37 |
| `targetSdk` | 37 |
| `minSdk` | 24 (Android 7.0 trở lên) |
| AndroidX AppCompat | `1.8.0` |
| Material Components | `1.14.0` |
| AndroidX Activity KTX | `1.13.0` |
| ConstraintLayout | `2.2.2` (khai báo dependency) |
| Giao diện | XML + Material 3 DayNight |
| Quản lý phiên bản | Git / GitHub (`master`) |

*Các phiên bản SDK, plugin và thư viện được đọc từ `app/build.gradle.kts` và `gradle/libs.versions.toml` của project đính kèm.*

## 📂 Cấu trúc dự án

```text
A6_241A010134/
├── app/
│   ├── build.gradle.kts
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/vn/edu/vhu/ltdd/a6controls/
│       │   ├── MainActivity.java
│       │   └── ConfirmActivity.java
│       └── res/
│           ├── layout/
│           │   ├── activity_main.xml
│           │   └── activity_confirm.xml
│           ├── values/
│           │   ├── arrays.xml
│           │   ├── strings.xml
│           │   ├── colors.xml
│           │   └── themes.xml
│           └── values-night/
│               └── themes.xml
├── gradle/
│   └── libs.versions.toml
├── build.gradle.kts
├── settings.gradle.kts
├── gradlew
├── gradlew.bat
└── .gitignore
```

*Chỉ hiển thị các file chính; đã lược bỏ thư mục sinh tự động, icon và một số resource phụ.*

## 🗂️ Vai trò các file chính

| File | Vai trò |
|---|---|
| `MainActivity.java` | Ánh xạ control, thiết lập listener, kiểm tra form, tạo bản tóm tắt, chuyển màn hình; xử lý NC2 và NC3 |
| `ConfirmActivity.java` | Nhận chuỗi tóm tắt từ `Intent`, hiển thị trên `tvTomTat`, xử lý nút quay lại |
| `activity_main.xml` | Bố cục biểu mẫu đăng ký với `ScrollView`, các control và nút thao tác |
| `activity_confirm.xml` | Giao diện màn hình xác nhận |
| `arrays.xml` | Danh sách học phần dùng cho `Spinner` |
| `strings.xml` | Tên ứng dụng, nhãn, thông báo lỗi, định dạng tóm tắt và chuỗi cho hai bài nâng cao |
| `themes.xml`, `values-night/themes.xml` | Khai báo theme Material 3 DayNight |
| `AndroidManifest.xml` | Khai báo hai Activity, đặt `MainActivity` là màn hình khởi chạy |
| `app/build.gradle.kts` | Cấu hình SDK, Java và các thư viện của module app |
| `gradle/libs.versions.toml` | Quản lý phiên bản plugin và dependencies |

## 🔄 Luồng xử lý và kiểm tra dữ liệu

```text
Mở ứng dụng
    ↓
MainActivity – Nhập thông tin và chọn các tùy chọn
    ↓
Nhấn "Xác nhận"
    ↓
Kiểm tra họ tên → MSSV → hệ đào tạo → buổi học
    ├── Có lỗi → setError() / Toast → ở lại biểu mẫu
    └── Hợp lệ
          ↓
    Tạo chuỗi tóm tắt (tom_tat_format)
          ↓
    Intent.putExtra(EXTRA_TOM_TAT, tomTat)
          ↓
    ConfirmActivity – Hiển thị thông tin
          ↓
    Nhấn "Quay lại chỉnh sửa" → finish() → MainActivity
```

**Các thành phần được đưa vào tóm tắt:** họ tên, MSSV, học phần, hệ đào tạo, các buổi học đã chọn, trạng thái nhận thông báo và trạng thái ưu tiên.

## 🔁 Bảng đối chiếu Android ↔ Flutter F3

| Việc cần làm | Android (Java + XML) – A6 | Flutter (Dart) – F3 |
|---|---|---|
| Ô nhập chữ | `EditText` + `setError()` | `TextFormField` + `validator` |
| Chọn nhiều | `CheckBox.isChecked()` | `CheckboxListTile` + biến `bool` |
| Chọn một (ít phương án) | `RadioGroup` + `RadioButton` | `RadioGroup` + `Radio` |
| Chọn một (danh sách dài) | `Spinner` + `ArrayAdapter` | `DropdownButtonFormField` |
| Bật/tắt | `MaterialSwitch` | `SwitchListTile` |
| Kiểm tra cả form | Các câu lệnh `if` kiểm tra từng điều kiện | `Form` + `_formKey.currentState!.validate()` |

**Nhận xét:** Android tách bố cục XML và logic Java, đồng thời cho phép kiểm soát từng bước xác thực bằng các điều kiện riêng. Flutter tổ chức giao diện bằng widget và có thể gom việc xác thực vào `Form` và `validator`, giúp quản lý biểu mẫu tập trung hơn. Cả hai đều hỗ trợ xây dựng giao diện nhập liệu và lựa chọn.

> Bảng này đối chiếu theo **yêu cầu của tài liệu Lab A6**, không khẳng định đã kiểm chứng từng dòng mã nguồn của project F3 (không nằm trong ZIP A6 đính kèm).

## ▶️ Cài đặt và chạy ứng dụng

1. Clone repository:

   ```bash
   git clone https://github.com/HTham326/A6_241A010134.git
   ```

2. Mở thư mục `A6_241A010134` bằng **Android Studio** và chờ Gradle Sync hoàn tất.
3. Đảm bảo Android SDK phù hợp với cấu hình project (compile SDK 37) và công cụ build được cài đặt.
4. Khởi động Android Emulator hoặc kết nối điện thoại Android **API 24 trở lên** đã bật USB debugging.
5. Chọn cấu hình chạy `app`, sau đó nhấn **Run ▶**.

**Chạy bằng Terminal trên Windows:**

```powershell
.\gradlew.bat assembleDebug
```

APK debug sau khi build thành công thường nằm tại `app/build/outputs/apk/debug/app-debug.apk`.

## 🧪 Kiểm thử

| STT | Tình huống | Kết quả mong đợi |
|---|---|---|
| 1 | Mở ứng dụng | Hiển thị form và các control |
| 2 | Để trống họ tên rồi xác nhận | `EditText` họ tên hiển thị lỗi |
| 3 | Nhập MSSV khác 10 ký tự | `EditText` MSSV hiển thị lỗi |
| 4 | Chưa chọn hệ đào tạo | Hiển thị thông báo yêu cầu chọn hệ đào tạo |
| 5 | Chưa chọn buổi học | Hiển thị thông báo yêu cầu chọn ít nhất một buổi |
| 6 | Điền đủ thông tin hợp lệ, xác nhận | Chuyển sang màn hình tóm tắt với dữ liệu đã chọn |
| 7 | Xoay màn hình khi đang nhập dở | Kiểm tra trạng thái các trường sau khi Activity được tạo lại |
| 8 | Bấm Back hoặc "Quay lại chỉnh sửa" từ màn hình xác nhận | Trở về form đăng ký |
| NC2 | Chọn/bỏ chọn các buổi học | `tvSoBuoi` cập nhật số buổi 0–3 |
| NC3 | Bật/tắt "Chế độ tối" | Giao diện chuyển giữa sáng và tối |

*Bảng trên là danh sách kịch bản và kết quả mong đợi theo logic mã nguồn; không thay thế bảng kết quả quan sát và ảnh chụp thực tế trong báo cáo.*

## 🧾 Lịch sử Git và tài liệu

Các commit chính trong quá trình hoàn thiện Lab A6:

```text
003909f A6: hoan thien man hinh xac nhan
499a21a A6: NC3 - Them switch Che do toi
f121082 A6: NC2 - Dem so buoi da chon
53746d4 A6: hoan thanh chuc nang co ban
eb1cc3e A6: khoi tao - xac nhan giao dien
```

**Báo cáo thực hành:** `A6_241A010134_BAOCAO.docx` (nộp theo yêu cầu môn học).  
**Mã nguồn:** [GitHub – A6_241A010134](https://github.com/HTham326/A6_241A010134.git).

---

**Thực hiện:** Nguyễn Thị Hồng Thắm · **MSSV:** 241A010134 · **Môn:** Lập trình di động – INT4211.
