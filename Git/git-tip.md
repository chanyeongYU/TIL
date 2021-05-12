# Git 유용한 명령어들



### 이전 커밋으로 reset

- `git reset --hard [commit number]`
- `--hard` 대신 `--soft` 등 옵션이 존재
  - `--hard` 는 해당 커밋 이후 작성한 코드를 모두 삭제후 완전히 이전 상태로 돌아감
- reset한 코드를 원격 저장소에 반영하고 싶을 때는 `git push -f origin [branch]`



### terminal에서 git log 그래프로 확인하기

- `git log --oneline --graph --decorate`

