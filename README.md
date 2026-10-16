# 고니 REA 스킬

REA(Reverse Engineer Anything)를 활용해 웹사이트와 배포 앱의 기능을 분석하고, 근거를 정리해 내 프로젝트의 독립 구현으로 연결하는 한국어 에이전트 스킬입니다.

**REA 엔진의 원본은 [morluto/rea](https://github.com/morluto/rea)입니다.** 이 저장소는 고니의 사용 절차·증거 판독 기준·CLI 도우미를 제공합니다. REA 엔진을 포함하지 않으며, 스킬 설치와 REA 도구 연결은 별도입니다.

## 스킬 설치

Codex에서는 다음처럼 설치합니다. 기존 폴더가 있으면 덮어쓰지 말고 내용을 먼저 비교하세요.

```bash
git clone https://github.com/qwer777qq/goni-rea-skill.git ~/.codex/skills/goni-rea
```

Claude Code에서는 `~/.claude/skills/goni-rea`에 같은 저장소를 복제합니다. 설치 후 사용하는 에이전트에서 스킬을 다시 불러오거나 새 세션을 시작합니다.

## REA 연결

Node.js와 npm이 필요합니다. 공식 안내는 [REA 설치 문서](https://github.com/morluto/rea/blob/main/docs/installation.md)를 참고하세요.

```bash
npx rea-agents setup
```

사용할 에이전트를 선택하고 설정 변경 내용을 확인해 적용한 뒤 에이전트를 다시 시작합니다. 네이티브 바이너리 분석은 대상 OS와 분석 제공자에 따라 별도 도구가 필요합니다.

MCP 대신 CLI를 사용할 때는 Python 3와 Node.js가 필요합니다. 이 스킬의 도우미는 재현성을 위해 **REA 6.5.0** npm 캐시를 사용합니다. 최신 버전 MCP와 도우미의 버전은 다를 수 있습니다.

```bash
npx --yes rea-agents@6.5.0 --help
cd ~/.codex/skills/goni-rea
python scripts/rea_cli.py --version
```

도우미는 패키지를 자동 설치하거나 클라이언트 설정을 바꾸지 않습니다. 캐시가 없으면 준비 명령을 안내하고 종료합니다. 명령과 옵션은 연결된 REA 버전의 도움말을 우선합니다.

## 사용 예시

```text
$goni-rea로 이 사이트의 검색 필터 기능을 분석해 줘.
대상: https://example.com
화면에서 확인한 동작과 코드에서 추론한 내용을 구분하고,
우리 프로젝트에 같은 사용자 흐름을 독립 구현한 뒤 실제 입력으로 검증해 줘.
```

```text
$goni-rea로 내가 제공한 JavaScript 앱 폴더를 분석해 줘.
내보내기 기능의 근거, 분석 범위, 아직 확인하지 못한 서버 동작을 정리해 줘.
```

분석 대상은 사용 권한이 있는 파일과 허용된 관찰 범위로 정합니다. 프런트엔드 분석으로 서버 모델·프롬프트까지 복원했다고 주장하지 않으며, 계정 토큰이나 원본 HAR 등 민감한 자료를 공개 저장소에 올리지 않습니다.

## 구성

- [SKILL.md](SKILL.md): 에이전트 작업 절차와 적용 범위
- [references/workflows.md](references/workflows.md): CLI 실행·증거 해석·문제 진단
- [scripts/rea_cli.py](scripts/rea_cli.py): 캐시된 공식 REA 6.5.0 실행 도우미
- [agents/openai.yaml](agents/openai.yaml): Codex 표시 이름과 호출 예시

검증 기준: 2026-10-16, Windows에서 공식 REA 6.5.0 CLI 실행 확인. 다른 운영체제의 실제 동작은 별도 검증이 필요합니다. REA 원본 라이선스는 [원본 저장소의 MIT 라이선스](https://github.com/morluto/rea/blob/main/LICENSE)를 따릅니다.
