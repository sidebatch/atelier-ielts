# Writing Atelier v2 — 개발 진행 기록
> 이 파일은 45분 주기 개발 크론이 읽는 공유 상태 파일이다. 작업할 때마다 "완료" 섹션을 갱신할 것.

## 프로젝트
- 공개 IELTS Writing Task 2 학습 앱. 리포: sidebatch/atelier-ielts (master), 실주소 https://sidebatch.github.io/atelier-ielts/
- 로컬: ~/workspace/atelier-ielts/index.html — **단일 파일** (CSS+JS 내장). manifest.webmanifest, sw.js, icon-192.png, icon-512.png 동봉.
- 배포: master에 push하면 GitHub Pages 자동 배포. push 후 `curl -s -o /dev/null -w "%{http_code}" https://sidebatch.github.io/atelier-ielts/` 로 200 확인 (배포 반영까지 1~2분 걸릴 수 있음).

## 공식 평가기준 (IELTS Writing Task 2 public band descriptors — 검증됨)
4개 기준, 각 25%. 밴드 7 목표 문구:
- **TR**: addresses all parts of the task / presents a clear position throughout / presents, extends and supports main ideas
- **CC**: logically organises, clear progression / uses a range of cohesive devices appropriately (under-/over-use 주의) / presents a clear central topic within each paragraph
- **LR**: sufficient range for some flexibility and precision / uses less common lexical items with some awareness of style and collocation
- **GRA**: uses a variety of complex structures / produces frequent error-free sentences / good control of grammar and punctuation

## 학습법 설계 (효과+효율)
- 풀에세이 40분을 매번 돌리는 건 비효율. **자가진단 → 최약점 기준 판별 → 마이크로 드릴(5~10분) 집중** 루프가 핵심.
- 드릴 → 풀 실전 → 기준별 자가 채점(밴드 추정) → 약점 재드릴.

## v2 구성 (목표: 오늘 23:00까지)
1. [x] 🏠 훈련 탭: 12주제 풀 플로우 (주제→5분 플래닝→아웃라인→40분 실전→완료) — 5개 유형 전부
2. [x] 🎯 드릴 탭: 4개 기준 × 드릴 엔진 (객관식, 셔플, 해설, 최고기록). 스타터 12문항 내장, 32문항 확장 대기 중
3. [x] 📖 가이드 탭: 4개 기준별 공식 디스크립터(band 6/7/8) 한국어 정리 + 5-30-5 시간배분 + 서론/바꿔쓰기/링킹워드/실수 TOP5
4. [x] 🗓 예약 탭: 훈련 예약 (날짜/시간/주제) + 시간 되면 앱내 배너 + Notification
5. [x] 📊 기록 탭: 누적 통계(총훈련/연속일/평균단어/달성률) + 기준별 평균 밴드 + 최약점 표시
6. [x] 밴드 추정기: 훈련 완료 화면에서 기준별 2문항씩 자가체크 → 예상 밴드 + 최약점 기준의 드릴로 연결
7. [x] PWA: manifest + 아이콘(192/512) + sw.js (오프라인 캐시)
8. [ ] 드릴 문제 32개로 확장 (/tmp/drills_*.txt 수령 후 DRILLS에 병합)
9. [ ] 실기기 QA + 폴리싱

## 드릴 콘텐츠 스펙 (서브에이전트에게 위임 → /tmp/drills_*.txt 로 받기)
- TR 드릴 8문항: 실전 문제 1개 + "이 문제의 요구사항은?" 객관식 4지선다 (질문 개수 세기 / 입장 필요 여부 / 논의형 함정 등), 정답+한줄 해설
- CC 드릴 8문항: 빈칸 있는 문단 + 연결어 4지선다 (however/furthermore/in contrast 등), 정답+해설
- LR 드릴 8문항: 제시 문장 1개 + "다음 중 바꿔쓰기로 적절한 것은?" 4지선다 (collocation 오류 함정 포함), 정답+해설
- GRA 드릴 8문항: 단문 2개 + "가장 자연스러운 복문은?" 4지선다, 정답+해설
- 형식: JS 객체 리터럴 배열, 필드 {criterion, q, choices[4], answer(인덱스), why}. 파일당 const 없이 리터럴만.

## 작업 규칙
- index.html은 단일 파일 유지. JS 수정 후 `node --check` 로 문법 검증 필수.
- 모든 id 참조는 HTML에 존재해야 함 (python으로 대조).
- 커밋 메시지: "v2: ..." 형식. push 후 실주소 200 확인.
- PROGRESS.md의 체크박스를 작업할 때마다 갱신할 것.

## 완료 로그
- 2026-10-04 16:44: 목표 goal_8a181bf84151 생성, 45분 주기 개발 크론 가동 시작, 주제 9개 수령(/tmp/extra_topics.txt)
- 2026-10-04 17:00: v2 코어 배포 완료 (커밋 "v2: 12주제+밴드 자가진단+약점 드릴+공식기준 가이드+예약/기록+PWA", 실주소 200 확인). 드릴 32문항 서브에이전트 작성 중.
