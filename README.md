# Luyện đề master – App Android

App là "vỏ" Android mở thẳng https://luyendemaster.vercel.app.
Cập nhật web trên Vercel → app tự có nội dung mới, **không cần build lại APK**.
Chỉ build lại APK khi đổi icon, tên app, hoặc code trong thư mục `native/`.

## Bước 1. Cập nhật web (project Vercel của bạn)
Chép đè các file trong thư mục `web-update/` (nằm trong file zip riêng) vào project web, rồi deploy:
`js/guard.js`, `js/app-bridge.js`, `index.html`, `vercel.json`, `download.html`.
Mở `js/app-bridge.js` và `download.html`, sửa dòng `GITHUB_REPO = "TEN_GITHUB/TEN_REPO"` thành repo thật (bước 2).

## Bước 2. Tạo repo và khai báo khóa ký
1. Tạo repo GitHub **Public** (để học sinh tải APK không cần đăng nhập), đẩy toàn bộ nội dung zip này lên (nhớ thư mục ẩn `.github`).
2. Vào repo → Settings → Secrets and variables → Actions → New repository secret. Thêm:

| Secret | Lấy từ đâu |
|---|---|
| `KEYSTORE_BASE64` | nội dung file `keystore-base64.txt` |
| `KEYSTORE_PASSWORD` | file `KEYSTORE-INFO.txt` |
| `KEY_ALIAS` | `ldm` |
| `KEY_PASSWORD` | file `KEYSTORE-INFO.txt` |
| `GOOGLE_SERVICES_JSON_BASE64` | (tùy chọn, cho thông báo đẩy) xem bước 4 |

⚠️ Giữ file keystore thật kỹ. Không đưa vào repo.

## Bước 3. Build APK
Actions → **Build APK** → Run workflow (có thể ghi "nội dung cập nhật"). Sau 5–8 phút, APK nằm ở **Releases** của repo.
Link tải cố định cho học sinh:
`https://luyendemaster.vercel.app/download`

## Bước 4. Thông báo đẩy (tùy chọn)
1. Firebase Console → project `nightshift-51ded` → Project settings → Your apps → **Add app → Android**, package name: `com.luyendemaster.app`.
2. Tải `google-services.json`, mã hóa base64 (Linux/Mac: `base64 -w0 google-services.json`) rồi dán vào secret `GOOGLE_SERVICES_JSON_BASE64`. Build lại.
3. Cho phép app ghi token: thêm vào Realtime Database Rules (trong `"rules"`):
   `"fcmTokens": { "$u": { ".read": false, ".write": true } }`
4. Gửi thông báo: Firebase Console → Engagement → **Messaging** → New campaign → Notifications → Target: chọn app Android → Send.

## Tự cập nhật
Mỗi lần Build APK sẽ tạo bản `vN` mới. Khi học sinh mở app, app so với bản mới nhất trên Releases và hiện nút **Cập nhật**.
Vì dùng cùng khóa ký, bản mới cài đè bản cũ, không mất đăng nhập.

## Trong app đã có
- Chặn chụp/quay màn hình khi làm bài (FLAG_SECURE).
- Nút Back: hỏi "Thoát bài làm?", bấm 2 lần mới thoát app.
- Nới anticheat cho điện thoại (không báo nhầm khi bật bàn phím).
