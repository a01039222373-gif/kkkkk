# Git 기본 정리

## 커밋(commit)이란

지금 파일 상태를 하나의 기록으로 저장하는 것. 게임의 세이브 포인트와 비슷하다.

| Git | 게임 |
| --- | --- |
| 파일 수정 | 게임 진행 |
| `git add` | 저장할 내용 고르기 |
| `git commit` | 세이브 |
| `git push` | 세이브 파일을 클라우드(GitHub)에 올리기 |
| `git revert` | 특정 세이브에서 한 일을 취소하는 새 세이브 만들기 |

커밋에 들어 있는 것:

- **변경 내용**: 어떤 파일의 어떤 줄이 추가되거나 삭제됐는지
- **메시지**: `-m "..."`에 쓴 설명
- **작성자와 시간**
- **고유 번호**: `1f3fd84` 같은 값. 이 번호로 특정 커밋을 가리킨다

커밋 결과 읽는 법:

```
[main 1f3fd84] king2 파일 삭제
 1 file changed, 1 deletion(-)
 delete mode 100644 README.md
```

- `main`: 커밋이 저장된 브랜치
- `1f3fd84`: 커밋 번호
- `king2 파일 삭제`: 커밋 메시지
- `delete mode ... README.md`: README를 삭제했다는 기록

## add, commit, push

```
파일 수정 ──git add──▶ 스테이징 ──git commit──▶ 내 컴퓨터에 기록 ──git push──▶ GitHub
```

- **add**: 이번 커밋에 넣을 파일 고르기
- **commit**: 내 컴퓨터에 기록 남기기 (GitHub에는 아직 안 올라감)
- **push**: 쌓인 커밋을 GitHub에 올리기

**push는 커밋만 올린다.** 수정, 삭제, 추가 모두 커밋을 해야 GitHub에 반영된다.

```bash
git add .
git commit -m "변경 내용 설명"
git push
```

- 커밋 없이 push하면 `Everything up-to-date`(올릴 게 없음)가 나온다
- 바뀐 파일 없이 커밋하면 `nothing to commit, working tree clean`이 나온다
- 커밋은 여러 번 하고 push는 한 번에 해도 된다
- `git status`에 `Your branch is ahead of 'origin/main' by 1 commit`이 나오면 push 안 한 커밋이 1개 있다는 뜻

## 브랜치 만들고 GitHub에 올리기

`git switch -c 이름`은 내 컴퓨터에만 브랜치를 만든다. GitHub에는 직접 올려야 생긴다.

```bash
git switch -c king      # 브랜치 생성 + 이동
git push -u origin king # GitHub에 처음 올리기
git push                # 이후에는 이것만
```

- `-u`: 로컬 브랜치와 GitHub 브랜치를 연결. 처음 한 번만

자주 나오는 오류:

| 오류 | 원인 | 해결 |
| --- | --- | --- |
| `'origin' does not appear to be a git repository` | GitHub 저장소 연결 안 됨 | `git remote add origin https://github.com/아이디/저장소.git` |
| `src refspec king does not match any` | 커밋이 하나도 없음 | `git add .` → `git commit -m "첫 커밋"` |

## 한 브랜치에서만 파일 삭제하기

브랜치마다 파일 상태가 따로 저장된다. king2에서 지워도 main에는 남아 있다.

```bash
git switch king2
git branch              # * king2 확인!
git rm README.md        # 폴더는 git rm -r 폴더이름
git commit -m "king2 README 삭제"
git push
```

- `git rm`은 삭제와 기록을 같이 해서 `git add`가 따로 필요 없다
- 탐색기에서 직접 지웠다면 `git add .` 후 커밋
- king2를 main에 병합하면 main에서도 파일이 지워진다

## 실수로 다른 브랜치에서 커밋했을 때

king2에서 지우려던 README를 main에서 지우고 push해 버린 경우:

```bash
git switch main
git revert 1f3fd84 --no-edit   # 삭제 커밋 되돌리기
git push
```

- `revert`는 기록을 지우지 않고 취소하는 커밋을 새로 만든다. 이미 push한 커밋을 되돌릴 때 안전하다
- 커밋 번호는 `git log --oneline`으로 확인

## 습관

- `git rm`, `git commit`, `git merge` 전에는 `git branch`로 `*` 위치 확인
- PowerShell 프롬프트에는 현재 브랜치가 표시되지 않는다
