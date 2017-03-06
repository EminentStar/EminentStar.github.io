---
layout: post
title: Rebase -i를 이용한 이전 커밋 합치기
---




git 저장소에서 이미 했던 commit들을 하나로 합치는 걸 해보려 합니다.  

이걸 하려는 이유는,, 최근에 제 Github page에 subdomain관련해서 CNAME을 설정하고 지우는 걸 반복했는데, 

Github page의 커밋목록을 보니 불필요하게 다음과 같이 커밋 기록들이 남아 있는 것이었습니다.

![20170306_1.png](/imgs/2017-03-06/20170306_1.png)

지금은 현재 remote와 local 리포지토리가 모두 Ceate CNAME, Delete CNAME 이란 커밋 기록들을 가지고 있습니다.

이 4개의 커밋(Delete CNAME, Create CNAME, Delete CNAME, Create CNAME)들을 없앤다기보다는,, 이들 이전의 커밋(Add og tags)에 포함시켜버리면 커밋기록은 사라지면서 수정사항들이 Add og tags 커밋에 반영이 되겠지요.


```
$ git rebase -i HEAD~5
혹은
$ git rebase <commit hash value>
```

커밋을 합치기 위해서는 우선 합치고자하는 커밋의 이전 커밋을 베이스로 interactive rebase 커맨드를 실행해줍니다.  

저같은 경우는 합치고 싶은 Add og tags가 HEAD로부터 4 커밋 떨어져있으니 그 바로 이전인 5커밋 이전을 base로 interactive rebase를 실행하였습니다.  

혹은 `git log`를 통해 보이는 특정 커밋의 해시값을 직접 넣어도 상관없습니다.


![20170306_2.png](/imgs/2017-03-06/20170306_2.png)

그럼 위와 같이 rebase를 위한 에디터가 뜨게 되는데, 위에서부터 아래로 베이스 커밋으로 부터 가까운 순이 됩니다.


![20170306_3.png](/imgs/2017-03-06/20170306_3.png)

그럼 커밋을 합치기 위해서 위와 같이 없애고 싶은 4개의 커밋 라인들의 커맨드를 s(squash)로 바꿔주고, 이 4개의 커밋의 내용들을 포함시키고자 하는 커밋은 pick으로 그대로 둡니다.

```
s, squash = use commit, but meld into previous commit  
```

squash는 영어로 적혀있는 그대로 이 커밋을 사용은 하되, 이전의 커밋에 녹여내겠다. 이런 뜻이네요.  

위와 같이 수정을 한다음 vim 에디터를 저장하고 종료하면 커밋을 위한 vim 에디터 창이 또 뜨게 되고, 그걸 저장종료하면 rebase가 완료됩니다.  

![20170306_4.png](/imgs/2017-03-06/20170306_4.png)

왼쪽의 git 그래프를 잘보시면 `Merge branch ~~@$@#$` 커밋을 기준으로 remote repository와는 별개로 지우고자 했던 4개의 커밋이 사라진 `(HEAD -> master)Add og tags`가 별도의 그래프 선을 가지는 걸 볼 수 있습니다.

<br>

저는 불필요한 커밋들을 지우기 위해서 위의 방법을 사용했지만, 여러 작은 단위의 커밋을 하나의 큰 범주의 커밋으로 합치는 용도로 쓴다고들 합니다.

