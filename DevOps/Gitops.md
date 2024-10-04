Gitops는 애플리케이션 배포와 운영에 대한 모든 요소를 코드화하여 Git에서 관리하는 방법론 중 하나.
## 장점
- 인프라를 포함한 모든 애플리케이션 배포와 운영에 관련된 모든 것을 코드로 관리
- Git을 단일 진실 공급원으로 지정하고 이 곳에서만 관리한다: 단방향 운영 시스템 > 시스템 구축에 용이하다: Circular 관계가 발생하지 않음(dependency를 생각해보자)
- Git 에서의 기능을 CD시 사용할 수 있다:  History / branch / merge ...
## ArgoCD
https://argo-cd.readthedocs.io/en/stable/
Manifest(.yaml) 파일을 주시하고, 변경이 일어나면(Commit - Push), Kubernetes에 적용해 준다.
![ArgoCD](https://1drv.ms/i/s!Ano-rmQ7e_nEwD4_QSGeuEZ_qMl6?embed=1&width=1067&height=660)
