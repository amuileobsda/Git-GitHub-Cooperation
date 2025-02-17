# Git-GitHub-Cooperation
Git Flow 전략을 사용한 협업 시나리오 입니다. 

# part1

1. 팀장이 프로젝트 세팅 합니다. (React 탬플릿 설정 등)

```
git clone {프로젝트 repo}
```

1. develop 브랜치를 생성합니다.

이 dev 브랜치에서 추후 생성될 각 기능 별 개발 브랜치인 topic의 내용들을 모아 저장합니다.

```jsx
git checkout -b dev

// git push origin main
// git push origin dev
git push --all // 위 두 명령어 한번에, 이건 최초에 해주는게 좋음
```

이러면 main, dev 브랜치 2개가 생성 되어있을 것입니다.

1. 레포지토리 Setting 
- Manage access ⇒ 팀원들 add people ⇒ 팀원들이 초대 accept
- 팀장이 브랜치 보호를 해줘야 함 (아무나 push 못하게)
    - Branches
        
        ⇒ Add rule: main 브랜치 Require a pull request before merging
        
        ⇒ Add rule: dev 브랜치 Require a pull request before merging
        

이렇게 하지 않으면 전부 다 dev 브랜치에 push를 마음대로 하게 되어서 log가 꼬이게 된다.

따라서 브랜치를 보호해서 팀장이 PR을 받아 승인하게 해야 한다.

1. 팀원이 dev 브랜치에 push 하려면?

팀원(meta)는 추후 복구를 대비하여 main, dev 브랜치를 모두 동기화 시켜주는게 좋습니다.

팀원은 topic 브랜치를 하나 더 만든 후 dev 에 merge 요청을 해야 합니다. (Pull Request)

그러면 팀장이 PR 확인 후 승인하거나 거부 요청을 보내면 됩니다.

---

# part2

### 팀원 시점에서 진행됩니다.

- 프로젝트 repo 를 clone

```jsx
git clone {프로젝트 repo}
```

이 시점에서 현재 저장소에는 main 브랜치만 있을 것입니다.

- 작업영역에 dev 브랜치를 동기화

```jsx
git checkout -b dev origin/dev
```

이 시점에서 현재 저장소에는 main, **dev** 브랜치가 있을 것입니다. 

이 dev에서는 개발을 하면 안됩니다. 만약 팀원이 이 dev 브랜치에서 바로 작업 후 push를 해버리면 다른 팀원들과의 로그가 꼬일 수 있기 때문입니다.

이것은 push가 바로 되게끔 설정한 팀장 잘못입니다. 이를 방지하기 위해 branch를 protect 하는 것이고요. 예를 들어 팀원에게 PR 요청을 거부하고

```jsx
“rebase를 통해서 commit log를 정리한 다음 git push -f origin topic 으로 PR 요청을 다시 보내라” 
```

라고 말할 수 있는겁니다.

- 토픽 만들기

예를 들어 회원가입 기능을 작업할 ‘join_topic’ 브랜치를 만든다고 가정해 봅시다.

```jsx
git checkout -b join_topic
```

join_topic 브랜치에서 회원가입 기능을 완성하고 잘 돌아가는 것을 확인했습니다. 그검 이 상태에서 바로 push를 해줍니다.

```jsx
git push origin join_topic
```

이 시점에서 현재 저장소에는 main, dev, **join_topic** 브랜치가 있을 것입니다. 아직 dev에는 merge가 안되었기 때문에 PR을 보내야 합니다.


“dev 에 join_topic을 merge해도 될까요?” ⇒ `Create pull request`

위와 같이 요청을 보냈으면 다음과 같이 block 되었다고 알려줄겁니다. 


이 상태에서는 팀장이 승인을 해야 merge를 할 수 있습니다. 따라서 팀장은 Settings ⇒ Notifications 에서 PR 요청이 오면 이메일을 통해 알 수 있도록 트리거를 걸어놓으면 좋습니다.

Pull request에서 Commits ⇒ Review changes 를 클릭해보면 다음과 같은 창이 나옵니다.


```
Comment: draft와 마찬가지로 리뷰를 해주는 용도입니다. (이건 이렇게 하는게 더 좋겠다~ etc)

Approve: 승인한다는 뜻입니다.

Request changes: 거절한다는 뜻입니다. (이거 잘못되었으니 수정해라~ etc)
```

팀원이 회원가입 기능을 아주 잘 만들었으므로 Approve를 선택 후 `Submit review` 를 해줍니다. 그러면 이제 팀장은 `Merge pull request`를 할 수 있게 됩니다.

하지만 매번 모든 팀원에게 merge를 해주고 알려주는 것은 번거로울 수 있기 때문에 승인만 해주고 팀원에게 스스로 `Merge pull request`를 하게 할 수도 있습니다. (개인적으로는 팀장이 직접 하는 것을 추천)

이렇게 merge PR 을 해주면 이제 dev 브랜치에도 회원가입 기능이 존재하게 됩니다.

팀장은 이제 join_topic 브랜치를 삭제해도 되기는 합니다. 하지만 팀장이 날리는게 더 좋겠죠. 다음 명령어를 통해 로컬에서만 브랜치를 삭제할 수 있습니다.

```
git push --delete origin join_topic
```

이 시점에서 팀원의 작업 영역에는 아직 **join_topic**이 남아 있을겁니다. 

혹시 모르니 토픽 브랜치는 삭제하지 않고 가지고 있는 것이 좋지만 깃허브 저장소에는 남겨놓을 필요가 없기 때문에 위처럼 로컬에만 push —delete를 한 것입니다.

### 이 모든 과정을 팀원모두가 다 외울 수 있을까

쉽지 않을 수 있습니다. 따라서 팀장이 다음과 같이 문서화를 시켜놓는 것이 좋습니다.

ex) 팀원 시점

```
0. dev 브랜치 pull
1. dev에서 topic 브랜치 생성 (기능_topic)
2. topic 브랜치에서 개발 진행
3. log 지저분하면 rebase
4. topic을 push
5. PR 요청 보냄
6. 그러면 팀장이 승인을 하고 merge를 해줄거임
7. merge가 완료되면 github에 branch는 삭제하시오
8. dev 브랜치 pull (아니면 fetch)
```

이런 프로토콜을 만들어 놓아야 협업하기 편합니다. 그리고 팀원들은 프로토콜을 살펴보며 충돌 및 오류를 방지할 수 있습니다. 

ex) 다 했는데 까먹고 dev 브랜치를 동기화 시키지 않고 다음 topic을 생성하는 문제 etc

---

지금까지는 팀원이 topic 생성 후 회원가입 기능을 만들고 push 후 PR을 보내고 팀장이 **승인**하는 시나리오를 작성해 보았습니다. 그럼 만약 팀장이 PR을 **거절**하게 되면 어떤 일이 벌어질까요?

# Part3

우선 회원가입 기능을 개발했으니 로그인 기능을 개발한다고 칩시다. 팀원은 이전에 작성해놓은 프로토콜을 보고 개발하면 되겠죠?

```
1. topic 브랜치 생성
2. topic 브랜치에서 개발 진행
3. log 지저분하면 rebase
4. topic을 push
5. PR 요청 보냄
6. 그러면 팀장이 승인을 하고 merge를 해줄거임
7. merge가 완료되면 github에 branch는 삭제하시오
8. 그리고 dev 브랜치 pull (아니면 fetch)
```

login_topic 브랜치를 만들어서 개발을 진행합니다.

```jsx
git checkout -b login_topic // 1. topic 브랜치 생성
```

개발을 진행하고, 커밋을 여러 개 한 후, push를 해주었습니다. 팀원은 문제가 없는 줄 알고 팀장에게 PR을 보냈지만 사실 `3. log 지저분하면 rebase` 과정을 빼먹었습니다.

---

이런 상황에서 팀장이 PR을 확인하고 택할 수 있는 선택지는 다음과 같습니다.

### 1. Squash merge

첫 번째 방법은 그냥 승인 해주고 팀장 본인이 squash merge를 하는 방법입니다.추천하는 방법은 아닙니다. 이렇게 되면 팀원이 다음에도 실수할 가능성이 있기 때문이죠.


### 2. Request changes (PR 거절)

다음 방법은 PR을 아예 거절하는 방법입니다. 어떻게 수정하면 좋을 지 알려줄 수 있습니다.


     팀원은 PR에서 거절당한 로그를 확인 후 자신이 `rebase` 를 하지 않았다는 사실을 깨달을 겁니다.

바로잡기 위해 팀원은 rebase를 진행하고 push를 진행하겠죠? 


```jsx
git rebase -i HEAD~4 //"3. 로그인 완료" 로 commit 메시지 작성
git push -f origin login_topic
```

`-f` 를 붙여서 강제로 push를 해주는 이유는 현재 rebase 된 commit 로그와 리모트의 commit 로그가 달라서 일반적으로 push하면 거절당하기 때문입니다.

rebase가 완료되었으므로 이제 팀장이 PR요청을 Approve 해주면 됩니다!

이 시점에서 이제 `dev` 브랜치에 로그인 기능이 잘 들어왔을 겁니다. 이제 팀원은 나머지 단계를 진행하면 됩니다.

```jsx
7. merge가 완료되면 github에 branch는 삭제하시오
	 git push --delete origin login_topic

8. 그리고 dev 브랜치 pull (아니면 fetch)
	 git checkout dev
   git pull origin dev
```

## 개발이 끝났다

프로젝트과 완료되었으면 팀장은 이제 dev 브랜치의 내용을 main 에 끌어와야 합니다.

```jsx
git pull origin dev // dev 브랜치 동기화
git checkout main // main으로 이동
git merge --no--ff dev // dev의 내용을 main에 merge
```

버전 관리 툴 답게 출시할 커밋에 버전 태그도 붙혀봅시다.

```jsx
git tag v1.0.0
git push --tags origin main
```

이제 배포를 진행하면 출시가 완료된 겁니다!
