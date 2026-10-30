# Git 브랜치 관리

> 생성 · 이동 · 업로드 · 병합 · 삭제

로컬에서 브랜치를 만들고 GitHub에 올린 뒤 main에 병합하고 정리하기까지의 명령어와 순서

*L.L.M Academy · 개발 환경*

---

## 명령어 한눈에 보기: 브랜치 작업 13가지

생성 → 이동 → 업로드 → 병합 → 삭제 순서로 정리. 예시 브랜치 이름은 `aaa`

| 작업 | 명령어 |
| --- | --- |
| 새 브랜치 생성 + 이동 | `git switch -c aaa` |
| 기존 브랜치로 이동 | `git switch aaa` |
| 브랜치 목록 확인 | `git branch` |
| 변경 사항 스테이징 | `git add .` |
| 커밋 생성 | `git commit -m ""` |
| GitHub에 최초 업로드 | `git push -u origin aaa` |
| GitHub에 추가 업로드 | `git push` |
| 로컬에서 aaa를 main에 병합 | `git merge aaa` |
| GitHub main을 로컬 main에 반영 | `git pull origin main` |
| GitHub aaa 브랜치 삭제 | `git push origin --delete aaa` |
| 로컬 aaa 브랜치 삭제 | `git branch -d aaa` |
| 로컬 aaa 강제 삭제 | `git branch -D aaa` |

> **병합은 받는 쪽에서.** `git merge` 전에 결과를 받을 브랜치(main)로 먼저 이동

---

## ① 브랜치 생성: main을 기준으로 새 브랜치 만들기

main에서 출발해야 최신 main 내용 위에서 작업 시작

**터미널: 생성**

```bash
# main으로 이동
git switch main
# main을 기준으로 aaa 생성 + 이동
git switch -c aaa
```

`-c` = create. 브랜치를 새로 만들면서 곧바로 그 브랜치로 이동

**터미널: 확인**

```bash
$ git branch
* aaa
  bbb
  main
```

`*` 표시 = 현재 위치한 브랜치. 생성 직후에는 aaa에 `*` 표시

---

## ② 브랜치 이동: `-c` 옵션 유무로 구분

이미 있는 브랜치는 이름만, 새 브랜치는 `-c`와 함께

```bash
git switch main   # main으로 이동
git switch aaa    # aaa로 이동
git switch bbb    # bbb로 이동
```

| 기존 브랜치로 이동 | 새 브랜치 생성 + 이동 |
| --- | --- |
| `git switch aaa` | `git switch -c aaa` |
| 이미 존재하는 브랜치로 이동 | 현재 브랜치를 기준으로 새 브랜치 생성 |
| 없는 이름이면 오류 | 생성과 동시에 그 브랜치로 이동 |

> **주의:** 커밋하지 않은 변경 사항이 있으면 브랜치 이동이 제한될 수 있음. 이동 전에 커밋부터

---

## ③ 커밋과 업로드: 로컬 aaa를 GitHub aaa로

aaa에서 작업·커밋 후 push. 최초 1회만 `-u`로 연결

**터미널: 작업 후 커밋**

```bash
git switch aaa
git add .
git commit -m "aaa 작업 내용"
```

**터미널: GitHub 업로드**

```bash
# 최초 업로드 (upstream 설정)
git push -u origin aaa
# 이후 업로드
git push
```

```
로컬 aaa (내 컴퓨터)  ── git push ──▶  GitHub aaa (origin/aaa)
```

- `-u`: 로컬 aaa가 GitHub의 `origin/aaa`를 추적하도록 upstream 설정. 이후 `git push`만으로 업로드

> **push ≠ 병합.** 원격 브랜치만 갱신. main에 자동 병합되지 않음

---

## ④-A 로컬에서 직접 병합: 받는 브랜치로 먼저 이동

Pull Request 없이 터미널에서 aaa를 main에 합치는 방법

```bash
# 1. 병합 결과를 받을 main으로 이동
git switch main
# 2. aaa의 변경 사항을 main에 병합
git merge aaa
# 3. 병합 결과를 GitHub main에 반영
git push origin main
```

**핵심 규칙**

- 병합 결과를 받는 브랜치에서 `git merge` 실행
- main에서 `git merge aaa` → aaa 변경 사항이 로컬 main에 통합
- 로컬 main만 바뀐 상태 → `git push origin main`으로 GitHub에 반영

**이럴 때는 먼저 정리**

- 로컬과 원격의 커밋 이력이 다르면 원격 변경 사항부터 통합
- 병합 충돌 발생 시 충돌 해결 후 커밋

---

## ④-B GitHub에서 병합한 경우: pull로 로컬 main 동기화

GitHub에서 이미 병합했다면 로컬에서 `git merge aaa` 불필요

| 단계 | 위치 | 작업 | 설명 |
| --- | --- | --- | --- |
| 1 | GitHub | Pull Request 생성 | aaa → main으로 병합 요청 |
| 2 | GitHub | Merge pull request | 병합 버튼 클릭 |
| 3 | GitHub | Confirm merge | GitHub main에 aaa 반영 완료 |
| 4 | 로컬 | `git pull origin main` | 최신 main을 로컬로 가져오기 |

```bash
git switch main
git pull origin main
```

```
GitHub main (aaa 병합 완료)  ── git pull ──▶  로컬 main (최신 내용 반영)
```

---

## ⑤ 브랜치 삭제: 원격과 로컬은 따로 삭제

브랜치를 지워도 이미 main에 병합된 내용은 그대로 유지

### GitHub 브랜치 삭제

```bash
git push origin --delete aaa
```

| 대상 | 결과 |
| --- | --- |
| GitHub aaa | 삭제 |
| 로컬 aaa | 유지 |
| main에 병합된 내용 | 유지 |

### 로컬 브랜치 삭제

```bash
# 삭제할 브랜치에서 벗어나기
git switch main
git branch -d aaa   # 안전한 삭제
git branch -D aaa   # 강제 삭제
```

- `-d`: 병합 상태를 확인하는 안전한 삭제. 병합되지 않은 커밋이 있으면 거부
- `-D`: 병합 여부와 무관하게 강제 삭제

> **`-D` 주의:** 아직 다른 곳에 보존되지 않은 커밋을 잃을 수 있음

---

## 전체 흐름: 로컬 작업 → GitHub PR 병합 → 정리

처음부터 끝까지 여섯 단계

1. **브랜치 생성**
   ```bash
   git switch main
   git switch -c aaa
   ```
2. **작업 후 커밋**
   ```bash
   git add .
   git commit -m "aaa 작업 내용"
   ```
3. **GitHub에 업로드**
   ```bash
   git push -u origin aaa
   ```
4. **GitHub에서 병합**: Pull Request 생성 → Merge pull request → Confirm merge
5. **로컬 main 동기화**
   ```bash
   git switch main
   git pull origin main
   ```
6. **브랜치 삭제**
   ```bash
   git branch -d aaa
   git push origin --delete aaa
   ```

---

## 명령어별 방향: 무엇이 어디로 가는지

`origin` = GitHub 원격 저장소. 콜론(`:`) 앞이 보내는 쪽, 뒤가 받는 쪽

| 명령어 | 구분 | 의미 |
| --- | --- | --- |
| `git push origin main` | PUSH | 로컬 main → GitHub main |
| `git push origin aaa` | PUSH | 로컬 aaa → GitHub aaa |
| `git push origin main:aaa` | PUSH | 로컬 main → GitHub aaa |
| `git pull origin main` | PULL | GitHub main → 현재 로컬 브랜치 |
| `git pull origin aaa` | PULL | GitHub aaa → 현재 로컬 브랜치 |
| `git merge aaa` | MERGE | 로컬 aaa → 현재 로컬 브랜치에 병합 |

---

## 핵심 정리: Push · Pull · Merge 구분

세 개념을 구분하면 브랜치 관리 기본 흐름 완성

| | 개념 | 방향 | 설명 | 명령어 |
| --- | --- | --- | --- | --- |
| 1 | **Push** | 로컬 → GitHub | 내 컴퓨터의 커밋을 원격 저장소로 보내기 | `git push` |
| 2 | **Pull** | GitHub → 로컬 | 원격 변경 사항을 가져와 현재 브랜치에 통합 | `git pull origin main` |
| 3 | **Merge** | 브랜치 → 브랜치 | 한 브랜치의 변경 사항을 다른 브랜치에 합치기 | `git merge aaa` |

> 브랜치를 삭제해도 이미 다른 브랜치에 병합된 내용은 사라지지 않음
