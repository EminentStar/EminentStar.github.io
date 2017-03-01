---
layout: post
title: Git 저장소 기존 커밋의 Author 변경
---




<div class="message">
  !주의: 이미 작성된 기록을 재작성하는 것은 좋지않은 행동임을 밝히고 글을 작성하겠습니다.
</div>


git 저장소에서 이미 했던 commit들에 대한 Author를 변경하는 것을 알아보려 합니다.   
이걸 정리하는 이유는,,, 최근들어 정신을 차리고 GitHub 초록색 박스를 꾸준히 채우는 것을 목표로 하고 있는데, 매일 커밋을 했음에도 불구하고 빈칸이 생기는 것이었습니다. (2017년2월 25일 커밋 박스가 공백.)

![20170301_1.png](/imgs/2017-03-01/20170301_1.png)

왜 이런 현상이 생기는지에 대해 찾아보던 중 다음과 같은 문구를 봤습니다.

![20170301_2.png](/imgs/2017-03-01/20170301_2.png)

terminal을 쓰던 툴을 쓰던 로컬에서 git config에 email을 설정하지 않으면 초록색 체크박스가 채워지지 않는다는 것이었습니다.

그래서 해당 프로젝트에서

```
$ git log
```
를 통해 2017/02/25 에 해당하는 커밋을 살펴보니
 
![20170301_3.png](/imgs/2017-03-01/20170301_3.png)

이와 같이 Author가 로컬로 되어 있었습니다. (노트북이 두개있다보니 하나에는 email config가 설정안되있다는걸 몰랐네요.;)  
그럼, 우선 local git config에 email을 추가하는 걸로 먼저 시작을 해야겠네요.
 
```
$ git config --global user.email "your_email@example.com"
```

your_email@example.com에는 자신의 github 계정에 등록된 email주소를 적으시면 됩니다. 위와 같이 커맨드를 입력하고 
 
```
$ git config --global user.email
```

커맨드를 입력해서 제대로 email주소가 들어갔는지 확인해봅니다.
 
사실 이미 해버린 커밋에 대해서 Author를 바꾸는 것은 config의 email과 상관이 없지만 추후에 할 커밋을 대비하여 config를 설정하는 것을 언급하였습니다.
 
확인이 다 되었으면 이제 바뀐 Author로 커밋을 변경해야하는데요.   
<br> 
이를 위해선 

* rebase : 병합용
* commit - -amend : 커밋 변경용


이 두가지 커맨드가 필요합니다.  


사실 제대로 커밋 구조에 대해 설명을 하려면 rebase에 대해서 잘 알고 있어야하지만, 지금의 저는 rebase에 대해 잘 모르기에 추후에 공부를 한후에 내용을 더 추가하도록 하겠습니다.

 
지금 제 repository에서 2017/02/25 에 해당하는 커밋은 총 세개 입니다.


#### 커밋 구조

![20170301_10.png](/imgs/2017-03-01/20170301_10.png)

 
구조를 따지자면 `X1`, `X2`, `X3`로 표시된 것이 2017/02/25일자 커밋이고, 제일 우측의 `O4`가 HEAD이고 가장 최근의 커밋입니다. (25일에 제가 3개의 커밋을 했네요.)
 
- `X1`(커밋 메시지: Add the url of naver API reference)
![20170301_4.png](/imgs/2017-03-01/20170301_4.png)
 
- `O2`(커밋 메시지:ADC-174 Implement PUT api)
![20170301_5.png](/imgs/2017-03-01/20170301_5.png)


먼저 커밋의 author를 바꾸고 싶다면, 바꾸기 전의 노드을 base로 잡아 rebase를 해야합니다.   
 
여기선 `O2`가 되겠죠.   
그럼 git log를 통해 볼 수 있는 `O2` 커밋의 commit hash값(예를 들면aece6e8ec8ee499b80be7c17e0c7498d3e1dfceb)을 복사한 후 
다음의 커맨드를 입력해줍니다.
 
```
$ git rebase -i <commit hash>
```
그 후 저는 vim을 쓰기에 vim 창이 뜨는데요,
 
여기서 `O2`를 base지점으로 해서 그위 4개 커밋에 대한 정보가 나와있습니다.
 
```
pick ??????? Add the url of naver API reference
pick ??????? ADC-174 Implement the logic getting keyword list
pick ??????? ADC-174 Implement the logic deleting a keyword
pick ??????? Add the sample getting Stat data
```
 
여기서 수정할 커밋에 대해서는 pick을 edit으로 바꿔줍니다.
위에서부터 `X1`, `X2`, `X3`가 되겠네요.
 
```
edit ??????? Add the url of naver API reference
edit ??????? ADC-174 Implement the logic getting keyword list
edit ??????? ADC-174 Implement the logic deleting a keyword
pick ??????? Add the sample getting Stat data
```
 
그리고 :wq를 통해 저장을 하면 이제 rebase 과정이 진행됩니다.
(`X1`, `X2`, `X3` 차례로 커밋 수정 후 rebase 계속 진행을 하게 됩니다.)
 
먼저 edit하기로 한 `X1`의 commit을 수정해야하는데요, 
 
```
$ git commit --amend --author="AuthorName <email@address.com>"
```
저의 경우에는 
git commit --amend --author="Junkyu Park <junk3843@naver.com>"
아래와 같습니다.
 
커맨드를 입력하게 되면 커밋을 위한 텍스트 에디터 창이 뜨고, :wq를 통해 커밋을 변경하도록 합니다.
 
커밋이 변경되면 아래의 커맨드로 rebase를 계속 진행합니다.
 
```
$ git rebase --continue
```
 
그럼 이제 `X2`를 수정할 차례네요.
`X1`과 마찬가지로 git commit amend를 통해 커밋을 변경하고 rebase 를 통해 rebase를 진행시킵니다.
 
`X3`도 `X1`, `X2`와 같은 작업을 해줍니다.
 
3번의 edit을 마치게 되면 rebase가 완료됩니다.
![20170301_7.png](/imgs/2017-03-01/20170301_7.png)
 
그리고 git log를 통해 커밋 정보를 확인해보면, 
 
![20170301_8.png](/imgs/2017-03-01/20170301_8.png)
 
와 같이 Author가 변경된 것을 확인할 수 있습니다.
 
이제 변경사항을 github의  remote repository에 적용시켜야합니다.
<br><br>
단순히 git push를 하면 되지않기때문에 강제적으로 
```
$ git push -f
```
를 통해 push해줍니다.(**다시 말하지만, 이미 작성된 기록을 재작성하는 것은 좋지않은 행동입니다**)  
<br>
push를 완료하고 나서 github 계정의 profile을 가보면 2017/02/25에 해당하는 커밋 3개가 채워진 것을 볼 수 있습니다.

![20170301_9.png](/imgs/2017-03-01/20170301_9.png)
