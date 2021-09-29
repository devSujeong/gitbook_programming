# commit

## Commit이란?

commit은 기록에 이름을 붙이는 행위입니다. 이러한 코드 변경 사항이 무엇을 의미하는지 이름을 짓고 기록함에 보관하는 행위입니다. 기록을 보기 편하게 깔끔하게 정리하기 위해서 커밋을 합치거나, 취소하거나, 삭제할 수 있습니다.

## commit 합치기

### 최신 commit에 합치기

```bash
git commit --amend -m "commit message"
```

### 여러 개의 commit 합치기 \(rebase\)

{% hint style="warning" %}
origin에 있는 커밋을 수정하는 것은 다른 개발자의 git에 영향을 줄 수 있으므로, 어쩔 수 없는 경우가 아니면 하지 않습니다. 강제로 수정할 경우, `git push -f origin <branchName>`으로 진행해야 합니다.
{% endhint %}

```bash
git log --branches --not --remotes --oneline # push 안한 commit 리스트 확인

# rebase할 commit 리스트 만드는 작업
git rebase -i @~2 # 1. HEAD부터 몇개 보일지 정함
git rebase -i HEAD~2 # 2. HEAD부터 몇개 보일지 정함
git rebase -i 9d9cde8 # 3. 수정할 커밋의 직전 커밋으로 불러

i # vi 편집하겠다.

`esc`, :wq # 편집 완료 후 종료
```

## Commit cancel

```bash
# [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
git reset --soft HEAD^

# [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
git reset --mixed HEAD^
git reset HEAD^
git reset HEAD~2

# [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
git reset --hard HEAD^
```

