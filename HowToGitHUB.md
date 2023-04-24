# git on GitHUB.com HOWTO

Mac の local の folder の内容を git の管理下にして GitHUB の remote repository と紐づけする方法です。つまり、source code や文章を先に作成し、その後、GitHUB の管理下におきたいときに使う方法です。

考え方としては二段階で、まず local の folder を git の管理下におきます。次に remote reposity を指定し、そこに登録するという流れです。イメージとしては、初期化して、管理下に加えて、リモートサイトを登録し、リモートサイトに送る、という流れになります。

## STEP 1: git管理下の設定

Mac の Termanal から対象の folder に移動し、folder をgit管理下にします。ここではまだ github.com は関係ありません。

```
$ cd target_directory
```

target_directory とは、登録したい file があるdirectory, folder のことです。

最初に git の管理下にすでに入っていないか念のため確認します。

```
$ ls -a
```

として、あるいは、$ git status で確認します。

.git の direcotry がなければ大丈夫です。もしすでに存在するのであれば、

```
$ rm -rf .git/
```
で削除します。

そして、初期化します。

```
$ git init
```

これで .git という directory が作成されます。

こんな waring も表示されたかもしれません。
```
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
```

これをやっておくとこの warning は表示されなくなります。

```
$ git config --global init.defaultBranch main
```

あるいは、後で次のようにします。コマンドの意味は現在のbranch 名を”main”に変更するというものです。-mとは違い、-Mで強制的に変更します。

```
$ git branch -M main
```

man git-branchより

    With a -m or -M option, <oldbranch> will be renamed to <newbranch>. If
    <oldbranch> had a corresponding reflog, it is renamed to match <newbranch>, and a
    reflog entry is created to remember the branch renaming. If <newbranch> exists,
    -M must be used to force the rename to happen.


## STEP 2: remote repository の設定

次に git管理化にしたい directory に remote reposity を設定し紐付けます。

今度はまず gitHUB.com で作業します。適当に名前を入れて新しい reposity を作成します。今回は名前を次のようにします。

```
rightmost-removed-4keys_thumb
```

Web siteでの詳細は略します。上記のようにできたとします。
次に作った reposity と local のfileを紐付けます。formatとしては次のように ID と repository の名前が入ることになります。

```Mac
$ git remote add origin git@github.com:UserID/RepositotyName.git
```

実際には、私のIDの例なら
```Mac:Terminal
$ git remote add origin git@github.com:etalli/rightmost-removed-4keys_thumb.git
```
とします。repository 名の後ろに.gitが必要です。

このときに
```
$ git remote add origin git@github.com:etalli/168_LMT.git
error: remote origin already exists.
```
となることがあります。
対処法としては次のように削除します。
```
$ git remote rm origin
```

このエラーメッセージが示しているのは、すでに同じ名前で設定されていますよ、という意味です。
なので、対処法としては、別の名前にして新規に足すか、更新するしかありません。

まずは状況を確認します。
```
$ git remote -v.
```
確認すると、
```
❯ git remote -v
origin	git@github.com:etalli/168_LMT.git (fetch)
origin	git@github.com:etalli/168_LMT.git (push)
```
となりました。登録済みです。

新しいリモートを足すには、
```
$ git remote add github git@github.com:ppreyer/first_app.git
```

更新するには
```
$ git remote set-url origin git@github.com:ppreyer/first_app.git
```
となります。

## STEP 3: remote repositoryへの追加

directory内のすべてのfileをgit管理するために git add で file や directory を追加します。git管理の外にしたいfileやdirectoryがある場合は、個別に git add するか、.gitignore file で除隊したい file や directory を記載しておきます。

```
$ git add *
```

これは少々時間がかかることがあります。
次に、git commit で local の git repository を commit します。

```
$ git commit -m 'first commit.'
```

## STEP 4: remote repositoryへのpush

```
$ git push -u origin main
```

このようにlocalのfolderをGitHUB管理下において、remote repository へ file を登録できます。


## others

### 設定された値の確認

```
% git config -l

```

### グローバル設定

全体の設定をしたいときは、

```
$ git config --global user.name "Yamada Tarou"
$ git config --global user.email "yamada@example.com"
```

### リポジトリ毎の設定

repository(directory)ごとにauthorを登録したいときは

```
$ git config user.name "Yamada Tarou"
$ git config user.email "yamada@example.com"
```

以上、local のfile, directory を GitHUBの管理下に置く方法でした。

## ファイル削除

```
$ git rm filename
```
