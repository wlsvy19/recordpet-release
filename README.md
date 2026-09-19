# RecordPet 설치 파일 배포

RecordPet **시제품** 설치 파일을 내려받는 곳이다. 소스는 별도 저장소인 [wlsvy19/recordpet](https://github.com/wlsvy19/recordpet)에 있고, 이 저장소는 설치 파일 배포만 담당한다.

## 내려받기

| 운영체제 | 내려받기 | 설치 |
| --- | --- | --- |
| Windows 10/11 (x64) | **[RecordPetSetup.exe](https://github.com/wlsvy19/recordpet-release/releases/latest/download/RecordPetSetup.exe)** | 실행하면 현재 사용자 계정에만 설치된다. 관리자 권한을 요구하지 않는다. 완료 창이 따로 없고 진행 막대가 닫히면 끝난 것이며, 바탕화면 바로가기가 생기고 앱이 바로 실행된다. |
| macOS (Apple Silicon, arm64) | **[RecordPet-arm64.dmg](https://github.com/wlsvy19/recordpet-release/releases/latest/download/RecordPet-arm64.dmg)** | dmg를 열어 앱을 `Applications`로 옮긴다. 서명·공증이 없어 처음 열 때 macOS가 막는다. 아래 ‘먼저 알아야 할 것’을 본다. |
| macOS (Intel) | 아직 없음 | 교차 빌드를 하지 않아 Intel Mac에서 따로 만들어야 한다. |

위 링크들은 **늘 최신 릴리스**를 가리킨다. 파일 이름에 버전을 넣지 않았기 때문이다. 어느 판인지 확인하려면 [Releases](https://github.com/wlsvy19/recordpet-release/releases) 페이지의 태그와 이 저장소의 `SHA256SUMS.txt`를 본다.

내려받은 파일이 맞는지 확인하려면 PowerShell에서:

```powershell
Get-FileHash .\RecordPetSetup.exe -Algorithm SHA256
```

macOS 터미널에서:

```sh
shasum -a 256 RecordPet-arm64.dmg
```

## 먼저 알아야 할 것

- **실사용 제품이 아니라 검증용 시제품이다.** 기록은 설정에서 동의하고 시작을 눌러야만 시작한다. 켜면 맨 앞 창의 **프로그램 이름과 창 제목**을 살피고, 화면 캡처가 켜져 있으면(기본값) **맨 앞 창의 화면을 찍어 글자만 읽은 뒤 그림은 버린다.** 제외 목록에 넣지 않은 앱의 화면 글자는 읽힌다. 화면 캡처만 따로 끌 수 있다. 읽은 내용은 이 PC에 암호화해 저장하고 밖으로 아무것도 보내지 않는다. 키 입력은 읽지 않는다.
- **설치 파일이 약 1.2GB다.** 업무일지 문장을 이 PC 안에서 다듬는 작은 언어 모델(Qwen3-1.7B)이 들어 있다. 외부 AI 서비스로 보내지 않는다.
- **코드 서명과 공증이 없다.** Windows에서는 SmartScreen이 ‘알 수 없는 발행자’ 경고를 띄운다. macOS에서는 Gatekeeper가 처음 여는 것을 막으므로 시스템 설정 → 개인정보 보호 및 보안에서 ‘그래도 열기’를 눌러야 하고, 화면 기록 권한은 다시 설치할 때마다 새로 켜야 할 수 있다. 서명 자격 취득은 아직 결정되지 않았다.
- **macOS는 일부만 검증했다.** Apple Silicon Mac 한 대에서 빌드·설치·실행과 화면 없는 검사를 했다. 펫 동작과 실제 기록의 화면 확인, 내려받은 dmg로 새 Mac에 설치하는 확인은 아직 하지 않았다.
- 앱은 로그인할 때 자동 실행되지 않는다. 껐다 켜면 직접 실행해야 한다.

## 설치 파일은 어떻게 올라오나

**바이너리는 어느 저장소에도 커밋하지 않는다.** GitHub의 저장소 파일 한도는 100 MiB인데 설치 파일은 그보다 크고(약 1.2GB), 바이너리는 릴리스 자산에 두는 것이 맞다. 이 저장소에는 `SHA256SUMS.txt`만 남는다. **그래서 저장소의 파일 목록에는 설치 파일이 보이지 않는다.** 올라간 파일은 [Releases](https://github.com/wlsvy19/recordpet-release/releases/latest)의 Assets에서 본다.

### Windows

소스 저장소에서 `scripts/윈도우 배포.cmd`를 더블클릭한다(명령으로는 `npm run deploy:win`). 설치 파일을 다시 만든 뒤 릴리스 자산으로 올리고, 이 저장소의 `SHA256SUMS.txt`를 갱신해 커밋한다. 두 저장소 모두 커밋하지 않은 변경이 없어야 시작하므로, 올라간 설치 파일은 늘 커밋 하나로 설명된다.

### macOS

Mac에서 소스 저장소의 `scripts/맥 배포.command`를 더블클릭한다(명령으로는 `npm run deploy:mac`). 그 Mac의 아키텍처용 dmg를 다시 만들어 릴리스 자산으로 올리고 `SHA256SUMS.txt`를 갱신해 커밋한다. 2026-09-19 Apple Silicon Mac에서 처음 올렸다. 교차 빌드를 하지 않으므로 Intel용은 그 기계에서 따로 올려야 한다. [자동 빌드](.github/workflows/release.yml)는 **아직 한 번도 실행하지 않았다.** Actions → 설치 파일 빌드와 릴리스 → Run workflow에서 `ref`에 소스 커밋을, `tag`에 릴리스 태그를 넣어 실행한다.

## 직접 빌드하기

소스 저장소를 clone한 뒤 그 OS에서 빌드한다. 교차 빌드는 하지 않는다.

```sh
git clone https://github.com/wlsvy19/recordpet.git
cd recordpet
npm ci
npm test          # 화면 없는 전체 검증
npm run dist:win  # Windows에서 out/Windows/RecordPetSetup.exe
npm run dist:mac  # macOS에서 dmg
```
