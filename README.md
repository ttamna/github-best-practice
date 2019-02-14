# GIT, GITHUB RULE

> 효율적인 협업을 위해서, 깃과 깃헙 사용 규칙을 정해 놓읍시다.

## Repository organisation

* One conceptual group per repository (the conceptual group size/scope may vary between projects)
* Define codeowners for each repository [code owner](https://help.github.com/articles/about-code-owners/)
* Don't [commit](#commit) things that can be regenerated from the code
* Use pre-commit hooks for code-checking [code quality](#Code-quality)
* Use continuous integration where appropriate (e.g. [`TravisCI`](https://travis-ci.org/))
* Use code quality integration where appropriate[code quality](#Code-quality)
* Use test coverage integration where appropriate (e.g. [`codecov.io`](https://codecov.io/gh))
* Don't change the published history
* Avoid large binary files, but if you can't avoid it, use `git-lfs` to store them

* align package version
* lock package version
* archive dead repositories

## Day-to-day working

* `git pull` before you work; `git push` when you're done
* Review code *before* you check it in
* Make small commits that implement a single logical change and can be described in a simple statement
* Commit early and often
* Write your commit messages as imperative statements * see [this](https://chris.beams.io/posts/git-commit/)
* Use SSH keys to log in if possible

## Workflow

> 제품 특성에 따라 다른 전략이 필요할 수 있다

* **Use the workflow that is customary in your group**

### Release가 명확한 제품

* 업데이트가 바로 반영될 수 없는 제품
* 프로그램이 클라이언트 측에서 동작
* [`git flow`](#Git-flow)

### Release가 명확하지 않은 서비스

* 팀 규모가 크거나, 수시로 업데이트가 되고 바로바로 반영되는 서비스
* 프로그램이 서버 측에서 동작
* [`github flow`](#Github-flow)

## Git flow

[git-flow](https://github.com/nvie/gitflow)
> `팀 규모가 작거나, 릴리즈가 없는 서비스에 적합`한 전략으로 Github flow에 비해 단순하다

### Git flow 특징

* 5가지 종류의 브랜치로 구성된다
  * master: 제품으로 출시될 수 있는 브랜치
  * develop: 다음 출시 버전을 개발하는 브랜치
  * feature: 기능을 개발하는 브랜치
  * release: 이번 출시 버전을 준비하는 브랜치
  * hotfix: 출시 버전에서 발생한 버그를 수정하는 브랜치

### Git flow 프로세스

* ![gitflow](https://nvie.com/img/git-model@2x.png)
* [상세](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html)
* [참고1-Github](https://datasift.github.io/gitflow/IntroducingGitFlow.html)
* [참고2-Woowabros](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)

## Github flow (Forking Worklfow)

[github-flow](http://scottchacon.com/2011/08/31/github-flow.html)

> `Release가 있는 제품에 적합`한 전략으로, 명확한 구조 위에서 작업한다

### Github flow 특징

* Upstream Repository, Origin Repository, Local Repository 로 구성된다
  * `Upstream Repository`는 다른 개발자들과 공유하는 원격 저장소로, 최신 정보를 유지하고, 배포에 사용된다
  * `Origin Repository`는 Upstream Repository로부터 복사한 원격 저장소로, 개인이 사용한다
  * `Local Repository`는 내 컴퓨터에 있는 저장소로, 개발할때 사용한다

* 아주 큰 규모의 분산된 팀에서도 안전하게 협업하기에 좋은 협업 방법
* 오픈소스 프로젝트에서 많이 사용하는 방식
* `CI`가 필수적이다
* `master branch`에만 정해진 규칙이 있다
* `master branch`는 안정 버전을 유지해야 하므로, `merge`하기 전에 충분히 테스트해야 한다

### Github flow 프로세스

* `Upstream Repository`로부터 `fork` (`Origin Repository`가 생긴다)
* `Origin Repository`로부터 `clone` (`Local Repository`에서 개발할 준비가 된다)
* Branch 생성
  * github flow에서 Local Repository, Origin Repository의 branch 생성은 자유롭다
  * 이름은 잘 지어야 한다. 브랜치 이름을 보면, 어떤 작업인지 알 수 있도록
  * `branch`는 항상 `master branch`에서 만든다

    ```bash
    git checkout -b <new_branch> master
    ```

* 자주 `push` 해 자신이 무엇을 하고 있는지 동료들과 공유한다
* 피드백이나 도움이 필요할때, 그리고 머징 준비가 완료됐을때는 `pull request`를 생성한다
  * 제목에 '도움이 필요해요', '검토좀 해주세요', '머지해도 됩니다' 로 구분한다
  * `@mention`으로 쉽게 다른 사람에게 검토를 요청할 수도 있다.
  * `pull request`는 브랜치를 두고 토론하는 것으로, 브랜치 히스토리가 업데이트되면, 그 최신정보도 자동으로 포함된다

* 기능에 대한 리뷰와 테스트가 끝난후 `master`로 머지한다
  * 테스트는 로컬에서 하는 것이 아니라 브랜치를 `push`하고 `Jenkins`로 테스트한다
  * push하면 자동으로 `pull request`가 닫힌다(?todo: 무슨내용인지 확인필요)

* 일단 master에 `merge` 하면 바로 deploy한다.
  * hubot으로 [deploy](#Deploy)한다. github은 최근 Jenkins + Hubot + Github을 묶은 Janky를 공개했다.

## 상세

### Separate

* separate configuration files from source code
* separate secret credentials from source code [valut](https://www.vaultproject.io/), [aws secret manager](https://aws.amazon.com/secrets-manager/), [pre-commit hook](https://github.com/git/git/blob/master/templates/hooks--pre-commit.sample)

### Branch

* protect the main branches from direct commits [gitflow workflow](https://nvie.com/posts/a-successful-git-branching-model/), [branch protection](https://help.github.com/articles/configuring-protected-branches/)
* protect your project (master branch를 보호하기 위해 pull requests로만 merge 되도록 하자)
* for every new feature, start a new branch named after that feature
* once the branch is stable, create a pull request

### Commit

* how to write a `git` commit message: [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/)
* writing good commit messages: [https://github.com/erlang/otp/wiki/Writing-good-commit-messages](https://github.com/erlang/otp/wiki/Writing-good-commit-messages)
* avoid committing dependencies into project [ref](https://github.com/github/git-sizer)
* commit regularly (it can make you to find problem easily)
* avoid merge commits [링크](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/)

### Pull Request

* always pull the latest updates (for avoid resolving unnecessary merge conflicts)
* keep history consistent ([squash](https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0)브랜치에서 작업하고 병합하기 전, 커밋을 하나로 스쿼시 하는 것이 좋다. 일관되고 쉽게 읽을 수 있게 만들어준다)
* rebase your branches periodically (큰 기능을 작업할때, 브랜치를 정기적으로 리베이스해 병합을 수행할때 충돌을 피하자)

* choose the right developers for the job (팀원중 가장 숙련된 개발자에게만 pull requests를 merge할 권한을 부여하자)
* the more approvals, the better (둘 이상의 개발자가 승인했을때 pull requests가 merge 되도록 하자)
* review other's code independently

### Useful `.gitconfig` aliases

```bashrc
alias.lol=log --graph --decorate --pretty=oneline --abbrev-commit
alias.lola=log --graph --decorate --pretty=oneline --abbrev-commit --all
alias.hist = log --graph --pretty=format:'%h %ad | %s%d [%an]' --date=short
```

### `.gitignore`

* create a meaningful .gitignore file for your projects[ref](https://gitignore.io/)

### with Jira

* make time tracking easier (JIRA와 같은 프로젝트 추적 소프트웨어를 사용할 때 진행 상황을 추적하기가 쉽게 티켓에 branch 이름을 추가하는게 좋다)

## Integration

### Code Quality

* write better code([learn about code review](https://github.com/features/code-review))
* [`pylint`](https://www.pylint.org/), [`pycodestyle` aka `pep8`](https://pypi.python.org/pypi/pep8) for Python
* [Shellcheck](https://github.com/koalaman/shellcheck) may be useful for `bash` scripts)

* [`Landscape.io`](https://landscape.io/)

### CI

* find the right tools([learn about integrations](https://github.com/features/integrations))

### Deploy

* [Hubot](https://hubot.github.com/)

### Jira

TODO: JIRA

## Useful hints

### Useful quotes

> I have certain mantras that I use to guide my programming. They generally revolve around this theme: "Thinking is hard, and I'm not very good at it; every block of code should be simple and obvious, because if it makes me think, I'll probably screw it up and break something." * [Remy Porter](http://thedailywtf.com/articles/the-refactoring)

### Oh, shit

* Oh, shit, `git`!: [http://ohshitgit.com/](http://ohshitgit.com/)
* rewrite; don't rage
  * 실수로 브랜치를 삭제하거나 잘못 머지했을때, 멘탈이 부서지고 강제로 push할수 있는데 그러지 말자
  * git undo 기능이 있다. 침착하자.
  * branch를 하나 더 만드는 것을 두려워하지 말자(실수했으면 지우고 브랜치를 새로 만들어서 작업하자)
  * remote 저장소의 내용이 잘못됐으면, local에서 바로잡고, 강제로 push 할수 있다(동료에겐 다시 pull 받으라고 할 수 있다)

### Use the Source!

* manage your chaos([learn about project management](https://github.com/features/project-management))

* Frank Carey's Git Best Practice: [https://github.com/frankcarey/git-best-practices](https://github.com/frankcarey/git-best-practices)
* GitHub best practices: [https://blog.bloc.io/github-best-practices/](https://blog.bloc.io/github-best-practices/)
* Islandora's Git Guidelines: [https://github.com/Islandora/islandora/wiki/Git-Guidelines-and-Best-Practices](https://github.com/Islandora/islandora/wiki/Git-Guidelines-and-Best-Practices)
* Seth Robertson's best practices: [https://sethrobertson.github.io/GitBestPractices/](https://sethrobertson.github.io/GitBestPractices/)
* USGS best practices for code development: [https://github.com/usgs/best-practices](https://github.com/usgs/best-practices)
* Chris Said So: [https://chriskottom.com/blog/2014/02/a-few-modest-best-practices-for-git/](https://chriskottom.com/blog/2014/02/a-few-modest-best-practices-for-git/)
