# Writing Atelier v3 — 개발 진행 기록
> 이 파일은 30분 주기 개발 크론이 읽는 공유 상태 파일이다. 작업할 때마다 "완료" 섹션을 갱신할 것.

## 프로젝트 (v3 = 현재 방향)
- 공개 IELTS Writing Task 2 학습 앱. 리포: sidebatch/atelier-ielts (master), 실주소 https://sidebatch.github.io/atelier-ielts/
- 로컬: ~/workspace/atelier-ielts/index.html — **단일 파일** (CSS+JS 내장). manifest.webmanifest, sw.js, icon-192.png, icon-512.png 동봉.
- **v3 핵심 방향: 수동적 학습자용. 직접 작문·텍스트 입력·40분 쓰기 없음.** 읽고 탭하고 고르기만으로 학습.
- 학습 루프: 모범 에세이 읽기(주석 탭) → 이해 퀴즈 → 약점 드릴(객관식) → 예약/기록으로 루틴 유지.
- 배포: master에 push하면 GitHub Pages 자동 배포. push 후 `curl -s -o /dev/null -w "%{http_code}" https://sidebatch.github.io/atelier-ielts/` 로 200 확인 (배포 반영까지 1~2분 걸릴 수 있음).

## 공식 평가기준 (IELTS Writing Task 2 public band descriptors — 검증됨)
4개 기준, 각 25%. 밴드 7 목표 문구:
- **TR**: addresses all parts of the task / presents a clear position throughout / presents, extends and supports main ideas
- **CC**: logically organises, clear progression / uses a range of cohesive devices appropriately (under-/over-use 주의) / presents a clear central topic within each paragraph
- **LR**: sufficient range for some flexibility and precision / uses less common lexical items with some awareness of style and collocation
- **GRA**: uses a variety of complex structures / produces frequent error-free sentences / good control of grammar and punctuation

## v3 구성 (목표: 오늘 23:00까지)
1. [x] 🏠 홈: "작문 숙제 없음" 선언 + 오늘의 15분 루틴 + 기준별 드릴 현황 + 약점 드릴 바로가기
2. [x] 🎯 드릴: TR/CC/LR/GRA 4개 기준 × 8문항 = 32문항 객관식 (셔플, 해설, 최고기록). 약점 기준 자동 판별
3. [x] 📖 에세이: Band 9 모범 에세이 5개 (의견형/논의형/장단점형/문제-해결형/이중질문형, 각 260~290단어). 주석 5종(P바꿔쓰기/T중심문장/C연결어/L고급어휘/G복문) 탭→한국어 해설. 에세이당 이해 퀴즈 2개
4. [x] 📚 가이드: 4개 기준별 공식 디스크립터(band 6/7/8) 한국어 정리 + 5-30-5 시간배분 + 서론/바꿔쓰기/링킹워드/실수 TOP5
5. [x] 🗓 예약: 학습 예약 (날짜/시간/할 일) + 시간 되면 앱내 배너 + Notification. 앱 꺼져 있으면 OS 알람 불가 → 한계 명시
6. [x] 📊 기록: 학습한 날/연속일/읽은 에세이/푼 드릴 문제 + 기준별 최고기록 + 에세이별 퀴즈 점수
7. [x] PWA: manifest + 아이콘(192/512) + sw.js (캐시 writing-atelier-v3)
8. [x] 실기기 QA + 폴리싱 (브라우저 스모크 테스트 전 항목 PASS: 탭 6개, 에세이 주석 탭, 퀴즈 2/2, 드릴 1문항, 가이드)

## v2 → v3 변경 내역 (2026-10-04)
- v2는 직접 작문 플로우(12주제, 5분 플래닝, 40분 실전 쓰기, 밴드 자가진단)였으나 사용자 지시로 전면 제거.
- v3는 읽고 탭하고 고르기만: 에세이 읽기+주석+퀴즈가 핵심, 드릴은 32문항 그대로 재사용, 가이드/예약/기록은 수동 학습에 맞게 단순화.
- 커밋: "v3: 수동형 학습으로 전면 개편 (작문 제거, 에세이 읽기+주석+퀴즈+드릴)" — 실주소 200 확인.

## 드릴 콘텐츠 스펙 (완료 — /tmp/drills_*.txt → 병합됨)
- TR 8문항: 실전 문제 + "이 문제의 요구사항은?" 4지선다
- CC 8문항: 빈칸 문단 + 연결어 4지선다
- LR 8문항: 문장 + "바꿔쓰기로 적절한 것은?" 4지선다 (collocation 함정 포함)
- GRA 8문항: 단문 2개 + "가장 자연스러운 복문은?" 4지선다
- 형식: {q, choices[4], answer(인덱스), why}

## 에세이 콘텐츠 스펙 (완료 — /tmp/essays.txt → 병합됨)
- 5개 유형 × 260~290단어 × 4문단. segments: [{t, k, note}] — k: P/T/C/L/G/null, note는 한국어 "왜 좋은지" 해설.
- quiz: [{q, choices[4], answer, why}] 2개씩.

## 작업 규칙
- index.html은 단일 파일 유지. JS 수정 후 `node --check` 로 문법 검증 필수.
- 모든 id 참조는 HTML에 존재해야 함 (python으로 대조). onclick 함수도 전부 정의돼 있어야 함.
- 커밋 메시지: "v3: ..." 형식. push 후 실주소 200 확인.
- PROGRESS.md의 체크박스를 작업할 때마다 갱신할 것.
- v3에는 작문 관련 요소(textarea, 40분 타이머, 밴드 추정기, 플래닝 입력)를 넣지 말 것.

## 완료 로그
- 2026-10-04 16:44: 목표 goal_8a181bf84151 생성, 개발 크론 가동 시작
- 2026-10-04 17:00: v2 코어 배포 (12주제+밴드 자가진단+약점 드릴+공식기준 가이드+예약/기록+PWA), 실주소 200 확인
- 2026-10-04 17:10: 드릴 32문항 병합·배포, 실주소 32문항 확인
- 2026-10-04 17:20경: **v3 피벗 결정** — 사용자가 직접 작문 연습 제거, 수동적 학습자용으로 재설계 지시
- 2026-10-04 17:3x: v3 배포 완료 (에세이 5개+주석+퀴즈, 드릴 32문항, 가이드, 예약, 기록, PWA 캐시 v3), 실주소 200 확인. 크론 30분 간격으로 변경
