# 데이트 아카이브 템플릿

우리 동네·우리 커플만의 **데이트 코스 & 커플 콘텐츠 레이더**를 GitHub Pages 로 무료 발행하는 템플릿이에요.
서버도 빌드도 필요 없어요. `entries.json` 에 항목을 쌓으면 페이지가 알아서 카드로 그려줘요.

## 이런 게 들어 있어요

- **누적형 피드** — 데이트코스 / 커플콘텐츠 카드, 같은 사안(`topicKey`)은 한 카드에 타임라인으로 묶임
- **⏰ 임박 칩 · D-day 배지 · NEW 배지 · 필터**
- **자동 히스토리** — 기한 지난 콘텐츠는 *Contents History*, 지난 주말 코스는 *Course History* 로 이동
- **상대 취향 파악 설문(20문항)** 과 **취향 업데이트 폼**, 카드별 **피드백 위젯** — 저장하면 JSON 파일이 다운로드돼요
- 라이트/다크 테마, 모바일 대응

## GitHub 없이 쓰기 (가장 쉬운 방법)

GitHub 계정이 없어도 **발행된 사이트 주소만 열면** 바로 쓸 수 있어요.

1. 페이지 중간의 **내 아카이브 관리 → 펼치기**
2. **내 사이트 설정**에 지역·이름을 저장 (처음 저장하면 예시 카드가 사라지고 내 아카이브가 시작돼요)
3. 카드 채우기 — 둘 중 편한 쪽으로:
   - **AI 챗봇 이용:** '프롬프트 복사' → 웹 검색되는 챗봇(ChatGPT·Claude·Gemini 등)에 붙여 넣기 → 챗봇이 준 JSON을 '붙여넣기'에 넣고 '가져오기'
   - **직접 입력:** '카드 직접 추가' 폼
4. 가끔 **백업 내보내기**로 파일을 받아두세요

데이터는 **이 기기의 이 브라우저(localStorage)에만** 저장되고 서버로 전송되지 않아요. 그래서 기기를 바꾸거나 브라우저 데이터를 지우면 사라져요. 나중에 GitHub 로 옮기고 싶으면 **GitHub용 entries.json 받기**로 파일을 받아 저장소의 `entries.json` 을 교체하면 돼요.

> GitHub 로 직접 운영하는 사람이 이 관리 메뉴를 숨기려면 `config.json` 에 `"localTools": false` 를 넣으세요.
> AI 챗봇이 준 내용은 지어낸 정보가 섞일 수 있어요. 장소·일정은 공식 공지로 꼭 확인하세요.

## GitHub 로 운영하기 (5분 시작하기)

1. 이 저장소 위쪽의 **Use this template → Create a new repository** (또는 Fork)
2. 새 저장소의 **Settings → Pages → Branch: `main` / (root)** 로 켜기
3. `config.json` 을 내 지역에 맞게 고치기

   ```json
   { "siteName": "성수 데이트 아카이브", "region": "성수", "schedule": "", "tagline": "", "updatedAt": "" }
   ```
4. `entries.json` 의 **[예시] 카드 3개를 지우고** 내 항목을 넣기 (아래 형식 참고)
5. 커밋·푸시 → 1~2분 뒤 `https://<내아이디>.github.io/<저장소이름>/` 에서 확인

로컬에서 미리 보려면 파일을 더블클릭하지 말고(브라우저가 JSON 읽기를 막아요) 폴더에서 간단한 서버를 켜세요.

```powershell
python -m http.server 8000
```

## entries.json 형식

배열이고, 새 항목을 **맨 앞**에 추가해요.

```json
{
  "date": "2026-11-22",
  "type": "데이트코스",
  "topicKey": null,
  "expiresOn": "2026-12-31",
  "title": "토요일: 공원 산책 + 전시 + 국물 저녁",
  "summary": "카드에 바로 보이는 2~3문장 요약",
  "content": "## 오후 2시 — 장소\n\n- 이동: 대중교통 30분\n- **확인 필요**: 휴관일\n",
  "link": "https://출처-주소",
  "urgent": false
}
```

| 필드 | 설명 |
|------|------|
| `date` | 발행일 `YYYY-MM-DD` |
| `type` | `"데이트코스"` 또는 `"커플콘텐츠"` |
| `topicKey` | 같은 사안을 이어 붙일 때 쓰는 영문 slug (예: `"han-river-fireworks"`). 1회성이면 `null` |
| `expiresOn` | (선택) 이 날이 지나면 자동으로 History 로 이동. 코스는 생략하면 발행일+9일, 콘텐츠는 +21일 |
| `title` / `summary` | 카드 제목 / 요약 |
| `content` | 상세. `## 소제목`, `- 목록`, `**굵게**` 만 지원하는 간단한 마크다운 |
| `link` | (선택) 출처. `http(s)://` 만 표시돼요 |
| `urgent` | `true` 면 ⏰ 임박 표시와 상단 칩에 올라와요 (예약·티켓팅 오픈 14일 이내 권장) |

## Claude 가 자동으로 채우게 하기 (선택)

[Claude Code](https://claude.com/claude-code) 를 쓴다면 `AGENT_PROMPT.md` 를 정기 작업(예약 작업)의 프롬프트로 쓰면,
매주 웹 검색 → `entries.json` 갱신 → 커밋·푸시까지 자동으로 돌릴 수 있어요.
내 취향·거주지는 `profile.example.md` 를 `profile.md` 로 복사해서 적어두면 추천에 반영돼요.

## ⚠️ 개인정보 주의

- GitHub Pages 는 저장소가 **public 이면 누구나** 볼 수 있어요. `entries.json`·`config.json` 에 **실명, 정확한 집 주소, 연락처, 상대방의 개인 취향·건강 정보**를 쓰지 마세요.
- 설문·피드백 저장 파일(`preference-update-*.json`, `feedback-*.json`)과 `profile.md`, `inbox/` 는 `.gitignore` 에 들어 있어 커밋되지 않아요. 그래도 `git status` 로 올라가는 파일을 한 번씩 확인하세요.
- 커밋에 찍히는 이메일이 부담되면 GitHub 의 *Keep my email addresses private* 설정을 켜고 `git config user.email "<아이디>@users.noreply.github.com"` 로 두세요.
- 카드에 적는 장소·일정은 공식 공지로 한 번 더 확인하세요. 이 템플릿은 정확성을 보장하지 않아요.

## 라이선스

MIT — 자유롭게 복사·수정·재배포하세요.
