# Git cheat sheet

Git tutorial([#](/README.md)) 내용을 바탕으로 명령어별로 Git 사용법 요약

### [기초]
|명령어|설명|
|--|--|
|<code> git init</code>|현재 폴더를 Git으로 관리시작|
|<code> git status </code>|변경사항 확인|
|<code> git clone *(원격 저장소 주소)* </code>|원격 저장소에서 프로젝트 다운|

### [add]
|명령어|설명|
|--|--|
|<code> git add *(파일명)*</code>|파일 하나 선택</br>(Working directory → Staging area)|
|<code> git add .</code>|모든 파일 선택|
|<code> git add -p</code>|hunk별로 선택|

### [branch]
|명령어|설명|
|--|--|
|<code>git branch</code>|브랜치 목록 확인|
|<code>git branch --all</code>|└ 원격의 브랜치까지 확인 (-a도 가능)</br>(<code>git fetch</code> 필요)|
|<code>git branch *(생성할 브랜치명)*</code>|브랜치 생성</br>(이동: <code>git switch *(브랜치명)*</code>)|
|<code>git branch -d *(삭제할 브랜치명)*</code>|브랜치 삭제|
|<code>git branch -D *(삭제할 브랜치명)*</code>|다른 브랜치로 안가져왔더라도 브랜치 강제 삭제|
|<code>git branch -m *(기존 브랜치명)* *(새로운 브랜치명)*</code>|브랜치명 변경|

### [checkout]
|명령어|설명|
|--|--|
|<code>git checkout HEAD^</code>|1개 커밋 전으로 이동 (삭제가 아님)|
|<code>git checkout HEAD^^^</code>|3개 커밋 전으로 이동|
|<code>git checkout HEAD^10</code>|10개 커밋 전으로 이동|
|<code>git checkout *(커밋 해시)*</code>|특정 커밋의 시점으로 이동|
|<code>git checkout *(태그이름)*</code>|특정 태그의 시점으로 이동|
|<code>git checkout -</code>|이전 이동 하나 취소 (Ctrl+Z 느낌)|

### [commit]
|명령어|설명|
|--|--|
|<code> git commit </code>|선택한 파일 커밋</br>(Staging area → Local Repository)|
|<code> git commit -m "*(메시지)*" </code>|└ 메시지도 함께 입력|
|<code> git commit -v </code>|현재 커밋에서의 변경사항을 확인하며 커밋함|
|<code> git commit -am "*(메시지)*" </code>|untracked 없을 때 add+commit 한번에|
|<code>git commit --amend</code>|마지막 커밋 메시지 수정</br>/add후 마지막 커밋에 같이 포함|
|<code>git commit --amend -m "*(메시지)*"</code>|└ 메시지도 함께 입력|

### [config]
|명령어|설명|
|--|--|
|<code> git config --global username "*(이름)*"</code>|이름 설정|
|<code> git config --global username "*(이메일)*"</code>|이메일 설정|
|<code> git config --global init.defaultBranch main</code>|기본 브랜치명을 main으로 변경|
|<code>git config --list</code>|local 설정 확인|
|<code>git config --global --list</code>|global 설정 확인|
|<code>git config -e</code>|local 설정 에디터에서 확인|
|<code>git config --global -e</code>|global 설정 에디터에서 확인|
|<code>git config --global core.editor "code --wait"</code>|에디터를 VSCode로 변경, 수정중 CLI정지|
|<code>git config --global alias.*(내가쓸단축키)* *(원래 명령어)*</code>|단축키 설정|

### [diff]
|명령어|설명|
|--|--|
|<code>git diff</code>|Staging area에 올리기 전 변경사항 확인|
|<code>git diff --name-only</code>|└ 변경사항 있는 파일 목록만 확인|
|<code>git diff --staged</code>|Staging area에 올린 후 확인 변경사항 확인|
|<code>git diff *(커밋해시/HEAD번호)*</code>|현재 커밋과의 차이 확인|
|<code>git diff *(커밋1)* *(커밋2)*</code>|두 커밋간의 차이 확인|
|<code>git diff *(브랜치1)* *(브랜치2)*</code>|두 브랜치간의 차이 확인|

### [help]
|명령어|설명|
|--|--|
|<code>git help</code>|기본적인 설명 보기|
|<code>git help -a</code>|모든 명령어 설명 보기|
|<code>git *(명령어)* -h</code>|특정 명령어 설명 및 옵션 보기|
|<code>git *(명령어)* --help</code>|특정 명령어 설명 및 옵션 웹페이지에서 보기</br>(안 열릴 경우 -w 추가)|

### [log]
|명령어|설명|
|--|--|
|<code> git log </code>|커밋 내역 확인|
|<code>git log -p</code>|각 커밋마다의 변경사항 함께 보기|
|<code>git log -*(개수)*</code>|최근 n개의 커밋 보기|
|<code>git log --stat</code>|간략한 통계와 함께 보기|
|<code>git log --shortstat</code>|더 간략하게 보기|
|<code>git log --oneline</code>|한 줄로 짧게 보기|
|<code>git log --all --decorate --oneline --graph</code>|브랜치 내역을 한 눈에 보기|
|<code>git log -S *(검색어)*</code>|파일의 변경사항을 기준으로 검색|
|<code>git log --grep *(검색어)*</code>|커밋 메시지를 기준으로 검색|

### [merge]
|명령어|설명|
|--|--|
|<code>git merge *sub*</code></br>(*main*브랜치에서)|*sub* 브랜치를 *main* 브랜치로 merge</br>(두 브랜치를 하나의 커밋으로 합침)|
|<code>git merge --abort</code>|충돌 발생시 수정 보류 후 merge 중단시</br>(충돌을 수정한 경우 add, commit)|
|<code>git merge --squash *(다른 브랜치)*</code>|다른 브랜치에 있는 커밋들을 복제해서 하나의 커밋으로 묶은 뒤 뒤에 붙임|
|<code>git merge --no-ff *(병합할 브랜치명)*</code>|fast-forward 대신 3-way-merge를 하고 싶은 경우|

### [pull]
|명령어|설명|
|--|--|
|<code>git pull *origin* *main*</code>|원격에서 로컬으로 pull (Remote Repository → Local Repository)|
| <code>git pull --no-rebase *origin* *main*</code>|push하기전 pull해야 할 경우 (merge방식)</br> (충돌시 충돌 수정 후 <code>git add .</code>, <code>git commit</code>)|
|<code>git pull --rebase *origin* *main*</code>|push하기전 pull해야 할 경우 (rebase방식)</br> (충돌시 충돌 수정 후 <code>git add .</code>, <code>git rebase --continue</code>)|

### [push]
|명령어|설명|
|--|--|
|<code>git push *origin* *main*</code>|로컬에서 원격으로 push (Local Repository → Remote Repository)|
|<code>git push --force *origin* *main*</code>|로컬에서 원격으로 강제로 push (--force대신 -f 가능)|
|<code>git push -u *origin* *main*</code>|현재 속한 브랜치에서 push 할 때 *origin*의 *main* 브랜치에 push할 것임을 세팅</br>(이후 push/pull시 *origin*과 *main*을 생략하고 git push, git pull로 간단히 작업가능)|
|<code>git push -u *origin* *(로컬의 브랜치명)*</code>|로컬에서 브랜치 만든 걸 원격에 push|
|<code>git push *origin* --delete *(원격의 삭제할 브랜치명)*</code>|원격에서 브랜치 삭제|
|<code>git push *(원격이름)* *(태그이름)*</code>|특정 태그 원격에 올리기|
|<code>git push --tags</code>|모든 태그 원격에 올리기|
|<code>git push --delete *(원격이름)* *(태그이름)*</code>|특정 태그 원격에서 삭제|

### [rebase]
|명령어|설명|
|--|--|
|<code>git rebase *main*</code></br>(*sub*브랜치에서)|*sub* 브랜치를 *main* 브랜치로 rebase</br>(*sub* 브랜치를 떼어서 *main* 브랜치에 이어붙임)</br>이후 *main*브랜치에서 <code> git merge *sub*</code>로 fast-forward 필요|
|<code>git rebase --abort</code>|충돌 발생시 수정 보류 후 rebase 중단시</br>(충돌을 수정한 경우 add 후 아래 명령어)|
|<code>git rebase --continue</code>|└ 충돌 수정 후 add 뒤 계속 진행|
|<code>git rebase --onto *main* *A* *B*</code>|*A*브랜치에서 파생된 *B*브랜치를 *main*브랜치로 이동시켜 붙이고 싶을 때</br>(*A*브랜치는 그대로 두고 *B*브랜치만 *main*으로 이동)</br>이후 <code>git switch *main*; git merge *B*</code> 로 fast-forward|
|<code>git rebase -i *(수정할 커밋의 바로 전 커밋)*</code>|과거의 커밋들 수정/삭제/병합/분할</br> p, pick: 커밋을 그대로 둠</br> r, reword: 커밋 메시지를 변경</br> e, edit: 해당 커밋까지 된 상태로 돌아가 직접 수정</br> d, drop: 커밋 삭제</br> s, squash: 바로 이전 커밋과 합침|

### [remote]
|명령어|설명|
|--|--|
|<code>git remote add *origin* *(원격 저장소 주소)*</code>|로컬에서 원격 저장소 추가|
|<code>git remote</code>|원격 저장소 목록 보기|
|<code>git remote -v</code>|원격 저장소 목록 자세히 보기|
|<code>git remote remove *origin*</code>|로컬과 원격 저장소와의 연결 제거|

### [reset]
|명령어|설명|
|--|--|
|<code>git reset --hard *(마지막으로 남길 커밋 해시)*</code>|수정사항 완전 제거|
|<code>git reset --mixed *(마지막으로 남길 커밋 해시)*</code>|현재의 내용을 Working directory까지 남기고 reset (디폴트값)|
|<code>git reset --soft *(마지막으로 남길 커밋 해시)*</code>|현재의 내용을 Staging area까지 남기고 reset|
|<code>git reset --hard</code>|마지막 커밋한 시점으로 현재 파일/폴더 상태 초기화 (untracked는 그대로)|
|<code>git reset HEAD*단계* *(옵션)*</code>|HEAD 기준으로 reset|

### [restore]
|명령어|설명|
|--|--|
|<code>git restore --staged *(파일명)*</code>|커밋할 파일 선택해제</br>(Staging area → Working directory)|
|<code>git restore --staged .</code>|커밋할 파일 모두 선택해제|
|<code>git restore *(파일명)*</code>|커밋할 파일 선택해제 후 이전 커밋으로 되돌리기</br>(Staging area → 이전 Local Repository)|
|<code>git restore .</code>|커밋할 파일 모두 선택해제 후 이전 커밋으로 되돌리기|
|<code>git restore --source=*헤드 또는 커밋 해시* *(파일명)*</code>|커밋할 파일 선택해제 후 지정한 이전 커밋으로 되돌리기|
|<code>git restore --source=*헤드 또는 커밋 해시* .</code>|커밋할 파일 모두 선택해제 후 지정한 이전 커밋으로 되돌리기|

### [revert]
|명령어|설명|
|--|--|
|<code> git revert --hard *(거꾸로 돌릴 커밋 해시)* </code>|대상 커밋을 거꾸로 커밋|
|<code>git revert --continue</code>|revert 충돌시 처리 후 계속 진행|
|<code> git revert --no-commit *(거꾸로 돌릴 커밋 해시)*</code>|커밋없이 revert (Staging area까지)|

### [stash]
|명령어|설명|
|--|--|
|<code>git stash save</code>|현재 작업 치워두기</br>(tracking 파일에 해당, save 생략 가능)|
|<code>git stash -p</code>|원하는 것만 stash|
|<code>git stash -m *"(메시지)"*</code>|메시지와 함께 stash|
|<code>git stash list</code>|전체 stash 목록 보기|
|<code>git stash apply *stash@{1}*</code>|치워둔 *1번 stash* 적용|
|<code>git stash drop *stash@{1}*</code>|치워둔 *1번 stash* 삭제|
|<code>git stash pop *stash@{1}*</code>|치워둔 *1번 stash* 적용 후 삭제|
|<code>git stash apply</code>|마지막 stash 적용|
|<code>git stash drop</code>|마지막 stash 삭제|
|<code>git stash pop</code>|마지막 stash 적용 후 삭제|
|<code>git stash branch *(브랜치명)*</code>|새 브랜치를 생성 후 pop|
|<code>git stash clear</code>|치워둔 모든 stash 삭제|

### [submodule]
|명령어|설명|
|--|--|
|<code>git submodule add *(submodule의 GitHub 레포지토리 주소)* *(하위폴더명, 없을 경우 생략)*</code>|현재 프로젝트에 서브모듈 추가|
|<code>git submodule init *(특정 서브모듈 지정시 해당 이름)*; git submodule update</code>|메인 프로젝트 clone한 상태에서 서브프로젝트까지 가져와서 업데이트|
|<code>git submodule update --remote</code>|원격에서 submodule의 수정이 발생한 상황에서 업데이트|
|<code>git submodule update --remote --recursive</code>|└ 서브모듈 안에 서브모듈이 또 있을 경우|

### [switch]
|명령어|설명|
|--|--|
|<code>git switch *(이동할 브랜치명)*</code>|해당 브랜치로 이동|
|<code>git switch -c *(생성할 브랜치명)*</code>|브랜치를 생성하고 바로 이동|
|<code>git switch -t *origin*/*(원격의 브랜치명)*</code>|원격의 브랜치를 로컬로 가져올 때|

### [tag]
|명령어|설명|
|--|--|
|<code>git tag *(태그이름)*</code>|마지막 커밋에 lightweight 태그 달기|
|<code>git tag -a *(태그이름)*</code>|마지막 커밋에 annotated 태그 달기|
|<code>git tag *(태그이름)* -m "*(메시지)*"</code>|메시지와 함께 annotated 태그 달기|
|<code>git tag</code>|현재 태그 확인</br>(원하는 태그 내용 확인: <code>git show *(태그이름)*</code>)|
|<code>git tag -d *(태그이름)*</code>|태그 삭제|
|<code>git tag *(태그이름)* *(커밋 해시)* -m "*(메시지)*"</code>|원하는 커밋에 태그 달기|
|<code>git tag -l "*(태그이름)*"</code>|원하는 조건으로 필터링</br>(*로 와일드카드 사용가능)|

### [기타 명령어]
|명령어|설명|
|--|--|
|<code>git rm *(파일명)*</code>|파일 삭제 및 add|
|<code>git mv *(기존 파일명)* *(새 파일명)*</code>|파일명 변경 및 add|
|<code>git fetch</code>|pull과 달리 내 브랜치에 적용하지는 않은 채로 로컬로만 가져옴|
|<code>git cherry-pick *(다른 브랜치의 원하는 해시)*</code>|다른 브랜치에서 원하는 커밋만 복제해서 뒤에 붙이고 싶을 때 (이후 커밋)|
|<code>git clean</code>|Git에서 추적하지 않는 파일들 삭제</br> -n: 삭제 대신 대상 파일들의 리스트만 확인</br> -i: 인터렉티브 모드로 어떤 걸 삭제할지 선택</br> -d: 폴더 포함해 삭제</br> -f: 강제로 삭제</br> -x: .gitignore에 등록된 파일들도 삭제</br>(ex. <code>git clean -df</code>)|
|<code>git reflog</code>|Git에서 한 모든 활동 확인</br>(이후 원하는 작업의 해시 찾아 reset 등 가능)|
|<code>git blame *(파일명)*</code>|어떤 코드를 누가 작성했는지 확인|
|<code>git blame -L *(시작줄)* *(끝줄/+줄수)* *(파일명)*</code>|어떤 코드를 누가 작성했는지 확인 (line 지정)|
|<code>git bisect start</code>|여러 커밋 중 어느 시점에 오류가 발생했는지 이진 탐색 시작</br> 이후 <code>git bisect bad</code>, <code>git checkout *(의심 커밋 해시)*</code>,</br> <code>git bisect good</code> / <code>git bisect bad</code>반복,</br> 종료시 <code>git bisect reset</code>|