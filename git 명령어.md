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