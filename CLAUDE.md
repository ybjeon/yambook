# yambook

맛집/카페/데이트 장소 DB와, 조건에 맞는 장소를 골라 블로그 글로 뽑아내기 위한 개인 저장소.

## 구조

- `places.csv` — 장소 DB. 컬럼: `이름, 설명, 네이버지도url, 대략적인 위치, 오픈 시간, 주차`
- `view.html` — `places.csv`를 정렬/검색/위치필터 가능한 표로 보여주는 로컬 뷰어 (브라우저로 직접 열기, 서버 불필요). CSV가 바뀌어도 코드 수정 없이 그대로 반영됨
- `blog/` — `/place-filter`로 생성된 필터링 결과 markdown 파일들이 저장되는 곳
- `imgs/` — 이미지 리소스
- `.claude/skills/` — 커스텀 slash command (아래 참고)

## Skills

- **`/place-filter <조건>`** — `places.csv`를 자연어 조건(예: `강남 분당 대형 카페`)으로 필터링해서 `blog/<조건-슬러그>.md`에 표 형태로 저장. CSV에 없는 속성(크기 등)은 알려진 장소면 지식으로 판단하고, 아니면 한계를 명시.
- **`/add-store <설명>`** — 자유 형식 텍스트(네이버지도 url, 이름, 위치, 영업시간, 주차 정보 등)에서 필드를 추출해 `places.csv`에 한 행 추가. 이름/url 중복 체크, 값 추론 불가 시 빈칸 유지(허구 데이터 생성 금지).

두 skill 모두 기존 `translate`/`summary` skill과 동일한 포맷(frontmatter `description` + `## Steps` + `## Rules`)을 따름.
