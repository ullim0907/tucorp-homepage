# 더울림코퍼레이션 홈페이지 (tucorp-homepage)

안성 스케줄보드와는 별도 레포로 분리된, **완전 공개용** 회사 홈페이지입니다.
같은 Firebase 프로젝트(TUCorp)를 공유하되, Hosting "사이트"만 별도로 만들어 배포합니다.

## 폴더 구조

```
tucorp-homepage/
├── index.html          ← 홈
├── 회사소개/index.html
├── 사업영역/index.html
├── 고시가정보/index.html  ← 기존 완성 파일로 교체 필요
├── 오시는길/index.html
├── 문의/index.html
├── assets/style.css     ← 공통 스타일 (네이비 + 크래프트지 톤)
├── firebase.json
└── .firebaserc          ← 프로젝트 ID 입력 필요
```

## 1. 고시가정보 파일 교체

`고시가정보/index.html`은 자리표시자입니다. Cowork에서 이미 완성한
고시가정보 단일 HTML 파일 내용으로 통째로 덮어써 주세요.

## 2. GitHub 레포 생성 및 push

```bash
cd tucorp-homepage
git init
git add .
git commit -m "init: 홈페이지 초기 스캐폴드"

# GitHub에서 새 레포(tucorp-homepage)를 먼저 만든 뒤
git remote add origin https://github.com/<계정명>/tucorp-homepage.git
git branch -M main
git push -u origin main
```

## 3. Firebase CLI 설치 및 로그인 (이미 되어 있다면 생략)

```bash
npm install -g firebase-tools
firebase login
```

## 4. 기존 TUCorp 프로젝트에 "공개용 사이트" 추가

스케줄보드가 이미 쓰고 있는 프로젝트를 그대로 사용합니다. 프로젝트 ID는
Firebase 콘솔 → 프로젝트 설정에서 확인할 수 있습니다.

```bash
firebase use --add
# 목록에서 TUCorp 프로젝트 선택 → alias는 default로

firebase hosting:sites:create tucorp-homepage
```

이후 `.firebaserc` 파일을 열어 두 군데의
`여기에_기존_TUCorp_Firebase_프로젝트_ID_입력` 부분을 실제 프로젝트 ID로
바꿔주세요. (사이트 이름을 `tucorp-homepage`가 아닌 다른 이름으로 만들었다면
`firebase.json`의 target 배열 값도 함께 맞춰주세요.)

## 5. 배포

```bash
firebase deploy --only hosting:homepage
```

배포가 끝나면 `https://tucorp-homepage.web.app` 같은 임시 주소로 바로
확인할 수 있습니다.

## 6. 커스텀 도메인 연결 (tucorp.co.kr)

1. Firebase 콘솔 → Hosting → 방금 만든 `tucorp-homepage` 사이트 → **커스텀 도메인 추가**
2. `tucorp.co.kr` 입력 → 안내되는 A 레코드(또는 TXT 인증값) 확인
3. 가비아 DNS 관리 화면(DNS 레코드 설정 → 레코드 수정)에서 안내된 값 그대로 추가
   - SPF/DKIM 등록 때와 동일한 화면, 동일한 방식
4. DNS 반영까지 대기 (수 분~24시간)

## 7. (선택) GitHub Actions로 자동 배포

`.github/workflows/firebase-hosting.yml`에 워크플로 템플릿을 넣어뒀습니다.
Firebase 콘솔에서 서비스 계정 키를 발급받아 GitHub 레포의
Settings → Secrets → Actions에 `FIREBASE_SERVICE_ACCOUNT`로 등록하면,
`main` 브랜치에 push할 때마다 자동 배포됩니다. 처음엔 4~5단계처럼 수동
배포만으로도 충분하니, 페이지 수정이 잦아지면 이 단계를 나중에 켜도 됩니다.

## 안성 스케줄보드와의 관계

- 스케줄보드: 별도 레포, 같은 Firebase 프로젝트의 **다른** Hosting 사이트,
  Firebase Auth 로그인 필요 (내부 전용 — 그대로 유지)
- 홈페이지(이 레포): `tucorp-homepage` 사이트, 로그인 없이 완전 공개
