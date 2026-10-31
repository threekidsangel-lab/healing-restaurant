# 🏮 Healing Restaurant · Grill & Shabu — 공개 배포 가이드

누구나 링크로 들어와 함께 굽고 끓여 먹는 3D 힐링 식당입니다.

- **Vercel**: 웹사이트(파일)를 올려 두는 곳 → `○○○.vercel.app` 주소가 생겨요
- **Firebase Realtime Database**: 접속한 사람들의 위치, 채팅, 식탁 상태를 실시간으로 주고받는 곳

두 서비스 모두 무료로 시작할 수 있어요. 30분 정도면 끝나요.

---

## 폴더 안의 파일

| 파일 | 하는 일 |
|---|---|
| `index.html` | 게임 본체 (고칠 필요 없음) |
| `config.js` | **Firebase 설정을 붙여 넣는 파일** ← 여기만 고치면 돼요 |
| `og.jpg` | 스레드·카톡 등에 링크를 올릴 때 보이는 미리보기 이미지 |
| `database.rules.json` | Firebase 보안 규칙 (복사해서 붙여 넣는 용도) |

---

## 1단계 · Firebase 만들기 (실시간 접속용)

1. https://console.firebase.google.com 에 구글 계정으로 로그인 → **프로젝트 추가**
   - 이름: 예) `healing-restaurant`
   - Google 애널리틱스는 **사용 안 함**으로 해도 돼요 → 프로젝트 만들기
2. 왼쪽 메뉴 **빌드(Build) → Realtime Database → 데이터베이스 만들기**
   - 위치: **싱가포르 (asia-southeast1)** — 한국에서 가장 가까워요
   - 보안 규칙: **잠금 모드로 시작** → 사용 설정
3. Realtime Database 화면의 **규칙(Rules)** 탭을 열고, 안의 내용을 모두 지운 뒤
   `database.rules.json` 파일 내용을 그대로 붙여 넣고 **게시**를 눌러요.
4. 왼쪽 위 ⚙️ **프로젝트 설정 → 일반 → 내 앱 → 웹 아이콘(</>)** 을 눌러요
   - 앱 닉네임: 예) `restaurant-web` → **앱 등록** (Firebase 호스팅은 체크하지 않아도 돼요)
   - 화면에 나오는 `firebaseConfig = { ... }` 값을 복사해 두세요.
5. `config.js` 를 메모장으로 열고, 복사한 값들을 따옴표 안에 붙여 넣어요.
   ```js
   firebase: {
     apiKey: "AIza…",
     authDomain: "healing-restaurant.firebaseapp.com",
     databaseURL: "https://healing-restaurant-default-rtdb.asia-southeast1.firebasedatabase.app",
     projectId: "healing-restaurant",
     appId: "1:…:web:…"
   },
   ```
   > ⚠️ `databaseURL` 이 비어 있으면 혼자 플레이 모드로만 돌아가요. 4번 화면에 없으면
   > Realtime Database 화면 맨 위에 보이는 `https://…firebasedatabase.app` 주소를 넣어 주세요.

> 💡 apiKey는 공개돼도 괜찮은 값이에요 (웹사이트에 원래 들어가는 값). 보안은 3번의 규칙이 담당해요.

---

## 2단계 · GitHub에 파일 올리기

Vercel은 GitHub 저장소와 연결해서 배포해요. 이렇게 해 두면 나중에 파일만 고쳐도 자동으로 다시 배포돼요.

1. https://github.com 가입/로그인 → 오른쪽 위 **+ → New repository**
2. Repository name: `healing-restaurant` → **Public** 또는 **Private** 아무거나 → **Create repository**
3. 생성된 화면에서 **uploading an existing file** 링크(또는 Add file → Upload files)를 눌러요.
4. 이 폴더의 파일 4개(`index.html`, `config.js`, `og.jpg`, `database.rules.json`)와 `README.md`를 끌어다 놓고 **Commit changes**.

---

## 3단계 · Vercel로 배포하기

산사 사이트를 올렸던 Vercel 계정을 그대로 쓰면 돼요.

1. https://vercel.com 로그인 → **Add New… → Project**
2. **Import Git Repository** 에서 `healing-restaurant` 옆 **Import** (처음이면 GitHub 연결 허용)
3. 설정 화면:
   - Framework Preset: **Other**
   - Root Directory: 그대로 (`./`)
   - Build Command / Output Directory: 비워 둠
4. **Deploy** → 1분 안에 `https://healing-restaurant-xxxx.vercel.app` 같은 주소가 생겨요. 🎉
5. 휴대폰과 컴퓨터로 동시에 열어 보세요. 왼쪽 위에 **“1호점 · 🟢 지금 2명이 함께 있어요”** 가 보이면 성공!

### (선택) 내 도메인 연결
Vercel 프로젝트 → **Settings → Domains** 에서 `healing.jhcha.com` 처럼 원하는 주소를 추가하고,
안내되는 DNS 값(CNAME)을 도메인 관리하는 곳에 넣으면 돼요. sansatemple.com 을 연결했던 방법과 같아요.

### (권장) 미리보기 이미지 주소 고치기
스레드에서 링크 미리보기가 잘 뜨도록, 배포 주소가 정해지면 `index.html` 의 이 줄을
```html
<meta property="og:image" content="/og.jpg">
```
아래처럼 **전체 주소**로 바꿔 주세요 (GitHub에서 파일 열고 ✏️ 연필 버튼으로 수정 → Commit).
```html
<meta property="og:image" content="https://내주소.vercel.app/og.jpg">
```

---

## 수정하고 싶을 때
GitHub에서 파일을 고치고 **Commit** 하면 Vercel이 자동으로 1분 안에 다시 배포해요.

---

## 사람이 많아지면? (무료 한도)

| 항목 | 무료(Spark) 한도 | 이 게임에서는 |
|---|---|---|
| 동시 접속 | **100명** | 한 식당(호점)에 12명씩 자동으로 나눠 들어가요 (1호점, 2호점…) |
| 다운로드 | 하루 약 **360MB** (월 10GB) | 움직인 정보만 조금씩 보내도록 줄여 놨어요. 대략 하루 수십 시간 분량의 플레이 |
| 저장 용량 | 1GB | 접속 중인 사람 정보만 잠깐 저장, 나가면 자동 삭제 |

- 한도를 넘으면 그날은 새 접속이 막히거나 끊길 수 있어요. 홍보가 크게 터지면 Firebase를
  **Blaze(종량제)** 로 바꾸면 돼요. 10GB 넘는 다운로드만 GB당 약 $1이에요.
- 한 호점 인원은 `config.js` 의 `roomSize` 로 바꿀 수 있어요 (6~16 권장).
- Firebase 콘솔 → Realtime Database → **사용량** 탭에서 매일 확인할 수 있어요.

---

## 스레드 홍보 팁
- 미리보기 이미지가 자동으로 붙어요 (`og.jpg`).
- “같이 고기 구워 먹어요 🥩 링크 들어오면 1호점에서 만나요!”처럼 **같은 시간대에 모이도록** 시간을 정해 올리면 좋아요.
- 채팅에는 기본 욕설 필터와 도배 방지(1.2초 간격)가 들어 있어요.

## 문제가 생기면
- **계속 “혼자 플레이 중”** → `config.js` 의 `databaseURL` 확인, Firebase 규칙을 게시했는지 확인
- **화면이 하얗게 나옴** → 인터넷 연결 확인 후 새로고침 (3D 엔진을 인터넷에서 불러와요)
- **채팅이 안 보임** → 규칙의 이름 길이 제한(12자), 채팅 길이 제한(80자) 안인지 확인
