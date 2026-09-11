# RecordPet 설치 파일 배포

RecordPet **시제품** 설치 파일을 내려받는 곳이다. 소스는 별도 저장소인 [wlsvy19/recordpet](https://github.com/wlsvy19/recordpet)에 있고, 이 저장소는 설치 파일 배포만 담당한다.

## 내려받기

| 운영체제 | 내려받기 | 설치 |
| --- | --- | --- |
| Windows 10/11 (x64) | **[RecordPetSetup.exe](https://github.com/wlsvy19/recordpet-release/releases/latest/download/RecordPetSetup.exe)** | 실행하면 현재 사용자 계정에만 설치된다. 관리자 권한을 요구하지 않는다. 완료 창이 따로 없고 진행 막대가 닫히면 끝난 것이며, 바탕화면 바로가기가 생기고 앱이 바로 실행된다. |
| macOS (Intel·Apple Silicon) | 아직 없음 | dmg는 한 번도 만든 적이 없다. 아래 ‘macOS’를 본다. |

위 Windows 링크는 **늘 최신 릴리스**를 가리킨다. 파일 이름에 버전을 넣지 않았기 때문이다. 어느 판인지 확인하려면 [Releases](https://github.com/wlsvy19/recordpet-release/releases) 페이지의 태그와 이 저장소의 `SHA256SUMS.txt`를 본다.

내려받은 파일이 맞는지 확인하려면 PowerShell에서:

```powershell
Get-FileHash .\RecordPetSetup.exe -Algorithm SHA256
```

## 먼저 알아야 할 것

- **실사용 제품이 아니라 검증용 시제품이다.** 기록은 설정에서 동의하고 시작을 눌러야만 시작한다. 켜면 1분마다 맨 앞 창의 **프로그램 이름과 창 제목만** 살펴 이 PC에 암호화해 저장한다. 창 본문·화면·키 입력은 읽지 않고, 밖으로 아무것도 보내지 않는다.
- **코드 서명과 공증이 없다.** Windows에서는 SmartScreen이 ‘알 수 없는 발행자’ 경고를 띄운다. 서명 자격 취득은 아직 결정되지 않았다.
- **macOS 동작은 검증되지 않았다.** 한 번도 빌드하거나 실행한 적이 없다.
- 앱은 로그인할 때 자동 실행되지 않는다. 껐다 켜면 직접 실행해야 한다.

## 설치 파일은 어떻게 올라오나

**바이너리는 어느 저장소에도 커밋하지 않는다.** GitHub의 저장소 파일 한도는 100 MiB인데 Windows 설치 파일은 그보다 크고, 바이너리는 릴리스 자산에 두는 것이 맞다. 이 저장소에는 `SHA256SUMS.txt`만 남는다.

### Windows

소스 저장소에서 `scripts/윈도우 배포.cmd`를 더블클릭한다(명령으로는 `npm run deploy:win`). 설치 파일을 다시 만든 뒤 릴리스 자산으로 올리고, 이 저장소의 `SHA256SUMS.txt`를 갱신해 커밋한다. 두 저장소 모두 커밋하지 않은 변경이 없어야 시작하므로, 올라간 설치 파일은 늘 커밋 하나로 설명된다.

### macOS

교차 빌드를 하지 않으므로 Mac 실기나 [자동 빌드](.github/workflows/release.yml)가 필요하다. 자동 빌드는 **아직 한 번도 실행하지 않았다.** Actions → 설치 파일 빌드와 릴리스 → Run workflow에서 `ref`에 소스 커밋을, `tag`에 릴리스 태그를 넣어 실행한다.

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
