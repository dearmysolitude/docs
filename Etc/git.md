## git 저장소 구조
![git 구조](https://1drv.ms/i/s!Ano-rmQ7e_nEwAmcHRqrrIkRt9Y6?embed=1&width=1046&height=582)
## git 관리 전략
### git-flow
![git-flow](https://1drv.ms/i/s!Ano-rmQ7e_nEwAj82Z5Uc1k4NZO6?embed=1&width=510&height=676)
- **Master나 develop, release branch의 경우 극히 신중히 merge 해야한다.**
- hotfix 브랜치의 경우에는 머지한 master에서 버그가 발생하는 경우에만 사용. 마스터로 머지 후, develop으로도 머지한다.
- master: 여기에 대한 merge는 명확해야 함 - master 브랜치에서는 땡겨올 수 는 있지만 전략을 세워 develop에서만 가져오도록 하는 경우도 있음.
- hook: 커밋 메세지가 규칙대로, 코드 들여쓰기를 잘 하는지 검사하는 경우 사용
도표를 살펴보면 쉽게 납득 가능. 하지만 세부적인 내용은 팀에 따라 규칙을 정하여 운영하게 된다.
[git-flow](https://techblog.woowahan.com/2553/)
## 명령어
`$ git branch -d develop (develop 브랜치 삭제)`
`$ git checkout -b develop (branch 생성, checkout 한번에)`
### 로컬 저장소 원격으로 연결
`git remote add origin [git url]`: 현재 git 로컬 저장소를 orign으로 url에 remote 저장소로 더한다.
`git config --global credential.helper store`: Git 자격 증명을 저장하는 방식을 설정.
`git push --set-upstream origin --all`: origin을 원격 저장소로 지정하고 --all로 모든 브랜치를 모두 push한다. **upstream**: 로컬 저장소가 추적하는 원격 저장소(remote repository)를 가리키는 용어. 
> [!note] Git Credential - Claud AI
> Git은 원격 저장소와 통신할 때 사용자 인증을 위해 자격 증명(보통 사용자 이름과 비밀번호)을 요구합니다. 이 자격 증명을 매번 입력하는 것은 번거로울 수 있으므로, Git은 자격 증명을 저장하는 여러 가지 방법을 제공합니다.
> `credential.helper store` 옵션은 자격 증명을 평문(plain text) 파일에 저장하는 방식입니다. 이 파일은 `.git-credentials` 파일이며, 사용자의 홈 디렉토리에 저장됩니다.
> 이 옵션을 사용하면 Git은 자격 증명을 입력하라는 메시지가 나올 때마다 사용자의 자격 증명을 기억하고 저장합니다. 이후에는 자동으로 저장된 자격 증명을 사용하여 인증을 수행합니다.
> 그러나 이 방식은 자격 증명을 평문으로 저장하기 때문에 보안 상의 문제가 있을 수 있습니다. 따라서 중요한 프로젝트에서는 `credential.helper`의 다른 옵션(예: `cache`, `manager` 등)을 사용하거나 별도의 자격 증명 관리 도구를 사용하는 것이 좋습니다.
### Merge/Rebase
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217915&authkey=%21ADO_z_4_vUYKQk8&width=622&height=469" alt="">
  <figcaption>Merge</figcaption>
</figure>
- Three-Way Merging
	- `git merge --no-ff`
	- 브랜치 기록을 유지하면서 머징
	- **어떤 기능이 추가되었는지 알기 쉽도록 팀 프로젝트에서는 이런 방식으로 하는 것이 좋다.**
- Fast-Forward Merging
	- `git merge`
	- plain, default로 적용되는 merge 옵션.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217916&authkey=%21AIrkV3LQ-Lmtv5Y&width=501&height=283" alt="">
  <figcaption>Rebase</figcaption>
</figure>
### Reset
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217917&authkey=%21AMXJH7wDgtvgj64&width=488&height=260" alt="">
  <figcaption>Reset: Soft, Mixed, Hard</figcaption>
</figure>
### Stash
로컬에서 작업한 소스 내용을 임시로 다른 곳에 저장. working tree에 대해서만 저장되며 충돌 역시 발생할 수 있다.
`$ git add -A`
`$ git stash`
`$ git stash list`
`$ git stash apply`
### Tag
`$ git tag v1.0`
`$ git tag -d v1.0`
### Cherry Pick
특정 커밋만 가져와 커밋
`$ git cherry-pick [commit-hash]`
### Revert
`$ git revert [commit-hash]`
### Fetch
브랜치 메타데이터만 불러온다.
`git fetch -all`: 해당 브랜치에 대한 메타데이터(변동사항)을 불러와
`git reset –hard origin/develop`: 해당 커밋으로 강제로 이동한다.
### Pull
Fetch + Checkout