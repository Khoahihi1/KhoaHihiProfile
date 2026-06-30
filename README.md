# Profile Website / Bio Page

<p align="center">
  <a href="https://khoahihiprofile.web.app/"><strong>Website sản phẩm mẫu: khoahihiprofile.web.app</strong></a>
</p>

Đây là source public mẫu cho một trang bio/profile tĩnh. Bản trong thư mục `github/` đã được che thông tin cá nhân: Firebase config, Discord ID, link mạng xã hội, analytics/user ID và ảnh cá nhân đều đã đổi sang ví dụ hoặc placeholder.

## Tính năng

- Trang bio tĩnh bằng HTML/CSS/JS thuần, không cần build.
- Card profile có avatar, mô tả, vị trí, badge, lượt thích và lượt xem.
- Link mạng xã hội dùng icon Font Awesome.
- Trình phát nhạc có playlist, ảnh bìa, thanh tiến trình, nút điều khiển và lyric `.lrc` đồng bộ.
- Widget trạng thái Discord qua Lanyard.
- Bộ đếm like/view lưu bằng Firebase Firestore.
- Layout responsive cho mobile.

## Chạy thử trên máy

Không mở trực tiếp bằng `file://`. Hãy chạy static server trong thư mục này:

```bash
python -m http.server 5500
```

Sau đó mở:

```text
http://localhost:5500/
```

Nếu dùng Node:

```bash
npx serve .
```

## Cấu trúc thư mục

```text
.
├─ index.html
├─ assets/
│  ├─ audio/      # File nhạc ví dụ
│  ├─ images/     # Ảnh ví dụ/avatar/cover
│  └─ lyrics/     # File lyric .lrc ví dụ
├─ images/        # Ảnh/font của template
├─ scripts/       # Thư viện JS và script template
├─ src/
│  ├─ firebase/   # Cấu hình Firebase
│  └─ features/   # Logic like/view
└─ styles/        # CSS template
```

## Những chỗ cần thay trước khi dùng

Mở `index.html` và đổi các giá trị ví dụ sau thành thông tin của bạn:

```html
<title>Demo Profile</title>
<p data-tippy-content="demo-tooltip" class="header" id="userName">Demo Profile</p>
<p id="bio_description" class="bio_description">Short bio example</p>
<p class="text">Your City</p>
```

Các link mạng xã hội hiện là ví dụ:

```text
https://example.com/facebook
https://example.com/spotify
https://github.com/your-username/your-repo
https://discord.gg/your-invite
```

Discord User ID mẫu:

```js
const userId = '123456789012345678';
```

Nếu muốn widget Discord hoạt động, bạn cần join server Lanyard trước:

```text
https://discord.gg/lanyard
```

## Cấu hình Firebase

Mở `src/firebase/firebase.js` và thay placeholder bằng Firebase Web App config của bạn:

```js
const firebaseConfig = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.firebasestorage.app",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_FIREBASE_APP_ID"
};
```

Bộ đếm dùng các document/collection sau:

```text
profile/stats
profileLikes/{clientId}
```

Rules Firestore mẫu:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /profile/stats {
      allow read: if true;
      allow write: if true;
    }

    match /profileLikes/{clientId} {
      allow read: if true;
      allow create: if true;
      allow update, delete: if false;
    }

    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

## Đổi nhạc và lyric

Thêm file vào các thư mục:

```text
assets/audio/ten-bai-hat.mp3
assets/images/anh-bia.jpg
assets/lyrics/ten-bai-hat.lrc
```

Sau đó sửa mảng `const playlist = [` trong `index.html`:

```js
{
  src: "./assets/audio/ten-bai-hat.mp3",
  title: "ten bai hat",
  totalDuration: "3:55",
  audioImage: "./assets/images/anh-bia.jpg",
  hasLyrics: true,
  lyrics: { source: "lrc", track: "assets/lyrics/ten-bai-hat.lrc", synced: "" }
}
```

Nếu thêm bài không có lyric, đổi biến này thành `false` để bài không bị bỏ qua:

```js
window.zyoAudioLyricsRequired = false;
```

## Deploy

Đây là web tĩnh nên có thể deploy lên:

- GitHub Pages
- Firebase Hosting
- Netlify
- Vercel
- Cloudflare Pages

Với GitHub Pages, đưa nội dung thư mục này lên repo rồi chọn Pages publish từ root của repo.

## Lưu ý trước khi public source của bạn

Trước khi public fork/source riêng, hãy kiểm tra và che các thông tin sau:

```text
Firebase config thật
Discord user ID thật
Link mạng xã hội thật
Analytics endpoint/user ID/signature
Ảnh cá nhân hoặc file audio bạn không muốn public
```

Bản mẫu này giữ nguyên tính năng chính, nhưng dùng placeholder để an toàn khi public source.