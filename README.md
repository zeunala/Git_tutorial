# Git 사용법 정리
얄팍한 코딩사전님의 제대로 파는 Git & GitHub 강의[(#)](https://www.inflearn.com/course/%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%8C%8C%EB%8A%94-%EA%B9%83?inst=a17e4bef)를 기초로 작성하였습니다.
### 목차
[1. Git 기본 설정](#1-git-기본-설정)  
[2. Commit](#2-commit)  
[3. Reset/Revert](#3-resetrevert)  
[4. Branch](#4-branch)  
[5. Merge/Rebase](#5-mergerebase)  
[6. 원격 저장소(GitHub) 사용](#6-원격-저장소github-사용)  
[7. Git의 여러 기능들](#7-git의-여러-기능들)  
[8. GitHub의 여러 기능들](#8-github의-여러-기능들)  
[9. 기타 사용법](#9-기타-사용법)  

## 1. Git 기본 설정
- 이름과 이메일 주소 설정
	- <code> git config --global username "*(이름)*" </code>
	- <code> git config --global username "*(이메일)*" </code>
- 기본 브랜치명을 main으로 변경
	-  <code> git config --global init.defaultBranch main</code>
- 현재 폴더를 Git으로 관리시작
	-  <code> git init</code>
- .gitignore 파일 예시
	```git
	# sample.c를 무시
	sample.c
	
	# 현재 폴더의 sample.c를 무시
	/sample.c
	# example 폴더 바로 안의 sample.c 무시
	example/sample.c
	# example 폴더 안의 sample.c 모두 무시
	example/**/sample.c
	
	# (...).c를 모두 무시하되 sample2.c는 무시안함
	*.c
	!sample2.c

	# 이름이 sample인 파일/폴더(그 안까지) 모두 무시
	sample

	# 이름이 sample인 폴더(그 안까지) 무시
	sample/
	```
## 2. Commit
- **Working directory - Staging area - Local Repository - Remote Repository** 를 기준으로 함
- 변경사항 확인
	-  <code> git status </code>
- 변경사항 상세 확인
	-  <code> git diff </code>
- 커밋할 파일 선택 (Working directory → Staging area)
	-  <code> git add *sample.c*</code> (sample.c 파일 하나)
	-  <code> git add .</code> (모든 파일)
	- <code> git add -p</code> (hunk별 스테이징)
		- <code>git diff --staged</code>로 이번 커밋에 담길 변경사항들을 볼 수 있다.
- 커밋할 파일 선택해제 (Staging area → Working directory)
	- <code>git restore --staged *(파일명)*</code>
- 커밋할 파일 선택해제 후 이전 커밋으로 되돌리기 (Staging area → 이전 Local Repository)
	- <code>git restore *(파일명)*</code>
		- Working directory에서도 제거하므로 파일 내용들을 변경한 것들도 사라지게 된다.
		- *(파일명)* 대신 . 를 입력하면 모든 파일을 대상으로 함
	- <code>git restore --source=*헤드 또는 커밋 해시* *(파일명)*</code>
- 선택한 파일 커밋 (Staging area → Local Repository)
	-  <code> git commit </code>
		- 이후 커밋 메시지 입력 가능
	-  <code> git commit -m "*(메시지)*" </code>
		- 커밋 메시지를 함께 입력함
	- <code> git commit -v </code>
		- 현재 커밋에서의 변경사항을 확인하며 커밋함 (<code>git diff --staged</code> 후 커밋하는 효과)
- 선택과 커밋을 한 번에 (Working directory → Local Repository)
	-  <code> git commit -am "*(메시지)*" </code>
		- 단, 새로 추가된 파일(untracked)이 있으면 안 된다.
- 마지막 커밋 메시지 수정 / add후 마지막 커밋에 같이 포함
	- <code>git commit --amend</code>
		- 이후 커밋 메시지 입력 가능
	- <code> git commit --amend -m "*(메시지)*"</code>
- 커밋 결과 확인
	-  <code> git log </code>
- 파일 삭제 및 add
	- <code>git rm *(파일명)*</code>
- 파일명 변경 및 add
	- <code>git mv *(기존 파일명)* *(새 파일명)*</code>

## 3. Reset/Revert
- Reset: 특정 시점으로 돌아가서 이후 커밋 삭제
	- ex. 1→2→3→4에서 2번대상 reset: 1→2로 만듦
- Revert: 특정 시점의 커밋을 거꾸로 실행
	- ex. 1→2→3→4에서 2번대상 revert: 1→2→3→4→-2
- reset 사용
	-  <code>git reset --hard *(마지막으로 남길 커밋 해시)*</code> (수정사항 완전 제거)
		- 커밋 해시는 <code>git log</code>로 확인 가능
	- <code>git reset --mixed *(마지막으로 남길 커밋 해시)*</code> (현재의 내용을 Working directory까지 남김)
		- --mixed는 default값이므로 생략 가능
	- <code>git reset --soft *(마지막으로 남길 커밋 해시)*</code> (현재의 내용을 Staging area까지 남김)
- 마지막 커밋한 시점으로 현재 파일/폴더 상태 초기화
	-  <code>git reset --hard</code>
		- 마지막 커밋 이후 새로 추가된 파일(untracked)은 그대로
- revert 사용
	- <code> git revert *(거꾸로 돌릴 커밋 해시)* </code>
		- 이후 커밋 메시지 입력 가능
		- 수정내역 충돌시 처리 후 <code>git revert --continue</code>
- 커밋없이 revert 사용 (Staging area까지)
	- <code> git revert --no-commit *(거꾸로 돌릴 커밋 해시)*</code>
- HEAD(현재 브랜치의 가장 최신 커밋) 기준으로 이동(삭제는 아님)
	- <code>git checkout HEAD^</code> (1개 커밋 전)
	- <code>git checkout HEAD^^^</code> (3개 커밋 전)
	- <code>git checkout HEAD^10</code> (10개 커밋 전)
	- <code>git checkout -</code> (이전 이동 하나 취소. Ctrl+Z 느낌)
		- <code>git checkout *(커밋 해시)*</code>로 이동할 수도 있다.
		- checkout 한 경우 현재 브랜치에서 익명의 브랜치로 이동하게 된다.
- HEAD 기준으로 reset
	- <code>git reset HEAD*단계* *(옵션)*</code>

## 4. Branch
- 브랜치 목록 확인
	-  <code>git branch</code>
		- 현재 브랜치가 *로 표시된다.
- *sampleBranch*이라는 이름의 브랜치 생성
	-  <code>git branch *sampleBranch*</code>
- *sampleBranch*이라는 이름의 브랜치로 이동
	-  <code>git switch *sampleBranch*</code>
- *sampleBranch*이라는 이름의 브랜치를 생성하고 바로 이동
	-  <code>git switch -c *sampleBranch*</code>
- *sampleBranch*이라는 이름의 브랜치 삭제
 	-  <code>git branch -d *sampleBranch*</code>
	-  <code>git branch -D *sampleBranch*</code>
		- 다른 브랜치로 가져오지 않은 내용이 있는 브랜치를 강제로 삭제할 때 사용
- 브랜치 이름을 *oldSample*에서 *newSample*로 바꾸기
	-  <code>git branch -m *oldSample* *newSample*</code>
- 브랜치 내역을 한 눈에 보기
	-  <code>git log --all --decorate --oneline --graph</code>
- 다른 브랜치에서 원하는 커밋만 복제해서 뒤에 붙이고 싶을 때
	- <code>git cherry-pick *(다른 브랜치의 원하는 해시)*</code>
- 다른 브랜치에 있는 커밋들을 복제해서 하나의 커밋으로 묶은 뒤 뒤에 붙이고 싶을 때
	- <code>git merge --squash *(다른 브랜치)*</code>
		- 이후 커밋한 후 메시지를 입력하면 된다.
- *A*브랜치에서 파생된 *B*브랜치를 *main*브랜치로 이동시켜 붙이고 싶을 때 (*A*브랜치는 그대로 두고 *B*브랜치만 *main*으로 이동)
	- <code>git rebase --onto *main* *A* *B*</code>
		- 이후 <code>git switch *main*; git merge *B*</code> 로 fast-forward 해주자
- 협업을 위해서 Git-flow 전략을 사용할 수 있다. [(#)](https://nvie.com/posts/a-successful-git-branching-model/)

## 5. Merge/Rebase
- Merge: 두 브랜치를 하나의 커밋으로 합침
- Revert: 한 브랜치를 떼어서 다른 브랜치에 이어붙임
- merge 사용(*subSample* 브랜치를 *mainSample* 브랜치로 merge)
	-  <code>git merge *subSample*</code> (*mainSample* 브랜치에서)
		- *mainSample* 브랜치로 이동한 후 위 명령어를 입력해야 한다.
		- 이후 커밋 메시지 입력 가능
		- merge도 하나의 커밋이므로 reset으로 되돌릴 수 있다.
		- 병합된 *subSample* 브랜치는 삭제해주자
		- 충돌이 발생할 경우 충돌 수정 후 <code>git add .</code>, <code>git commit</code>을 해주면 된다.
		- 충돌 해결이 어려울 경우 <code>git merge --abort</code>로 merge를 중단할 수 있다.
- rebase 사용(*subSample* 브랜치를 *mainSample* 브랜치로 rebase)
	1. <code>git rebase *mainSample*</code> (*subSample* 브랜치에서)
		- merge때와는 달리 *subSample* 브랜치로 이동한 후 위 명령어를 입력해야 한다.
		- 충돌이 발생할 경우 충돌 수정 후 <code>git add .</code>, <code>git rebase --continue</code>로 계속 진행할 수 있다.
		- 충돌 해결이 어려울 경우 <code>git rebase --abort</code>로 rebase를 중단할 수 있다.
	2. <code> git merge *subSample*</code> (*mainSample* 브랜치에서)
		- 1번만 할 경우 main 브랜치는 뒤쳐져 있기에 *mainSample* 브랜치로 이동한 후 위와 같이 fast-forward를 해주어야 한다.
		- 이후 *subSample* 브랜치는 삭제해주자
- merge가 이루어지는 방식으로 fast-forward와 3-way-merge가 있는데, 한 브랜치(A)가 다른 브랜치(B)보다 뒤쳐져 있는 상태에서 A브랜치에서 B브랜치를 대상으로 merge할 경우 A브랜치의 HEAD를 B브랜치의 HEAD로 옮기는 방식으로 이루어진다. (<code>git merge *B*</code>이면 B브랜치를 따라간다는 느낌) 이를 fast-forward라 한다.
- fast-forward를 하지 않고 일반적인 merge처럼(3-way-merge) 따로 하나의 커밋으로 합치고 싶은 경우
	- <code>git merge --no-ff *(병합할 브랜치명)*</code>

## 6. 원격 저장소(GitHub) 사용
- 로컬에서 원격 저장소 추가 후 푸쉬
	1. <code>git remote add *origin* *(원격 저장소 주소)*</code>
	2. <code>git branch -M *main*</code>
		- 메인 브랜치 이름을 *main*으로 변경(GitHub 권장사항)
	3. <code>git push -u *origin* *main*</code>
		- 현재 속한 브랜치에서 push 할 때 *origin*의 *main* 브랜치에 push할 것임을 세팅 (이후 push/pull시 *origin*과 *main*을 생략하고 git push, git pull로 간단히 작업가능)
- 원격 저장소에서 프로젝트 다운
	- <code>git clone *(원격 저장소 주소)*</code>
- 원격 저장소 목록 보기
	-  <code>git remote</code>
	-  <code>git remote -v</code> (자세히 보기)
- 로컬과 원격 저장소와의 연결 제거
	-  <code>git remote remove *origin*</code>
- 로컬에서 원격으로 push(Local Repository → Remote Repository)
	-  <code>git push *origin* *main*</code>
- 원격에서 로컬으로 pull(Remote Repository → Local Repository)
	-  <code>git pull *origin* *main*</code>
		- <code>git pull</code> 대신 <code>git fetch</code>를 쓰면 내 브랜치에 적용하지는 않은 채로 로컬로만 가져와서 볼 수 있다.
- push하기전 pull해야 할 경우
	-  <code>git pull --no-rebase *origin* *main*</code> (merge방식)
		- 충돌이 발생할 경우 충돌 수정 후 <code>git add .</code>, <code>git commit</code>을 해주면 된다.
	-  <code>git pull --rebase *origin* *main*</code> (rebase방식)
		- 원격에 맞춘 후 내가 한 로컬의 것을 잘라붙인다.
		- 충돌이 발생할 경우 충돌 수정 후 <code>git add .</code>, <code>git rebase --continue</code>로 계속 진행할 수 있다.
- 로컬에서 원격으로 강제로 push
	-  <code>git push --force *origin* *main*</code> (--force대신 -f 가능)
- 로컬에서 *sampleBranch* 브랜치 만든 걸 원격에 push
	- <code>git push -u *origin* *sampleBranch*</code>
		- 원격의 브랜치까지 확인하려면 <code>git fetch</code>후 <code>git branch --all</code>를 하면 된다. (--all 대신 -a 가능)
- 원격에서 *sampleBranch* 브랜치 만든 걸 로컬로 가져오기
	- <code>git switch -t *origin*/*sampleBranch*</code>
- 원격의 *sampleBranch* 브랜치를 삭제
	- <code>git push *origin* --delete *sampleBranch*</code>

## 7. Git의 여러 기능들
- Stash를 이용하여 중간에 하던 작업을 치워둘 수 있다. (이 때 작업중인 파일은 tracking이 되는 상태여야 한다)
	- <code>git stash ~~save~~</code> (현재 작업 치워두기, save 생략 가능)
	- <code>git stash -p</code> (원하는 것만 stash)
	- <code>git stash -m *"(메시지)"*</code> (메시지와 함께 stash)
	- <code>git stash list</code> (전체 stash 목록 보기)
	- <code>git stash apply *stash@{1}*</code> (치워둔 *1번 stash* 적용)
	- <code>git stash drop *stash@{1}*</code> (치워둔 *1번 stash* 삭제)
	- <code>git stash pop *stash@{1}*</code> (치워둔 *1번 stash* 적용 후 삭제)
		- apply, drop, pop에서 *stash@{1}* 등 명시 안하면 마지막이 기준임
		- stash 적용할 경우 파일만 원래대로 돌릴뿐 add를 하는 개념은 아님
		- stash후 다른 브랜치로 이동해서 그 브랜치에 적용시킬 수 있다.
	- <code>git stash branch *(브랜치명)*</code> (새 브랜치를 생성 후 pop)
	- <code>git stash clear</code> (치워둔 모든 stash 삭제)
- 과거의 커밋들 수정/삭제/병합/분할
	- <code>git rebase -i *(수정할 커밋의 바로 전 커밋)*</code>
		- 해당 커밋부터 시작하는 새로운 브랜치를 만들고 rebase하는 방식
		- 해당 커밋 바로 다음~끝까지의 순서로 커밋 내역들이 나오며, pick으로 되어 있는 걸 수정하면 된다.
			- p, pick: 커밋을 그대로 둠
			- r, reword: 커밋 메시지를 변경
			- e, edit: 해당 커밋까지 된 상태로 돌아가 직접 수정
			- d, drop: 커밋 삭제
			- s, squash: 바로 이전 커밋과 합침
- Git에서 추적하지 않는 파일들 삭제
	- <code>git clean</code>
		- -n: 삭제 대신 대상 파일들의 리스트만 보여줌
		- -i: 인터렉티브 모드로 어떤 걸 삭제할지 선택 가능
		- -d: 폴더 포함해 삭제
		- -f: 강제로 삭제
		- -x: .gitignore에 등록된 파일들도 삭제
	- <code>git clean -df</code> (일반적으로 많이 사용)
- Git에서 한 모든 활동 확인
	- <code>git reflog</code>
		- 원하는 작업의 해시를 찾고 <code>git reset --hard</code> 해주면 그 작업까지 한 상태로 돌아가고 돌아간 내역도 남게 된다. (git reset 등 복구 가능)
- tag 기능을 이용하여 특정 시점을 키워드로 저장하거나 커밋에 버전 정보를 붙일 수 있다.
	- <code>git tag *(태그이름)*</code> (마지막 커밋에 lightweight 태그 달기)
	- <code>git tag -a *(태그이름)*</code> (마지막 커밋에 annotated 태그 달기)
		- 이후 메시지 작성 가능
		- lightweight는 태그로 가리키기만 하고, annotated는 작성자 정보/날짜/메시지/GPG 서명 등을 포함할 수 있다.
	- <code>git tag *(태그이름)* -m "*(메시지)*"</code> (메시지와 함께 annotated 태그 달기)
	- <code>git tag</code> (현재 태그 확인)
	- <code>git show *(태그이름)*</code> (원하는 태그 내용 확인)
	- <code>git tag -d *(태그이름)*</code> (태그 삭제)
	- <code>git tag *(태그이름)* *(커밋 해시)* -m "*(메시지)*"</code> (원하는 커밋에 태그 달기)
	- <code>git tag -l "*(태그이름)*"</code> (원하는 조건으로 필터링)
		- 예를 들어 "v1.*" 와 같이 와일드카드를 사용해서 v1.로 시작하는 태그만 확인할 수 있다.
	- <code>git checkout *(태그이름)*</code> (특정 태그의 시점으로 이동)
	- <code>git push *(원격이름)* *(태그이름)*</code> (특정 태그 원격에 올리기)
	- <code>git push --delete *(원격이름)* *(태그이름)*</code> (특정 태그 원격에서 삭제)
	- <code>git push --tags</code> (모든 태그 원격에 올리기)
		- GitHub에서 원하는 태그를 release 버전으로 출시해서 배포판으로 만들 수 있다.
- log 다양하게 활용하기
	- <code>git log -p</code> (각 커밋마다의 변경사항 함께 보기)
	- <code>git log -*(개수)*</code> (최근 n개의 커밋 보기)
		- ex. <code>git log -p -3</code>
	- <code>git log --stat</code> (간략한 통계와 함께 보기)
	- <code>git log --shortstat</code> (더 간략하게 보기)
	- <code>git log --oneline</code> (한 줄로 짧게 보기)
		- <code>git log --pretty=oneline --abbrev-commit</code>와 동일
	- <code>git log -S *(검색어)*</code> (파일의 변경사항을 기준으로 검색)
	- <code>git log --grep *(검색어)*</code> (커밋 메시지를 기준으로 검색)
- diff 다양하게 활용하기
	- <code>git diff</code> (Staging area에 올리기 전 변경사항 확인)
	- <code>git diff --name-only</code> (변경사항 있는 파일 목록만 확인)
	- <code>git diff --staged</code> (Staging area에 올린 후 확인)
	- <code>git diff *(커밋1)* *(커밋2)*</code> (커밋간의 차이 확인)
		- 커밋 해시 또는 HEAD 번호 입력
		- 커밋 하나만 입력하면 현재 커밋과 비교함
	- <code>git diff *(브랜치1)* *(브랜치2)*</code> (브랜치간의 차이 확인)
- blame으로 어떤 코드를 누가 작성했는지 확인가능하다.
	- <code>git blame *(파일명)*</code>
	- <code>git blame -L *(시작줄)* *(끝줄/+줄수)* *(파일명)*</code>
		- ex. <code>git blame -L 10, +3 sample.txt</code> (line10-12 확인)
- bisect로 여러 커밋들 중에서 어느 시점에 오류가 발생했는지를 이진 탐색으로 찾을 수 있다.
	1. <code>git bisect start</code> (이진 탐색 시작)
	2. <code>git bisect bad</code> (오류발생 지점임을 표시)
	3. <code>git checkout *(의심 커밋 해시)*</code> (의심 지점으로 이동)
	4. <code>git bisect good</code> / <code>git bisect bad</code> (오류 발생 유무 표시)
		- 4번을 반복하다 찾았을 때 어느 시점에 발생했는지 출력해준다
	5. <code>git bisect reset</code> (이진 탐색 종료)
- submodule로 프로젝트 폴더 안에 또 다른 프로젝트가 포함될 때, 하지만 메인 프로젝트와는 별개로 다루고 싶을 때 사용할 수 있다.
	- <code>git submodule add *(submodule의 GitHub 레포지토리 주소)* *(하위폴더명, 없을 경우 생략)*</code> (현재 프로젝트에 서브모듈 추가)
		- 현재 프로젝트에 서브모듈파일과 .gitmodules 파일이 생기며, .gitmodules파일에는 서브모듈의 이름과 함께 path, url 정보가 적혀있다.
		- 서브모듈을 포함한 프로젝트는 그 서브모듈 안의 파일 변경사항에는 관여하지 않지만, 그 서브모듈에 어떤 커밋이 발생했는가는 git으로 관리한다.
	- <code>git submodule init *(특정 서브모듈 지정시 해당 이름)*; git submodule update</code> (메인 프로젝트 clone한 상태에서 서브프로젝트까지 가져와서 업데이트 할 때)
	- <code>git submodule update --remote</code> (GitHub, 즉 remote에서 submodule의 수정이 발생한 상황에서 업데이트 할 때)
		- 서브모듈 안에 서브모듈이 또 있을 경우 <code>--recursive</code>를 끝에 추가하면 된다.

## 8. GitHub의 여러 기능들
- Pull request(PR) 기능을 이용하여 변경사항을 merge하기 전 팀원들의 리뷰와 동의를 거치도록 할 수 있다.
- Issue 기능을 이용하여 버그나 문제점 제보, 추가할 기능 등에 대해 이야기를 나눌 수 있다.
- 오픈소스 프로젝트를 fork한 뒤, 코드 수정 후 pull request를 해서 프로젝트에 기여할 수 있다.
- GitHub의 레포지토리 명을 <code>(내 아이디).github.io</code> 로 짓고 최상위 디렉토리에 <code>index.html</code>을 작성해서 GitHub에 블로그를 만들 수 있다.

## 9. 기타 사용법
- help보기
	- <code>git help</code> (기본적인 설명)
	- <code>git help -a</code> (모든 명령어 설명)
	- <code>git *(명령어)* -h</code> (특정 명령어 설명 및 옵션)
		- -h대신 --help 입력시 웹사이트에서 볼 수 있다. (안 열릴 경우 -w추가)
- 현재 설정 확인
	- <code>git config --list</code> (local)
	- <code>git config --global --list</code> (global)
	- <code>git config -e</code> (local을 에디터에서 보기)
	- <code>git config --global -e</code> (global을 에디터에서 보기)
- Git에서 에디터를 VSCode를 이용하도록 설정(+수정중 CLI정지)
	- <code>git config --global core.editor "code --wait"</code>
- Git 단축키 설정
	- <code>git config --global alias.*(내가쓸단축키)* *(원래 명령어)*</code>