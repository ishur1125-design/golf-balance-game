# 골프 성향 16유형 (골프 밸런스게임)

극단적인 14개의 밸런스게임으로 골프 성향 4글자를 뽑아주는 웹앱입니다.
빌드 도구 없이 `index.html` 하나로 돌아갑니다.

- 배포 예정 주소: https://ishur1125-design.github.io/golf-balance-game/
- 4개의 축: **D**river/**L**ayup · **S**core/**F**un · **P**lan/**I**mpulse · **N**ervous/**C**ool

## 파일

| 파일 | 설명 |
|---|---|
| `index.html` | 앱 전체 (문항·유형·궁합·공유·결과 카드). 의존성 없음 |
| `Code.gs` | 구글 시트 집계용 Apps Script. **깃허브가 아니라 구글에 붙입니다** |
| `og-image.png` | 카톡·SNS 링크 미리보기 이미지 (1200×630) |

## 1단계 — 깃허브에 올리기

1. github.com에서 **New repository** → 이름 `golf-balance-game` → **Public** → Create
2. **uploading an existing file** 클릭 → `index.html`, `og-image.png`, `README.md` 끌어다 놓기 → Commit
3. 저장소 **Settings → Pages** → Source를 `Deploy from a branch`, 브랜치 `main` / `/(root)` → Save
4. 1~2분 뒤 `https://ishur1125-design.github.io/golf-balance-game/` 접속

여기까지만 해도 앱은 완전히 동작합니다. (선택 비율 섹션만 "집계 중"으로 표시됨)

## 2단계 — 선택 비율 집계 켜기 (Apps Script)

이 단계는 구글 계정 소유자만 할 수 있습니다.

1. [sheets.new](https://sheets.new) 로 **새 스프레드시트**를 만들고 이름을 `골프16유형 응답`으로
2. 상단 메뉴 **확장 프로그램 → Apps Script** 클릭
3. 열린 편집기의 기존 코드를 **전부 지우고** `Code.gs` 내용을 붙여넣기 → 저장(💾)
4. 오른쪽 위 **배포 → 새 배포**
   - 유형 선택(⚙️) → **웹 앱**
   - 실행 계정: **나**
   - 액세스 권한: **모든 사용자** ← 이걸 안 바꾸면 집계가 안 됩니다
   - **배포** → 권한 검토 → 계정 선택 → "고급" → "안전하지 않음(이동)" → 허용
5. 나오는 **웹 앱 URL**(`https://script.google.com/macros/s/…/exec`)을 복사
6. 깃허브에서 `index.html`을 열어 연필(✏️) 아이콘 → 아래 줄을 찾아 URL을 붙여넣고 Commit

```js
var API_URL = "";   // ← 따옴표 안에 붙여넣기
```

`API_URL`이 비어 있으면 집계 기능은 통째로 꺼집니다. 언제든 다시 비우면 됩니다.

### 집계에 대해 알아둘 것

- 저장되는 값: 시각 · 유형 코드 · 14문항 응답 · 성별 · 나이대 · 구력 · 평균타수 · 목적
- **닉네임은 저장되지 않습니다.**
- 참여자가 `MIN_N`명(기본 30명) 미만이면 비율을 표시하지 않습니다. `index.html`의 `var MIN_N = 30;`에서 조정합니다.
- 웹 앱 URL은 `index.html`에 적히므로 공개됩니다. 장난성 응답을 막기 위해 `Code.gs`에 하루 300건 제한과 형식 검증을 넣어두었습니다. 필요하면 `MAX_PER_DAY` 값을 조정하세요.

## 문항·유형 수정하는 법

`index.html` 안에서 아래 이름으로 찾으면 됩니다.

- `var QS = [` — 14개 문항. `ax`는 축, `av`/`bv`는 각 선택지가 가리키는 글자
- `var T = {` — 16유형 이름과 설명
- `function compat(` — 궁합 점수 규칙
- `var TIPS`, `var PURPOSE` — 평균타수별 팁 / 목적별 코멘트

문항을 더하거나 뺄 때는 같은 축의 문항 수에 맞춰 `var W = {` 의 가중치 배열 길이도 함께 고쳐야 합니다.

## 로컬에서 확인하기

```bash
cd golf-balance-game && python3 -m http.server 8901
```

브라우저에서 `http://localhost:8901` 로 접속합니다.

---

재미로 보는 테스트입니다. 공인 심리검사가 아니며, MBTI와 무관한 독자적인 4축 체계를 사용합니다.
