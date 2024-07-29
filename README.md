# Start

bundle install
bundle exec jekyll serve

```
minimal-mistakes
├── _data                      # 테마를 사용자 정의하기 위한 데이터 파일
|  ├── navigation.yml          # 주요 내비게이션 링크
|  └── ui-text.yml             # 테마의 UI 전반에서 사용되는 텍스트
├── _includes                  # 포함 파일들
|  ├── analytics-providers     # 분석 스니펫 (Google 및 사용자 정의)
|  ├── comments-providers      # 댓글 스니펫
|  ├── footer
|  |  └── custom.html          # 사이트 하단에 추가할 사용자 정의 스니펫
|  ├── head
|  |  └── custom.html          # 사이트 헤드에 추가할 사용자 정의 스니펫
|  ├── feature_row             # 기능 행 헬퍼
|  ├── gallery                 # 이미지 갤러리 헬퍼
|  ├── group-by-array          # 아카이브용 배열 그룹화 헬퍼
|  ├── nav_list                # 내비게이션 목록 헬퍼
|  ├── toc                     # 목차 헬퍼
|  └── ...
├── _layouts                   # 레이아웃 파일들
|  ├── archive-taxonomy.html   # Jekyll 아카이브 플러그인용 태그/카테고리 아카이브
|  ├── archive.html            # 아카이브 기본 레이아웃
|  ├── categories.html         # 카테고리별로 그룹화된 포스트 목록 아카이브
|  ├── category.html           # 특정 카테고리로 그룹화된 포스트 목록 아카이브
|  ├── collection.html         # 특정 컬렉션의 문서 목록 아카이브
|  ├── compress.html           # 순수 Liquid로 HTML 압축
|  ├── default.html            # 다른 모든 레이아웃의 기본
|  ├── home.html               # 홈페이지 레이아웃
|  ├── posts.html              # 연도별로 그룹화된 포스트 목록 아카이브
|  ├── search.html             # 검색 페이지
|  ├── single.html             # 단일 문서 (포스트/페이지/기타) 레이아웃
|  ├── tag.html                # 특정 태그로 그룹화된 포스트 목록 아카이브
|  ├── tags.html               # 태그별로 그룹화된 포스트 목록 아카이브
|  └── splash.html             # 스플래시 페이지
├── _sass                      # SCSS 파셜 파일들
├── assets                     # 자산 파일들
|  ├── css
|  |  └── main.scss            # 메인 스타일시트, _sass의 SCSS 파셜 파일을 로드
|  ├── images                  # 포스트/페이지/컬렉션 등을 위한 이미지 자산
|  ├── js
|  |  ├── plugins              # jQuery 플러그인
|  |  ├── vendor               # 외부 스크립트
|  |  ├── _main.js             # jQuery 이후 로드할 플러그인 설정 및 기타 스크립트
|  |  └── main.min.js          # </body> 전에 로드할 최적화 및 병합된 스크립트 파일
├── _config.yml                # 사이트 설정 파일
├── Gemfile                    # gem 파일 종속성
├── index.html                 # 최근 포스트를 보여주는 페이지네이션된 홈페이지
└── package.json               # NPM 빌드 스크립트
```
