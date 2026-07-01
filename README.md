# Obsidian × Claude Code 연동 설치 가이드 (Windows)

> **이 문서를 Claude Code에 던져주는 방법:**
> ```
> 이 링크의 지침서대로 Obsidian을 설치하고 Claude와 연동해줘:
> https://raw.githubusercontent.com/flyingjoojak/obsidian-claude-setup/master/README.md
> ```
> Claude가 이 문서를 읽고 순서대로 안내합니다.

---

## 이 가이드로 완성되는 것

```
Obsidian (로컬 노트 앱)
    ↕ Smart Connections MCP
Claude Code (AI 에이전트)
```

- Claude가 옵시디언 노트를 **읽고** 답변 (시맨틱 검색)
- Claude가 옵시디언 노트를 **직접 작성** (회의록, 일지, 메모 등)
- 일간·주간 노트로 할 일·한 일 자동 관리

---

## 사전 요구사항

- Windows 10/11
- [Claude Code](https://claude.ai/code) 설치됨
- Node.js 18+ 설치됨 (`node --version`으로 확인)
- Git 설치됨 ([git-scm.com](https://git-scm.com))

---

## STEP 1 · Obsidian 설치

1. [https://obsidian.md](https://obsidian.md) → **Download for Windows** 클릭
2. 설치 후 실행
3. **새 볼트 만들기** 선택
   - 볼트 이름: `second_brain`
   - 위치: `C:\Users\[사용자명]\Desktop\second_brain` 권장
4. 볼트 경로를 메모해두기 (이후 설정에 사용)
   ```
   내 볼트 경로: C:\Users\[사용자명]\Desktop\second_brain
   ```

> **⚠️ 중요**: Obsidian은 볼트 내부에 같은 이름의 폴더를 만듭니다.  
> 예: `...\second_brain\second_brain\`  
> 파일을 만들 때는 **안쪽 폴더** (`second_brain\second_brain\`) 기준으로 작성해야 PARA 구조에 맞게 저장됩니다.

---

## STEP 2 · 볼트 폴더 구조 생성

Obsidian 안에서 아래 폴더를 생성합니다. (좌측 사이드바 → 우클릭 → 새 폴더)

```
second_brain/
├── 🏠 Home.md              ← 메인 대시보드
├── 🤖 AI Context.md        ← Claude가 새 세션 시작 시 읽는 파일 (아래 참고)
├── 00_Inbox/               ← 빠른 캡처, 미분류
├── 01_Journals/
│   ├── Daily/              ← YYYY-MM-DD.md
│   ├── Weekly/             ← YYYY-WXX.md
│   └── Monthly/            ← YYYY-MM.md
├── 02_Projects/
│   ├── Active/             ← 진행 중인 프로젝트
│   └── Archive/            ← 완료된 프로젝트
├── 03_Areas/               ← 지속 관리 영역
├── 04_Knowledge/
│   ├── Learnings/          ← 새로 배운 것
│   ├── Ideas/              ← 아이디어
│   └── References/         ← 참고자료
└── 99_Templates/           ← Templater 템플릿
    ├── Daily Note.md
    ├── Weekly Review.md
    ├── Monthly Review.md
    ├── Project.md
    ├── Learning.md
    └── Idea.md
```

---

## STEP 3 · 핵심 플러그인 설치

Obsidian 설정(⚙️) → **커뮤니티 플러그인** → **커뮤니티 플러그인 검색**

| 플러그인 | 역할 | 필수 여부 |
|---------|------|----------|
| Smart Connections | Claude 연동 (시맨틱 검색) | **필수** |
| Periodic Notes | 일간·주간·월간 노트 원클릭 생성 | **필수** |
| Templater | 날짜 자동입력, 템플릿 처리 | **필수** |
| Calendar | 사이드바 달력 뷰 | 권장 |
| Tasks | 할일 마감·우선순위·전체조회 | 권장 |
| Dataview | 노트 쿼리 대시보드 | 권장 |
| QuickAdd | 빠른 캡처 | 선택 |
| Kanban | 프로젝트 칸반보드 | 선택 |
| Tag Wrangler | 태그 관리 | 선택 |

### 3-1. Smart Connections 설정 (필수)
- 설치 → 활성화
- **설정 → Smart Connections → 볼트 인덱싱 시작** 클릭
- 완료되면 `.smart-env\` 폴더가 생성됨 (정상)

### 3-2. Periodic Notes 설정
- Daily Notes: 활성화 / 경로 `01_Journals/Daily/` / 형식 `YYYY-MM-DD`
- Weekly Notes: 활성화 / 경로 `01_Journals/Weekly/` / 형식 `YYYY-[W]WW`

### 3-3. Templater 설정
- Template folder: `99_Templates`

### 비활성화할 코어 플러그인 (커뮤니티 플러그인과 충돌 방지)
- `Daily notes` → 비활성화 (Periodic Notes 사용)
- `Templates` → 비활성화 (Templater 사용)

---

## STEP 4 · 노트 템플릿 생성

`99_Templates\` 폴더에 아래 파일을 생성합니다.

### Daily Note.md
```markdown
---
date: <% tp.date.now("YYYY-MM-DD") %>
day: <% tp.date.now("dddd") %>
week: "[[<% tp.date.now("YYYY-[W]WW") %>]]"
tags:
  - journal/daily
---

# <% tp.date.now("YYYY-MM-DD (ddd)") %>

## 📆 오늘 일정
- 

## ✅ 오늘 할 일
- [ ] 

## 📝 오늘 한 일
- 

## 💡 메모 & 생각
> 

## 🔗 관련 노트
- 
```

### Weekly Review.md
```markdown
---
week: "<% tp.date.now("YYYY-[W]WW") %>"
month: "[[<% tp.date.now("YYYY-MM") %>]]"
tags:
  - journal/weekly
---

# <% tp.date.now("YYYY-[W]WW") %> 주간 리뷰

## 🎯 이번 주 목표
- [ ] 

## ✅ 완료한 것
- 

## ❌ 못한 것 & 이유
- 

## 📚 이번 주 배운 것
- 

## 🔮 다음 주 계획
- [ ] 

## 💬 한 줄 회고
> 
```

---

## STEP 5 · AI Context 파일 생성 (Claude 연동 핵심)

볼트 루트에 `🤖 AI Context.md` 파일을 만듭니다.  
Claude가 새 대화 시작 시 이 파일을 읽으면 볼트 구조와 규칙을 즉시 파악합니다.

```markdown
# 🤖 AI Context — Second Brain 가이드

> 이 파일은 Claude가 볼트 작업을 이어갈 때 읽는 컨텍스트 파일입니다.
> 새 대화 시작 시: "내 AI Context 파일 읽어줘" 라고 하면 됩니다.

---

## 👤 이 볼트의 목적
이 볼트는 **제2의 뇌(Second Brain)** 시스템입니다:
- 업무 및 프로젝트 관리
- 할 일(Tasks) 추적
- 일간 기록 (오늘 한 일)
- 주간 / 월간 계획 및 회고
- 새로 배운 것 (Learnings)
- 아이디어 캡처 (Ideas)
- `[[링크]]`로 노트 간 연결

---

## 🗂️ 볼트 구조
[위의 STEP 2 폴더 구조를 그대로 붙여넣기]

---

## 🔌 설치된 플러그인
[위의 STEP 3 플러그인 표를 붙여넣기]

> **Claude MCP 사용 규칙**:
> - 노트 읽기/검색 → Smart Connections MCP (`search_notes`, `get_note_content`)
> - 노트 생성/수정 → Write 도구로 직접 파일 경로에 저장

---

## 📐 컨벤션 & 규칙

### 날짜 형식
- Daily: `YYYY-MM-DD`
- Weekly: `YYYY-[W]WW`
- Monthly: `YYYY-MM`

### 태그 형식 (반드시 리스트)
```yaml
tags:
  - journal/daily
```

### 태그 체계
- `journal/daily`, `journal/weekly`, `journal/monthly`
- `project`
- `knowledge/learning`, `knowledge/idea`
- `meeting/weekly`, `meeting/action-review`

### 주의사항
- ⚠️ 템플릿 파일에 `[[wikilink]]` 금지 (그래프에 유령 노드 생성됨)
- 존재하는 파일만 링크할 것
- 빠른 메모는 `00_Inbox/`에 먼저, 나중에 정리
- 프로젝트 완료 시 `02_Projects/Archive/`로 이동

---

## ⚠️ 경로 정보
- 볼트 경로: `C:\Users\[사용자명]\Desktop\second_brain\second_brain`
- MCP 설정: `C:\Users\[사용자명]\Desktop\AI_AGENT\.mcp.json`

---

*마지막 업데이트: YYYY-MM-DD*
```

> **사용법**: 새 Claude 세션에서 `"내 AI Context 파일 읽어줘"` 라고 하면  
> Claude가 볼트 구조와 규칙을 파악하고 이어서 작업합니다.

---

## STEP 6 · Smart Connections MCP 서버 설치

> **⚠️ 레포 변경 안내 (2026-07 기준)**  
> 원작자(`brianpetro/smart-connections-mcp`)의 레포가 삭제됐습니다.  
> 현재 가장 활성화된 포크인 `msdanyg/smart-connections-mcp`를 사용하세요.

**PowerShell (관리자)**에서 실행:
```powershell
# 1. MCP 서버 클론
git clone https://github.com/msdanyg/smart-connections-mcp $env:USERPROFILE\smart-connections-mcp

# 2. 의존성 설치 및 빌드
cd $env:USERPROFILE\smart-connections-mcp
npm install
npm run build

# 3. 빌드 확인
dir dist\index.js
```

---

## STEP 7 · Claude Code MCP 설정

Claude Code 프로젝트 루트에 `.mcp.json` 파일을 생성합니다:

```json
{
  "mcpServers": {
    "smart-connections": {
      "command": "node",
      "args": [
        "C:\\Users\\[사용자명]\\smart-connections-mcp\\dist\\index.js"
      ],
      "env": {
        "SMART_VAULT_PATH": "C:\\Users\\[사용자명]\\Desktop\\second_brain"
      }
    }
  }
}
```

```powershell
echo $env:USERNAME   # 사용자명 확인
```

---

## STEP 8 · 연결 확인 및 테스트

Claude Code 재시작 후:

### 읽기 테스트
```
내 AI Context 파일 읽어줘
```
```
[검색어]와 관련된 노트 찾아줘
```

### 쓰기 테스트
```
오늘 일지를 옵시디언에 작성해줘. 오늘 한 일: 옵시디언 Claude 연동 설정 완료
```
→ `01_Journals\Daily\YYYY-MM-DD.md` 파일이 생성되면 성공

---

## Claude와 Obsidian 함께 쓰는 법

### 읽기: Smart Connections MCP
Claude에게 노트를 물어볼 때는 Smart Connections를 통해 시맨틱 검색으로 읽습니다.
파일 직접 열기(Read/cat)보다 **유사도 기반 검색**이 더 정확합니다.

```
"지난주 미팅 회의록 찾아줘"
"OKR 관련 노트 검색해줘"
"[프로젝트명] 진행 상황 알려줘"
```

### 쓰기: 직접 파일 경로에 저장
```
"오늘 회의 내용 옵시디언에 정리해줘"
"이번 주 할 일 목록 업데이트해줘"
"[프로젝트명] 회의록 작성해줘"
```

파일 경로 규칙:
```
C:\Users\[사용자명]\Desktop\second_brain\second_brain\{폴더}\{파일명}.md
```

> ⚠️ 볼트 루트(`second_brain\`)가 아닌 **안쪽 폴더**(`second_brain\second_brain\`)에 작성해야 합니다.

### 폴더별 저장 위치

| 노트 종류 | 저장 위치 |
|-----------|-----------|
| 일간 일지 | `01_Journals\Daily\YYYY-MM-DD.md` |
| 주간 리뷰 | `01_Journals\Weekly\YYYY-WXX.md` |
| 프로젝트 노트 | `02_Projects\Active\[프로젝트명]\` |
| 학습 정리 | `04_Knowledge\Learnings\` |
| 아이디어 | `04_Knowledge\Ideas\` |

### 노트 작성 형식
```markdown
---
date: YYYY-MM-DD
tags:
  - 카테고리/세부
---

# 제목

내용...

관련 노트: [[노트이름]]
```

### Callout 활용
```markdown
> [!important] 중요
> 핵심 내용

> [!tip] 팁
> 유용한 정보

> [!summary] 요약
> 요약 내용
```

---

## 연동 구조 요약

```
┌─────────────────────────────────────────────┐
│              Obsidian 볼트                    │
│  🤖 AI Context.md       ← 세션 시작 시 읽기 │
│  01_Journals\Daily\     ← 일간 노트          │
│  01_Journals\Weekly\    ← 주간 노트          │
│  02_Projects\           ← 프로젝트 노트      │
│  .smart-env\            ← 임베딩 (자동 생성) │
└───────────────────┬─────────────────────────┘
                    │ Smart Connections MCP
                    ↕ (읽기: 시맨틱 검색)
┌───────────────────────────────────────────────┐
│              Claude Code                        │
│  - 노트 읽기: search_notes, get_note_content  │
│  - 노트 쓰기: Write 도구로 직접 파일 저장     │
└───────────────────────────────────────────────┘
```

---

## 자주 묻는 문제

### Smart Connections 인덱싱이 안 되는 경우
```powershell
dir "C:\Users\[사용자명]\Desktop\second_brain\.smart-env"
# 비어있으면 Obsidian → Smart Connections 설정 → 인덱싱 재실행
```

### MCP 서버가 연결 안 되는 경우
```powershell
node C:\Users\[사용자명]\smart-connections-mcp\dist\index.js
# 오류 메시지 확인
```

### 경로에 한글/공백이 있는 경우
`.mcp.json`에서 역슬래시를 두 번 씁니다:
```json
"C:\\Users\\홍길동\\Desktop\\second_brain"
```

---

## 참고

- [Obsidian](https://obsidian.md)
- [Smart Connections Plugin](https://github.com/brianpetro/obsidian-smart-connections)
- [Smart Connections MCP](https://github.com/msdanyg/smart-connections-mcp) _(원작자 레포 삭제로 포크 사용)_
- [ai-dev-setup](https://github.com/flyingjoojak/ai-dev-setup) — dev-flow / full-review 워크플로우
