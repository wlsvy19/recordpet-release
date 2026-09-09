# RecordPet 설치 파일 배포

RecordPet **시제품** 설치 파일을 내려받는 곳이다. 소스는 별도 저장소인 [wlsvy19/recordpet](https://github.com/wlsvy19/recordpet)에 있고, 이 저장소는 설치 파일 배포만 담당한다.

## 내려받기

[Releases](https://github.com/wlsvy19/recordpet-release/releases) 페이지에서 최신 릴리스의 자산을 내려받는다.

| 운영체제 | 파일 | 설치 |
| --- | --- | --- |
| Windows 10/11 (x64) | `RecordPet-Prototype-<버전>-x64-Setup.exe` | 실행하면 현재 사용자 계정에만 설치된다. 관리자 권한을 요구하지 않는다. |
| macOS (Intel·Apple Silicon) | `RecordPet Prototype-<버전>.dmg` 또는 `-arm64.dmg` | 열어서 앱을 응용 프로그램으로 옮긴다. |

설치 파일이 아직 올라와 있지 않으면 아래 ‘설치 파일 만들기’를 따라 릴리스를 만든다.

## 먼저 알아야 할 것

- **실사용 제품이 아니라 검증용 시제품이다.** 실제 PC 활동을 기록하지 않는다. 작업 일지 화면에 보이는 내용은 고정된 합성 예시이며 이 앱은 다른 프로그램의 창이나 화면을 관찰하지 않는다.
- **코드 서명과 공증이 없다.** Windows에서는 SmartScreen이 ‘알 수 없는 발행자’ 경고를 띄우고, macOS에서는 Gatekeeper가 ‘확인되지 않은 개발자’로 막는다. 서명 자격 취득은 아직 결정되지 않았다.
- **macOS 동작은 검증되지 않았다.** dmg는 자동 빌드로 만들어지지만 Mac 실기에서 실행·성능을 확인한 적이 없다.
- 앱은 표시 설정(펫 크기 등)만 로컬에 저장한다. 외부로 아무것도 전송하지 않고 자동 실행에 등록하지 않는다.

## 설치 파일 만들기

설치 파일은 저장소에 커밋하지 않는다. GitHub의 저장소 파일 한도는 100 MiB인데 Windows 설치 파일은 그보다 크고, 바이너리는 릴리스 자산으로 두는 것이 맞다. 그래서 [자동 빌드](.github/workflows/release.yml)가 두 OS에서 빌드해 릴리스에 올린다.

1. 이 저장소의 **Actions → 설치 파일 빌드와 릴리스 → Run workflow**를 연다.
2. `ref`에 소스 커밋(브랜치·태그·커밋 해시)을, `tag`에 릴리스 태그를 넣는다. 예: `v0.1.0-prototype.1`.
3. 실행하면 `windows-latest`에서 `.exe`, `macos-latest`에서 `.dmg`를 만들고 릴리스를 만들어 자산으로 올린다.

Windows 러너와 macOS 러너가 서로 독립적으로 진행되므로 한쪽이 실패해도 다른 쪽 결과와 로그를 볼 수 있다. macOS 빌드는 이 자동 빌드가 처음 시도하는 경로라 실패할 수 있고, 그때는 로그를 보고 소스 저장소에서 고친다.

## 직접 빌드하기

소스 저장소를 clone한 뒤 그 OS에서 빌드한다. 교차 빌드는 하지 않는다.

```sh
git clone https://github.com/wlsvy19/recordpet.git
cd recordpet
npm ci
npm test          # 화면 없는 전체 검증
npm run dist:win  # Windows에서 .exe
npm run dist:mac  # macOS에서 .dmg
```

산출물은 `dist/`에 생기며 이 폴더는 소스 저장소에서 Git 제외 대상이다.
