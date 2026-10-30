# 깃허브 연습용 저장소

브랜치를 나눠 팀원끼리 자료를 공유하는 방법을 연습하는 저장소입니다.

## 브랜치 역할

| 브랜치 | 역할 |
| --- | --- |
| `main` | 팀원 브랜치. 공유된 자료가 모이는 곳 |
| `king2` | 내 작업 브랜치. 여기서 작업하고 main으로 보낸다 |

## 폴더 구성

| 폴더 | 내용 |
| --- | --- |
| `git/` | Git 공부 정리 (`git.md`, `git_branch 관리.md`) |

## 자료 공유 순서

1. king2를 main과 맞추기

   ```bash
   git switch king2
   git pull origin main
   ```

2. king2에서 공유할 폴더를 만들고 파일 넣기
3. 그 폴더만 커밋하고 올리기

   ```bash
   git add 폴더이름
   git commit -m "폴더이름 공유"
   git push
   ```

4. GitHub에서 Pull Request 만들기: `base: main` ← `compare: king2`
5. **Files changed**에 보낼 폴더만 있는지 확인하고 병합

## 규칙

- 내 브랜치에서 팀원 파일은 지우거나 옮기지 않는다
- 작업 전에 `git pull origin main`으로 main의 최신 내용을 가져온다
- `git add .` 대신 보낼 폴더나 파일 이름을 적는다
- 커밋이나 병합 전에 `git branch`로 현재 브랜치를 확인한다
