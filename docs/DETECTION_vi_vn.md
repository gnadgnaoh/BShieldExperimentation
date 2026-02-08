# Những kiểm tra / lỗ hổng mà BShield có thể kiểm tra trên Android

Tài liệu này liệt kê tất cả các cơ chế phát hiện đã được quan sát trong BShield trên Android. Thông tin chính xác tính đến ngày 8 tháng 2 năm 2026. Nếu bạn phát hiện thêm các cơ chế phát hiện khác, hãy thoải mái báo cáo trong tab Issues.

> [!CAUTION]
> **Dự án này chỉ phục vụ mục đích giáo dục. Mục tiêu là làm nổi bật những điểm yếu của các giải pháp bảo mật hiện tại và khuyến khích phát triển những giải pháp thay thế tốt hơn, đáng tin cậy hơn. Hãy sử dụng thông tin này một cách có trách nhiệm. KHÔNG sử dụng cho mục đích độc hại. Tôi không chịu trách nhiệm cho bất kỳ hành động nào được thực hiện bởi người dùng của module hoặc dự án này.**

**Mục lục:**

- [Những kiểm tra / lỗ hổng mà BShield có thể kiểm tra trên Android](#những-kiểm-tra--lỗ-hổng-mà-bshield-có-thể-kiểm-tra-trên-android)
  - [Mã lỗi 1 (Phát hiện ứng dụng bị chỉnh sửa)](#mã-lỗi-1-phát-hiện-ứng-dụng-bị-chỉnh-sửa)
  - [Mã lỗi 2 (Phát hiện máy ảo)](#mã-lỗi-2-phát-hiện-máy-ảo)
  - [Mã lỗi 3 (Phát hiện danh sách ứng dụng)](#mã-lỗi-3-phát-hiện-danh-sách-ứng-dụng)
  - [Mã lỗi 4 (Phát hiện công cụ debug, hiếm)](#mã-lỗi-4-phát-hiện-công-cụ-debug-hiếm)
  - [Mã lỗi 5 (Phát hiện root)](#mã-lỗi-5-phát-hiện-root)
    - [Thuộc tính hệ thống](#thuộc-tính-hệ-thống)
    - [Phát hiện qua map bộ nhớ](#phát-hiện-qua-map-bộ-nhớ)
    - [Trạng thái Enforcing](#trạng-thái-enforcing)
    - [Phát hiện tên gói](#phát-hiện-tên-gói)
    - [Rò rỉ từ launcher tùy chỉnh](#rò-rỉ-từ-launcher-tùy-chỉnh)
    - [Kiểm tra bootloader](#kiểm-tra-bootloader)
    - [Mount đáng ngờ](#mount-đáng-ngờ)
    - [\[CHƯA XÁC NHẬN\] Phát hiện vòng lặp image proc của module KSU/AP](#chưa-xác-nhận-phát-hiện-vòng-lặp-image-proc-của-module-ksuap)
  - [Mã lỗi 6 (Phát hiện bootloader mở khóa, không sử dụng)](#mã-lỗi-6-phát-hiện-bootloader-mở-khóa-không-sử-dụng)
  - [Mã lỗi 7 (Phát hiện ứng dụng, hiếm)](#mã-lỗi-7-phát-hiện-ứng-dụng-hiếm)
  - [Mã lỗi 8 (Phát hiện ứng dụng dạng Privacy Space)](#mã-lỗi-8-phát-hiện-ứng-dụng-dạng-privacy-space)
  - [Mã lỗi 10 (Phát hiện chế độ debug ADB)](#mã-lỗi-10-phát-hiện-chế-độ-debug-adb)
  - [Mã lỗi 11 (Phát hiện chế độ nhà phát triển)](#mã-lỗi-11-phát-hiện-chế-độ-nhà-phát-triển)

## Mã lỗi 1 (Phát hiện ứng dụng bị chỉnh sửa)

**Link tham khảo:** <https://vneid.gov.vn/shield-warning?code=1>

Lỗi này xảy ra khi bạn cài đặt ứng dụng chưa được ký hoặc ứng dụng đã bị chỉnh sửa. Đối với các nhà phát triển ứng dụng patch, hiện tại tôi không tìm ra cách nào để làm cho nó hoạt động. Bạn có thể thử, nhưng sẽ rất khó để thành công.

**Cách khắc phục:**  
Gỡ bỏ ứng dụng đã bị chỉnh sửa hoặc chưa ký khỏi hệ thống và cài đặt lại từ Google Play.

## Mã lỗi 2 (Phát hiện máy ảo)

**Link tham khảo:** <https://vneid.gov.vn/shield-warning?code=2>

Lỗi này xảy ra khi bạn cài đặt ứng dụng trong máy ảo.

**Cách khắc phục:**  
Không cài đặt ứng dụng trong máy ảo (hiển nhiên :v).

## Mã lỗi 3 (Phát hiện danh sách ứng dụng)

**Link tham khảo:** <https://vneid.gov.vn/shield-warning?code=3>

Lỗi này xảy ra khi bạn cài đặt trình quản lý root hoặc các ứng dụng đáng ngờ trên thiết bị.

Dưới đây là danh sách các ứng dụng mà BShield hiện đang phát hiện (có thể còn nhiều hơn; đây chỉ là những ứng dụng đã được xác nhận qua thử nghiệm. Bạn có thể yêu cầu cập nhật trong tab Issues):

```txt
com.topjohnwu.magisk
com.drdisagree.iconify
<phần lớn các ứng dụng lsposed>
```

**Cách khắc phục:**
Bạn có thể sử dụng tổ hợp:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

để ẩn các ứng dụng này.

Hoặc nếu bạn không dùng root, đơn giản là đừng cài đặt trình quản lý root trên thiết bị.

## Mã lỗi 4 (Phát hiện công cụ debug, hiếm)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=4](https://vneid.gov.vn/shield-warning?code=4)

Lỗi này xảy ra khi bạn chạy ứng dụng bằng các công cụ debug của Google. Lỗi này sẽ không xuất hiện trong bản build production của ứng dụng. Nếu bạn gặp lỗi này, hãy liên hệ với nhà phát triển ứng dụng hoặc báo cho tôi biết.

## Mã lỗi 5 (Phát hiện root)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=5](https://vneid.gov.vn/shield-warning?code=5)

Đây là cơ chế phát hiện khó vượt qua nhất của BShield khi bạn sử dụng root, bao gồm nhiều dạng phát hiện root và hệ thống khác nhau. Dưới đây là một số cơ chế đã được xác nhận:

### Thuộc tính hệ thống

BShield phát hiện một số thuộc tính hệ thống Android nhất định. Một số ví dụ đã biết:

- `init.svc.adb_root`
- `service.adb.root`

**Cách khắc phục:**
Các thuộc tính này có thể được ẩn bằng cách override, ví dụ:

```sh
resetprop -n -p init.svc.adb_root ""
resetprop -n -p service.adb.root ""
```

**Lưu ý:** Các thuộc tính này sẽ bị reset sau khi reboot. Bạn có thể sử dụng module ví dụ của tôi trong tab Release để tự động fix sau mỗi lần khởi động.

### Phát hiện qua map bộ nhớ

BShield có thể phát hiện map bộ nhớ có chứa dấu vết của các entry liên quan đến injection hay không.

Bạn có thể kiểm tra bằng công cụ **Native Detector** ([tải tại đây](https://github.com/reveny/Android-Native-Root-Detector/releases/latest)).

Ví dụ, nó có thể báo lỗi "Injection Detection".

**Một số cách khắc phục:**

- Nếu kernel của bạn hỗ trợ KernelSU + SuSFS (bật SUS_MAP), bạn có thể thêm các đường dẫn map bị rò rỉ vào danh sách map của SuSFS.
- Nếu bạn dùng module font, nó cũng có thể làm rò rỉ map. Hãy gỡ bỏ hoặc thêm đường dẫn của nó vào SUS_MAP như trên.
- Bạn cũng có thể thử module TreatWheel của Pedro để ẩn maps, nhưng hiệu quả có hạn và yêu cầu build ReZygisk.

**Lưu ý về phát hiện maps của /system/framework/framework-res.apk.**

Bạn có thể thấy trong công cụ **Native Detector** báo **Found Injection**, với kết quả tương tự như sau:

```txt
7982fef000-7983031000 r--s 00000000 fd:00 1549 /system/framework/framework-res.apk
7988432000-7988448000 r--s 00344000 fd:01 1631 /system/framework/framework-res.apk
7988538000-7988546000 r--s 00000000 fd:03 3165 /system/framework/framework-res.apk
798bf3d000-798bf3e000 r--s 00055000 fd:00 1549 /system/framework/framework-res.apk
798bf3e000-798bf3f000 r--s 00000000 fd:03 3166 /system/framework/framework-res.apk
798bf4d000-798bf4e000 r--s 00002000 fd:03 3166 /system/framework/framework-res.apk
798bf4e000-798bf4f000 r--s 00000000 fd:27 518  /system/framework/framework-res.apk
7992736000-799273c000 r--s 0035d000 fd:01 1631 /system/framework/framework-res.apk
7992826000-7992827000 r--s 00000000 fd:27 503  /system/framework/framework-res.apk
7992976000-799297c000 r--s 00013000 fd:03 3165 /system/framework/framework-res.apk
79929ef000-79929f0000 r--s 00000000 fd:27 500  /system/framework/framework-res.apk
```

Điều này xảy ra vì kernel tùy chỉnh của bạn có thể đã chứa bản vá ẩn file LineageOS trong `task_mmu.c`. Xem [commit tham khảo (MoonWake@bea4fe4)](https://github.com/RainyXeon/moonwake_kernel_xiaomi_ruby/commit/bea4fe4ecfa41edb52f26ce9254a16643dda57ea).

Mục đích của bản vá này là thay thế đường dẫn file LineageOS thật bằng `framework-res.apk`. Nếu một file được map bởi VMA có chứa `lineage` trong tên, thì `/proc/<pid>/map_files/<start>-<end>` sẽ trỏ tới `framework-res.apk` thay vì file thật, nhằm tránh bị các công cụ kiểm tra root phát hiện.

Tuy nhiên, cơ chế này đã lỗi thời và vô tình kích hoạt **Found Injection** trong **Native Detector**, do header VMA giả vẫn có thể bị phát hiện. Có thể vì chỉ thay thế đường dẫn file mà không thay thế toàn bộ metadata của VMA.

Nếu bạn là nhà phát triển kernel tùy chỉnh, bạn có thể revert commit chứa đoạn code ẩn file LineageOS. Nếu bạn là người dùng, bạn không thể làm gì trừ khi thay kernel hoặc yêu cầu nhà phát triển kernel sửa đổi.

### Trạng thái Enforcing

Đây là dạng phát hiện phổ biến. Không nên sử dụng ROM tùy chỉnh với **SELinux permissive**, vì bị xem là không an toàn theo tiêu chuẩn hiện đại.

BShield yêu cầu **SELinux** phải ở trạng thái **Enforced** để hoạt động bình thường.

**Cách khắc phục:**

- Đặt SELinux sang **Enforcing**

```sh
setenforce 1
```

- Sử dụng kernel hoặc ROM với **SELinux Enforcing**

### Phát hiện tên gói

BShield kiểm tra danh sách ứng dụng đã cài để tìm các ứng dụng thường liên quan đến root. Phát hiện gốc nằm ở [mã lỗi 3](#mã-lỗi-3-phát-hiện-danh-sách-ứng-dụng), nhưng vì lý do nào đó, các app như `me.bmax.apatch` hoặc `com.rifsxd.ksunext` sẽ kích hoạt mã lỗi 5 thay vì 3. Có thể đây là cách tạm thời để phát hiện các giải pháp root nâng cao như KernelSU.

Danh sách ứng dụng bị phát hiện:

```txt
com.topjohnwu.magisk
com.drdisagree.iconify
com.rifsxd.ksunext
me.bmax.apatch
me.weishu.kernelsu
```

**Cách khắc phục:**
Sử dụng kết hợp:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

để ẩn các ứng dụng này.

### Rò rỉ từ launcher tùy chỉnh

BShield có thể phát hiện nhiều module launcher tùy chỉnh, có thể thông qua mount, memory maps hoặc các chỉ dấu khác.

**Cách khắc phục:**
Cách đơn giản nhất là gỡ launcher tùy chỉnh và dùng launcher mặc định của hệ thống. Hoặc dùng launcher tiêu chuẩn từ Play Store.

### Kiểm tra bootloader

Tôi có thể xác nhận rằng BShield đã kiểm tra bootloader vào đầu năm 2026. Nó chỉ kiểm tra trạng thái khóa và AOSP keybox; key attestation bị revoke hiện vẫn dùng được.

**Cách khắc phục:**
Cài đặt [JingMatrix/TEESimulator](https://github.com/JingMatrix/TEESimulator), thêm tên gói vào file `target.txt` như sau:

```txt
com.vnid
```

### Mount đáng ngờ

Trong thời gian dài, BShield kiểm tra mount để phát hiện root. Điều này thường xảy ra khi cài các module như đổi font hoặc launcher.

Bạn có thể kiểm tra bằng **Native Detector**.

Ví dụ, nó có thể báo "Suspicious Mount".

**Cách khắc phục:**

- Nếu file zip của module có `mount --bind`, hãy cẩn thận vì có thể kích hoạt lỗi này. Yêu cầu dev sử dụng [phương pháp này thay thế](https://kernelsu.org/guide/module.html).
- KSU 3.0 và một số phiên bản mới của APatch, Magisk đã xử lý tốt hơn vấn đề này.
- Trên một số thiết bị, ReZygisk không thể unmount một số path. Nếu bạn gặp lỗi này, hãy báo cho tôi.

### [CHƯA XÁC NHẬN] Phát hiện vòng lặp image proc của module KSU/AP

Theo báo cáo gần đây từ [@Hzzmonet](https://t.me/Hzzmonet), BShield có thể phát hiện vòng lặp image proc của module KSU/AP. Nguyên nhân là các phiên bản KSU/AP cũ dùng OverlayFS, dẫn đến bị phát hiện.

Bạn có thể kiểm tra bằng **Native Detector**.

Ví dụ, nó có thể báo "KSU/AP loop".

**Cách khắc phục:**

- Nếu bạn dùng KernelSU bản gốc hoặc cũ, hãy dùng module TreatWheel của Pedro để ẩn.
- Nếu bạn dùng KernelSU-Next, hãy tắt tùy chọn `Use OverlayFS` trong phần cài đặt. Nhớ sao lưu module trước khi thao tác.

## Mã lỗi 6 (Phát hiện bootloader mở khóa, không sử dụng)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=6](https://vneid.gov.vn/shield-warning?code=6)

Lỗi này xảy ra khi bootloader bị mở khóa. Hiện tại hầu hết các ứng dụng dùng BShield chưa kích hoạt phát hiện này. Nếu trong tương lai có dùng, bạn có thể tham khảo cách khắc phục bên dưới.

**Cách khắc phục:**
Cài đặt [JingMatrix/TEESimulator](https://github.com/JingMatrix/TEESimulator), thêm tên gói vào `target.txt`:

```txt
com.vnid
```

## Mã lỗi 7 (Phát hiện ứng dụng, hiếm)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=7](https://vneid.gov.vn/shield-warning?code=7)

Lỗi này xảy ra khi có ứng dụng đáng ngờ trên thiết bị. Hiếm khi gặp trong thực tế.

**Cách khắc phục:**
Giống [mã lỗi 3](#mã-lỗi-3-phát-hiện-danh-sách-ứng-dụng).

## Mã lỗi 8 (Phát hiện ứng dụng dạng Privacy Space)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=8](https://vneid.gov.vn/shield-warning?code=8)

Lỗi này xảy ra khi bạn sử dụng ứng dụng trong emulator hoặc app dạng không gian riêng tư.

**Cách khắc phục:**
Không sử dụng ứng dụng bên trong app của bên thứ ba.

## Mã lỗi 10 (Phát hiện chế độ debug ADB)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=10](https://vneid.gov.vn/shield-warning?code=10)

Lỗi này xảy ra khi bạn bật chế độ debug ADB trên thiết bị.

**Cách khắc phục:**
Thực hành an toàn: không bật ADB debug khi không cần dùng.

## Mã lỗi 11 (Phát hiện chế độ nhà phát triển)

**Link tham khảo:** [https://vneid.gov.vn/shield-warning?code=11](https://vneid.gov.vn/shield-warning?code=11)

Lỗi này xảy ra khi bạn bật Developer Mode trên thiết bị.

**Cách khắc phục:**

Bạn có thể sử dụng tổ hợp:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [ImNotADeveloper](https://github.com/notyour777/ImNotADeveloper)

để ẩn Developer Mode và ADB debug mode.

Hoặc cách tốt nhất: không bật Developer Mode khi không sử dụng.
