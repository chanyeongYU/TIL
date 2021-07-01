# Git 유용한 명령어들



### 이전 커밋으로 reset

- `git reset --hard [commit number]`
- `--hard` 대신 `--soft` 등 옵션이 존재
  - `--hard` 는 해당 커밋 이후 작성한 코드를 모두 삭제후 완전히 이전 상태로 돌아감
- reset한 코드를 원격 저장소에 반영하고 싶을 때는 `git push -f origin [branch]`



### terminal에서 git log 그래프로 확인하기

- `git log --oneline --graph --decorate`



### git 게정 설정

- 전체 레포지토리에 설정할 경우

  - `git config --global user.name "이름" `
  - `git config --global user.email "이메일"`

  

- 특정 레포지토리에만 설정할 경우

  - 회사에서는 빗버킷을 사용하고 집애서는 깃헙을 사용하기 때문에 레포별로 별도의 설정이 필요할 경우가 있다.

  - `--local`으ㄴ `--global` 보다 높은 우선순위를 가짐

  - `git config --local user.name "이름" `

  - `git config --local user.email "이메일"`

    

