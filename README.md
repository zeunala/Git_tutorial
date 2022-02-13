# Git 사용법 정리
얄팍한 코딩사전님의 제대로 파는 Git & GitHub 강의[(#)](https://www.inflearn.com/course/%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%8C%8C%EB%8A%94-%EA%B9%83?inst=a17e4bef)를 기초로 작성하였습니다.

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
- 선택한 파일 커밋 (Staging area → Local Repository)
	-  <code> git commit </code>
		- 이후 커밋 메시지 입력 가능
	-  <code> git commit -m "*(메시지)*" </code>
		- 커밋 메시지를 함께 입력함
- 선택과 커밋을 한 번에 (Working directory → Local Repository)
	-  <code> git commit -am "*(메시지)*" </code>
		- 단, 새로 추가된 파일(untracked)이 있으면 안 된다.
- 커밋 결과 확인
	-  <code> git log </code>

## 3. Reset/Revert
- Reset: 특정 시점으로 돌아가서 이후 커밋 삭제
	- ex. 1→2→3→4에서 2번대상 reset: 1→2로 만듦
- Revert: 특정 시점의 커밋을 거꾸로 실행
	- ex. 1→2→3→4에서 2번대상 revert: 1→2→3→4→-2
- reset 사용
	-  <code> git reset --hard *(마지막으로 남길 커밋 해시)* </code>
		- 커밋 해시는 <code>git log</code>로 확인 가능
- 마지막 커밋한 시점으로 현재 파일/폴더 상태 초기화
	-  <code>git reset --hard</code>
		- 마지막 커밋 이후 새로 추가된 파일(untracked)은 그대로
- revert 사용
	- <code> git reset --hard *(거꾸로 돌릴 커밋 해시)* </code>
		- 이후 커밋 메시지 입력 가능
		- 수정내역 충돌시 처리 후 <code>git revert --continue</code>
- 커밋없이 revert 사용 (Staging area까지)
	- <code> git revert --no-commit *(거꾸로 돌릴 커밋 해시)*</code>

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
- *sampleBranch*이라는 이름의 브랜치 삭제하기
 	-  <code>git branch -d *sampleBranch*</code>
	-  <code>git branch -D *sampleBranch*</code>
		- 다른 브랜치로 가져오지 않은 내용이 있는 브랜치를 강제로 삭제할 때 사용
- 브랜치 이름을 *oldSample*에서 *newSample*로 바꾸기
	-  <code>git branch -m *oldSample* *newSample*</code>
- 브랜치 내역을 한 눈에 보기
	-  <code>git log --all --decorate --oneline --graph</code>

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
		- 충돌 해결이 어려울 경우 <code>git rebase --abort</code>로 merge를 중단할 수 있다.
	2. <code> git merge *subSample*</code> (*mainSample* 브랜치에서)
		- 1번만 할 경우 main 브랜치는 뒤쳐져 있기에 *mainSample* 브랜치로 이동한 후 위와 같이 fast-forward를 해주어야 한다.
		- 이후 *subSample* 브랜치는 삭제해주자

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