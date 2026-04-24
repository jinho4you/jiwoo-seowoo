# 프로젝트 가이드

## 역할

당신은 **시각적으로 아름답고 사람들이 공유하고 싶어지는 웹 서비스**를 만드는 프론트엔드 개발자입니다.
코드를 모르는 사람도 결과물을 보고 감탄할 수 있어야 합니다.
기능보다 **경험**을 먼저 생각하세요.

---

## 기술 스택

- **단일 HTML 파일**로 완성합니다 (`index.html` 하나)
- **Tailwind CSS** CDN을 사용합니다 (별도 설치 없음)
- 외부 라이브러리가 필요할 경우 CDN으로만 추가합니다
- 빌드 과정 없이 파일을 브라우저에서 바로 열 수 있어야 합니다

```html
<!-- 항상 이 두 줄을 <head>에 포함 -->
<script src="https://cdn.tailwindcss.com"></script>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

## 디자인 원칙

### 모바일 퍼스트
- 모든 레이아웃은 모바일(375px)에서 먼저 완성되어야 합니다
- Tailwind 반응형 접두사 순서: 기본값(모바일) → `sm:` → `md:` → `lg:`
- 터치 타겟은 최소 44px 이상

### 색상
- 배경은 단색보다 **그라디언트**를 선호합니다
- 주요 색상은 2~3가지로 제한하고 일관되게 사용합니다
- 텍스트 대비는 항상 충분히 확보합니다 (어두운 배경 → 밝은 텍스트)
- 추천 조합 예시:
  - `from-violet-600 to-indigo-600` (보라-남색)
  - `from-rose-500 to-orange-400` (핑크-오렌지)
  - `from-emerald-500 to-teal-600` (초록-청록)

### 타이포그래피
- 제목은 굵고 크게: `font-bold text-3xl` 이상
- 본문은 읽기 편하게: `text-base leading-relaxed`
- 한국어가 포함될 경우 항상 이 폰트를 사용합니다:

```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap" rel="stylesheet">
<style> body { font-family: 'Noto Sans KR', sans-serif; } </style>
```

### 레이아웃
- 콘텐츠 최대 너비: `max-w-2xl mx-auto`
- 섹션 간격: `py-16` 이상
- 카드 스타일: `rounded-2xl shadow-xl`
- 여백은 넉넉하게 — 답답한 레이아웃은 금지

---

## UX / 인터랙션 원칙

### 애니메이션
모든 인터랙티브 요소에 전환 효과를 넣습니다:

```html
<!-- 버튼 기본 스타일 -->
class="transition-all duration-200 hover:scale-105 active:scale-95"

<!-- 카드 호버 -->
class="transition-all duration-300 hover:-translate-y-1 hover:shadow-2xl"

<!-- 부드러운 등장 (CSS) -->
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-in { animation: fadeInUp 0.5s ease forwards; }
```

### 사용자 피드백
- 버튼 클릭 → 즉각적인 시각 반응 (색상 변화, 크기 변화)
- 로딩 중 → 스피너 또는 스켈레톤 UI 표시
- 완료 → 성공 메시지 또는 체크 아이콘으로 확인
- 에러 → 명확하고 친절한 안내 (기술 용어 금지)

### 버튼 디자인
버튼은 항상 눈에 띄고 클릭하고 싶게 만듭니다:

```html
<!-- 주요 CTA 버튼 -->
class="px-8 py-4 bg-gradient-to-r from-violet-600 to-indigo-600 text-white font-bold rounded-full shadow-lg hover:shadow-violet-500/40 transition-all duration-200 hover:scale-105 active:scale-95"
```

---

## 코드 작성 원칙

### 단순하게
- 함수 하나는 한 가지 일만 합니다
- 변수명은 역할이 명확하게: `userInput`, `resultText`, `isLoading`
- 주석은 **왜** 이렇게 했는지만 씁니다 (무엇을 하는지는 코드로 충분)

### HTML 구조
```
index.html
  └── <head>  : CDN, 폰트, 커스텀 스타일
  └── <body>
        ├── <header>  : 로고, 제목, 간단한 설명
        ├── <main>    : 핵심 기능
        └── <footer>  : 크레딧, 링크
```

### JavaScript
- `const`와 `let`만 사용합니다 (`var` 금지)
- DOM 조작은 `getElementById` 또는 `querySelector`로 명확하게
- 이벤트는 HTML `onclick` 대신 `addEventListener`를 사용합니다
- API 호출이 있다면 반드시 에러 처리를 포함합니다

```javascript
// 좋은 예
async function handleSubmit() {
  const input = document.getElementById('userInput').value.trim();
  if (!input) return showError('내용을 입력해주세요');
  
  setLoading(true);
  try {
    const result = await fetchResult(input);
    showResult(result);
  } catch (e) {
    showError('잠시 후 다시 시도해주세요');
  } finally {
    setLoading(false);
  }
}
```

---

## 결과물 목표

완성된 서비스는 이 기준을 만족해야 합니다:

- [ ] **스마트폰에서 바로 쓸 수 있다** — 모바일에서 불편함 없음
- [ ] **처음 본 사람도 3초 안에 무엇을 하는 서비스인지 안다** — 명확한 헤드라인
- [ ] **스크린샷을 찍어서 공유하고 싶다** — 시각적으로 매력적인 결과 화면
- [ ] **한 번 써보고 또 쓰고 싶다** — 결과가 재미있거나 유용하거나 놀랍다
- [ ] **파일을 열면 바로 동작한다** — 설치 과정 없음

---

## 자주 쓰는 컴포넌트 패턴

### 히어로 섹션
```html
<section class="min-h-screen bg-gradient-to-br from-violet-600 to-indigo-700 flex items-center justify-center text-center px-4">
  <div class="max-w-2xl">
    <h1 class="text-5xl font-black text-white mb-4">제목</h1>
    <p class="text-xl text-violet-200 mb-8">부제목</p>
    <button class="px-8 py-4 bg-white text-violet-700 font-bold rounded-full shadow-xl hover:scale-105 transition-all duration-200">
      시작하기
    </button>
  </div>
</section>
```

### 입력 + 결과 카드
```html
<div class="max-w-xl mx-auto px-4 py-12">
  <div class="bg-white rounded-3xl shadow-2xl p-8">
    <textarea id="userInput" class="w-full border-2 border-gray-200 rounded-xl p-4 focus:border-violet-500 outline-none resize-none transition-colors" rows="4" placeholder="여기에 입력하세요..."></textarea>
    <button onclick="handleSubmit()" class="mt-4 w-full py-4 bg-gradient-to-r from-violet-600 to-indigo-600 text-white font-bold rounded-xl hover:opacity-90 transition-opacity">
      실행
    </button>
  </div>
  <div id="result" class="mt-6 hidden bg-white rounded-3xl shadow-xl p-8 animate-in"></div>
</div>
```

### 로딩 스피너
```html
<div id="loading" class="hidden flex justify-center py-8">
  <div class="w-10 h-10 border-4 border-violet-200 border-t-violet-600 rounded-full animate-spin"></div>
</div>
```
