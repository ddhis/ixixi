---
pubDatetime: 2026-08-01T10:30:00+09:00
modDatetime: 2026-08-02T11:30:00+09:00
title: Astro Paper 블로그 시작하는 방법
slug: getting-started-with-astropaper
featured: true
draft: false
tags:
  - 블로그
  - 아스트로
  - AstroPaper
description: AstroPaper 테마로 블로그를 시작하는 방법을 안내합니다. 설치부터 첫 글 작성 기본 설정까지 한눈에 알아보세요.
---

## Table of contents

## 설치 및 시작 방법

VS 코드의 터미널에서 아래 명령어를 실행합니다.

```bash
npm create astro@latest -- --template satnaing/astro-paper
```

> [!INFO]- Git 저장소
https://github.com/satnaing/astro-paper.git

```
Where should we create your new project? 엔터
./square-star
새 프로젝트를 어디에 생성해야 하나요?

Install dependencies? Y
의존성을 설치하시겠습니까?

Initialize a new git repository? N
새 Git 저장소를 초기화하시겠습니까?
```


```bash
cd ./square-star
```

```bash
npm run dev
```

---

## 폴더 구조

```bash
Astro Paper/
├── public/
│   ├── pagefind/          # 빌드 시 자동 생성
│   ├── favicon.svg
│   └── default-og.jpg
├── src/
│   ├── assets/
│   │   ├── icons/
│   │   └── images/
│   ├── components/
│   ├── content/
│   │   ├── pages/
│   │   │   └── about.md
│   │   └── posts/          # 게시글
│   │       └── some-blog-posts.md
│   ├── i18n/               # 언어 설정
│   ├── layouts/
│   ├── pages/
│   ├── scripts/
│   ├── styles/
│   ├── types/
│   ├── utils/
│   ├── config.ts
│   └── content.config.ts
├── .github/
├── .vscode/
├── astro-paper.config.ts  # 사용자 설정 파일
├── astro.config.ts
├── package.json
├── pnpm-lock.yaml
├── tsconfig.json
└── (기타 설정 파일들: Dockerfile, eslint.config.js, LICENSE, README.md 등)
```

---

## 블로그 언어 변경

블로그를 한국어로 변경하려면 아래 파일을 추가합니다.

```ts file=src/i18n/lang/ko.ts
import type { UIStrings } from "../types";

export default {
  nav: {
    home: "홈",
    posts: "글",
    tags: "태그",
    about: "소개",
    archives: "보관함",
    search: "검색",
  },
  post: {
    publishedAt: "게시일",
    updatedAt: "수정일",
    sharePostIntro: "공유하기:",
    sharePostOn: "{{platform}}에서 공유하기",
    sharePostViaEmail: "이메일로 공유하기",
    tagLabel: "태그",
    backToTop: "맨 위로",
    goBack: "뒤로가기",
    editPage: "페이지 수정",
    previousPost: "이전 글",
    nextPost: "다음 글",
  },
  pagination: {
    prev: "이전",
    next: "다음",
    page: "페이지",
  },
  home: {
    socialLinks: "소셜 링크",
    featured: "추천 글",
    recentPosts: "최신 글",
    allPosts: "전체 글",
  },
  footer: {
    copyright: "Copyright",
    allRightsReserved: "All rights reserved.",
  },
  pages: {
    tagTitle: "태그",
    tagDesc: "다음 태그가 포함된 모든 글",

    tagsTitle: "태그",
    tagsDesc: "사용된 모든 태그",

    postsTitle: "전체 글",
    postsDesc: "게시한 모든 글",

    archivesTitle: "보관함",
    archivesDesc: "보관된 모든 글",

    searchTitle: "검색",
    searchDesc: "원하는 글을 검색해보세요.",
  },
  a11y: {
    skipToContent: "본문으로 이동",
    openMenu: "메뉴 열기",
    closeMenu: "메뉴 닫기",
    toggleTheme: "테마 변경",
    searchPlaceholder: "글 검색...",
    noResults: "검색 결과가 없습니다.",
    goToPreviousPage: "이전 페이지",
    goToNextPage: "다음 페이지",
  },
  notFound: {
    title: "404 페이지를 찾을 수 없습니다",
    message: "요청하신 페이지를 찾을 수 없습니다.",
    goHome: "홈으로 돌아가기",
  },
} satisfies UIStrings;
```

그리고 다음 파일을 변경합니다.

```ts file=astro.config.ts
  i18n: {
    locales: ["en"], // [!code --]
    defaultLocale: "en", // [!code --]
    locales: ["en","ko"], // [!code ++]
    defaultLocale: "ko", // [!code ++]
    routing: {
      prefixDefaultLocale: false,
    },
  },
```

---

## 블로그 기본 정보 변경하기

```ts file=astro-paper.config.ts
import { defineAstroPaperConfig } from "./src/types/config";

export default defineAstroPaperConfig({
  site: {
    url: "배포된 URL",
    title: "블로그 이름",
    description: "블로그 설명",  
    author: "작성자 이름",
    profile: "https://github.com/내 깃허브",
    ogImage: "default-og.jpg",
    lang: "ko",                 // 기본 언어
    timezone: "Asia/Seoul",     // 한국 시간
    dir: "ltr",                 // 텍스트 방향 ("ltr", "rtl", "auto")
    googleVerification: " - "   // Google Search Console 인증 메타 태그 값입니다. 선택 사항이며, PUBLIC_GOOGLE_SITE_VERIFICATION환경 변수보다 우선 적용됩니다.
  },
  posts: {
    perPage: 4,     // 전체 글에서 한 페이지에 표시할 글 개수
    perIndex: 4,    // 최근 게시물 섹션에 표시되는 게시물 수
    scheduledPostMargin: 15 * 60 * 1000,  // 예약 게시 허용 시간(15분)
  },
  features: {
    lightAndDarkMode: true, // 라이트/다크 모드 사용 여부
    dynamicOgImage: true,   // 게시글마다 OG 이미지 자동 생성
    showArchives: true,     // /archives페이지와 해당 헤더 링크를 표시합니다 .
    showBackButton: true,   // 게시글에 뒤로가기 버튼 표시
    editPost: {             // GitHub '페이지 편집' 버튼 표시
      enabled: true,        
      url: "https://github.com/satnaing/astro-paper/edit/main/",  // enabled가 false라면 url 삭제
    },
    search: "pagefind",     // 검색 기능(Pagefind 사용)
  },
  socials: [
    { name: "github",   url: "https://github.com/내아이디" },
    { name: "mail",     url: "mailto:내 이메일 주소" },
  ],
  shareLinks: [
    { name: "whatsapp", url: "https://wa.me/?text=" },
    { name: "facebook", url: "https://www.facebook.com/sharer.php?u=" },
    { name: "x",        url: "https://x.com/intent/post?url=" },
    { name: "telegram", url: "https://t.me/share/url?url=" },
    { name: "pinterest", url: "https://pinterest.com/pin/create/button/?url=" },
    { name: "mail",     url: "mailto:?subject=See%20this%20post&body=" },
  ],
});
```

---

## 메인 페이지 변경하기

```bash file=src/pages/index.astro
38번째 줄
56 ~ 69번째 줄
```

---

## 날짜 포맷 변경하기

```ts file=src/components/Datetime.astro
const date = datetime.format("D MMM, YYYY");   // [!code --]
const date = datetime.format("YYYY년 M월 D일");   // [!code ++]
```

---

## 폰트 변경하기

```ts file=astro.config.ts
  fonts: [
    {
      // [!code --:2]
      name: "Google Sans Code",
      cssVariable: "--font-google-sans-code",
      provider: fontProviders.google(),
      fallbacks: ["monospace"],
      weights: [300, 400, 500, 600, 700],
      styles: ["normal", "italic"],
      formats: ["woff", "ttf"],
      // [!code ++:2]
      name: "Noto Sans KR",
      cssVariable: "--font-noto-sans-kr",
      provider: fontProviders.google(),
      fallbacks: ["monospace"],
      weights: [300, 400, 500, 600, 700],
      styles: ["normal", "italic"],
      formats: ["woff", "ttf"],
    },
  ],
```


```ts file=src/layouts/Layout.astro
    <Font
      cssVariable="--font-google-sans-code"   // [!code --]
      preload={[{ subset: "latin", weight: 400, style: "normal" }]}   // [!code --]
      cssVariable="--font-noto-sans-kr"   // [!code ++]
      preload={[{ subset: "korean", weight: 400, style: "normal" }]}   // [!code ++]
    />
```


```ts file=src/styles/theme.css
  --font-app: var(--font-google-sans-code);  // [!code --]
  --font-app: var(--font-noto-sans-kr);   // [!code ++]
```

```ts file=src/pages/posts/[...slug]/index.png.ts
  const fonts = fontData["--font-google-sans-code"];  // [!code --]
  const fonts = fontData["--font-noto-sans-kr"];   // [!code ++]
```

```ts file=src/pages/og.png.ts
  const fonts = fontData["--font-google-sans-code"];  // [!code --]
  const fonts = fontData["--font-noto-sans-kr"];   // [!code ++]
```

---

## 컬러 변경하기

```css file=src/styles/theme.css
/* Light theme values */
:root,
[data-theme="light"] {
  --background: #fdfdfd;        /* 전체 페이지 배경색 */
  --foreground: #282728;        /* 기본 본문 글자색 */
  --accent: #006cac;            /* 강조 색상 */
  --accent-foreground: #ffffff; /* 강조 요소 내부의 글자색 */
  --muted: #e6e6e6;             /* 연한/비활성화 배경색 */
  --muted-foreground: #6b7280;  /* 부가적인 글자색 */
  --border: #ece9e9;            /* 테두리 및 구분선 색상 */
}

/* Dark theme values */
[data-theme="dark"] {
  --background: #212737;        /* 전체 페이지 배경색 */
  --foreground: #eaedf3;        /* 기본 본문 글자색 */
  --accent: #ff6b01;            /* 강조 색상 */
  --accent-foreground: #ffffff; /* 강조 요소 내부의 글자색 */
  --muted: #343f60;             /* 연한/비활성화 배경색 */
  --muted-foreground: #afb9ca;  /* 부가적인 글자색 */
  --border: #ab4b08;            /* 테두리 및 구분선 색상 */
}
```

---
## 게시물 초안

```md file=src/content/posts/.md파일
---
pubDatetime: 2026-08-01T10:30:00+09:00 // 작성 시간
modDatetime: 2026-08-02T11:30:00+09:00 // 수정 시간
title: 글제목
slug: post-link // 커스텀 URL
featured: true // 추천글에 게시
draft: false // 초안 유지
tags:
  - 태그1
  - 태그2
description: 글 요약
---

## Table of contents // 목차

## 설치 및 시작 방법

글 내용

```

---

## 게시물 파일 경로와 URL

```bash
src/content/posts/very-first-post.md          -> mysite.com/posts/very-first-post
src/content/posts/2025/example-post.md        -> mysite.com/posts/2025/example-post
src/content/posts/_2026/another-post.md       -> mysite.com/posts/another-post
src/content/posts/docs/_legacy/how-to.md      -> mysite.com/posts/docs/how-to
src/content/posts/Example Dir/Dummy Post.md   -> mysite.com/posts/example-dir/dummy-post
```

---

## 콜아웃

> [!NOTE] 메모
> 독자가 알아두어야 할 추가 정보입니다.

> [!TODO] 할일
> 향후 진행해야 하거나 완료해야 할 작업 목록입니다.

> [!ABSTRACT] 요약
> 문서나 글의 주요 내용을 간략하게 요약한 핵심 정보입니다.

> [!TIP] 팁
> 유용한 조언, 지름길 또는 모범 사례.

> [!WARNING] 경고
> 잘못될 수도 있고 예상치 못한 결과를 초래할 수도 있는 것.

> [!QUESTION] 질문
> 독자가 생각해 보거나 확인 및 해결이 필요한 의문 사항입니다.

> [!DANGER] 위험
> 심각한 오류, 데이터 손실 또는 오작동의 위험이 있습니다.

> [!FAILURE] 실패
> 작업이 실패했거나 시도가 거부되었음을 나타냅니다.

> [!BUG] 버그
> 수정이 필요한 시스템 오작동이나 소프트웨어 오류입니다.

> [!INFO] 정보
> 정보 전달의 맥락이 중립적이며 메모보다 긴급성이 떨어집니다.

> [!SUCCESS] 성공
> 어떤 일이 제대로 작동했거나 정확하다는 것을 확인하는 것.

> [!EXAMPLE] 예시
> 개념을 이해하는 데 도움이 되는 구체적인 예시입니다.

> [!QUOTE] 인용
> 다른 출처나 인물의 발언을 그대로 인용한 문구입니다.

지원되는 타입의 전체 목록 : NOTE, TODO, ABSTRACT, TIP, WARNING, QUESTION, DANGER, FAILURE, INFO, SUCCESS, EXAMPLE, QUOTE

> [!WARNING]- 접이식 콜아웃
> 이 내용은 독자가 펼쳐볼 때까지 숨겨져 있습니다.

```md
> [!NOTE]- 접이식 콜아웃
> 내용
```

> [!TIP]+ 유용한 팁 (기본적으로 펼쳐짐)
> 이 기능은 처음에는 열려 있지만 접을 수 있습니다.

```md
> [!NOTE]+ 접이식 콜아웃 (펼쳐짐)
> 내용
```

> [!NOTE] 제목 지정
> 글자체 뒤의 텍스트는 콜아웃의 제목이 됩니다. 이 부분을 생략하면 글꼴 이름이 자동으로 사용됩니다.

```md
> [!NOTE] 제목 지정
> 내용
```

---

## 목차 추가

```md

<!-- [!code ++] -->
## Table of contents

<!-- the rest of the post -->
```

---

## 구문 강조 표시

```ts file="astro.config.ts"
// ...
// [!code ++:5]
import {
  transformerNotationDiff,
  transformerNotationHighlight,
  transformerNotationWordHighlight,
} from "@shikijs/transformers";

export default defineConfig({
  // ...
  markdown: {
    remarkPlugins: [remarkToc, [remarkCollapse, { test: "Table of contents" }]],
    shikiConfig: {
      themes: { light: "min-light", dark: "night-owl" },
      defaultColor: false,
      wrap: false,
      transformers: [
        transformerFileName(),
        // [!code --:3]
        transformerNotationHighlight(),
        transformerNotationWordHighlight(),
        transformerNotationDiff({ matchAlgorithm: "v3" }),
      ],
    },
  },
  // ...
});
```

> [!EXAMPLE] 예시
> // [!code ++:5] <br>
> // [!code --:3] <br>
> &lt;!-- [!code ++] -->

---

## 블로그 콘텐츠용 이미지


디렉토리 내부 src/assets/(권장)

해당 디렉터리 안에 이미지를 저장할 수 있습니다 . 이러한 이미지는 Astro의 이미지 서비스 API를src/assets/ 통해 자동으로 최적화됩니다 .

example.jpg예: 경로가 인 항목을 표시하고 싶다고 가정해 보겠습니다 src/assets/images/example.jpg.

```md
![something](@/assets/images/example.jpg)
<!-- OR -->
![something](../../assets/images/example.jpg)
```


내부 public/디렉토리

해당 디렉터리 안에 이미지를 저장할 수 있습니다 public/ 이 디렉터리에 저장된 이미지는 public/Astro에서 최적화되지 않은 상태로 저장되므로 사용자가 직접 최적화해야 합니다.

이 이미지들을 표시하려면 절대 경로를 사용하세요. 마크다운 이미지 구문 이나 HTML img태그를 사용하여 표시할 수 있습니다.


예시: example.jpg가 에 위치한다고 가정합니다 public/assets/images/example.jpg.

```md
![something](/assets/images/example.jpg)
<!-- OR -->
<img src="/assets/images/example.jpg" alt="something">
```

---

## 광고 영역 설정

```ts file=src/pages/posts/[...slug]/index.astro
    <div class="my-2 flex items-center gap-2">
      <Datetime {pubDatetime} {modDatetime} {timezone} size="lg" />
      <span
        aria-hidden="true"
        class:list={[
          "text-muted-foreground max-sm:hidden",
          { hidden: !config.features.editPost?.enabled || hideEditPost },
        ]}
      >
        |
      </span>
      <EditPost {hideEditPost} {post} class="max-sm:hidden" />
    </div>
    // [!code ++:2]
    <div class="ad-container my-6 flex min-h-[90px] w-full items-center justify-center bg-gray-50/50 dark:bg-gray-800/20">
    </div>
    <article
      id="article"
      class:list={[
        "mt-8 w-full",
        "app-prose max-w-app",
        "prose-pre:bg-(--shiki-light-bg) dark:prose-pre:bg-(--shiki-dark-bg)",
      ]}
    >
      <Content />
    </article>
    // [!code ++:2]
    <div class="ad-container my-6 flex min-h-[120px] w-full items-center justify-center bg-gray-50/50 dark:bg-gray-800/20">
    </div>
    <hr class="my-8 border-dashed" />
```

---

## 새로운 상단 네비(메뉴) 추가하기


새로운 파일 추가

```ts file=src/components/NewTopNav.astro
페이지 구성
```


```ts file=src/components/Breadcrumb.astro
const navLabels: Record<string, string> = {
  posts: t.nav.posts,
  tags: t.nav.tags,
  about: t.nav.about,
  // [!code ++]
  newtopnav: t.nav.newtopnav,
  archives: t.nav.archives,
  search: t.nav.search,
};
```


```ts file=src/components/Header.astro
        <li class="col-span-2">
          <a
            href={getRelativeLocaleUrl(locale, "about")}
            class:list={{ "active-nav": isActive("/about") }}
          >
            {t.nav.about}
          </a>
        </li>
        // [!code ++:8]
        <li class="col-span-2">
          <a
            href={getRelativeLocaleUrl(locale, "newtopnav")}
            class:list={{ "active-nav": isActive("/newtopnav") }}
          >
            {t.nav.newtopnav}
          </a>
        </li>
        {
          features.showArchives && (
            <li class="col-span-2">
              <LinkButton
```


```ts file=src/content/pages/newtopnav.md
---
title: "새로운 상단 메뉴"
description: "파일명은 .mx 그리고 .mdx 둘다 가능합니다."
---

// [!code ++]
import NewTopNav from "../../components/NewTopNav.astro";

이 내용이 먼저 보여지고 하단에 NewTopNav.astro의 작성내용이 생깁니다.

// [!code ++]
<NewTopNav />
```


```ts file=src/i18n/types.ts
export interface UIStrings {
  nav: {
    home: string;
    posts: string;
    tags: string;
    about: string;
    // [!code ++]
    newtopnav: string;
    archives: string;
    search: string;
  };
```


```ts file=src/i18n/lang/en.ts
export default {
  nav: {
    home: "Home",
    posts: "Posts",
    tags: "Tags",
    about: "About",
    // [!code ++]
    newtopnav: "NewTopNav",
    archives: "Archives",
    search: "Search",
  },
```


```ts file=src/i18n/lang/ko.ts
export default {
  nav: {
    home: "홈",
    posts: "글",
    tags: "태그",
    about: "소개",
    // [!code ++]
    newtopnav: "새로운상단메뉴",
    archives: "보관함",
    search: "검색",
  },
```


새로운 파일 추가

```ts file=src/pages/bookmarks.astro
---
import { getEntry, render } from "astro:content";
import Layout from "@/layouts/Layout.astro";
import Header from "@/components/Header.astro";
import Breadcrumb from "@/components/Breadcrumb.astro";
import Main from "@/components/Main.astro";
import Footer from "@/components/Footer.astro";
import config from "@/config";

// [!code ++]
const newtopnav = await getEntry("pages", "newtopnav");

// [!code ++:5]
if (!newtopnav) {
  throw new Error(
    "Missing content entry: `newtopnav.md` or `newtopnav.mdx` in `src/content/pages/`"
  );
}

// [!code ++]
const { Content } = await render(newtopnav);
---

// [!code ++:6]
<Layout
  title={`${newtopnav.data.title} | ${config.site.title}`}
  description={newtopnav.data.description}
  ogImage={newtopnav.data.ogImage}
  canonicalURL={newtopnav.data.canonicalURL}
>
  <Header />

  <Breadcrumb />

// [!code ++]
  <Main pageTitle={newtopnav.data.title} class="app-prose">
    <Content />
  </Main>

  <Footer />
</Layout>

```

