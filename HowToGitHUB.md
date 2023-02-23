# git on GitHUB.com HOWTO

既存のMacのlocalのfolderの内容をgit管理下にして remote repositoryと紐づけする方法です。
つまり、source code や文章を先に作成し、その後、GitHUBの管理下におきたいときに使う方法です。

考え方としては二段階で、まずlocalのfolderをgitの管理下において、その上でremote のreposityを指定し、そこに登録するという流れになります。

## STEP 1: git管理下の設定

MacのTermanalから対象のfolderに移動し、folderをgit管理下にします。ここではまだ github.com は関係ありません。

```
$ cd target_directory
```

target_directoryには自分のfileがある実際のdirectoryのことです。

最初にgitの管理下にすでに入っていないか念のため確認します。

```
$ ls -a
```

として、

.git のdirecotryがなければ大丈夫です。もしすでに存在するのであれば、

```
$ rm -rf .git/
```
で削除します。

そして、

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

これをやっておくとこのwarningは表示されなくなります。

```
$ git config --global init.defaultBranch main
```

あるいは、後で次のようにします。意味は現在のbranch 名を”main”に変更します。-mとは違い、-Mで強制的に変更します。

```
$ git branch -M main
```

man git-branchより

    With a -m or -M option, <oldbranch> will be renamed to <newbranch>. If
    <oldbranch> had a corresponding reflog, it is renamed to match <newbranch>, and a
    reflog entry is created to remember the branch renaming. If <newbranch> exists,
    -M must be used to force the rename to happen.


## STEP 2: remote repositoryの設定

次に git管理化にしたい directory に remote reposity を設定し紐付けます。

今度はまずGitHUB.comで作業します。
適当に名前を入れて新しいreposityを作成します。

今回は名前を次のようにします。

```
rightmost-removed-4keys_thumb
```
できました。

次に作ったreposityと紐付けます。formatとしては次のように ID と repository の名前が入ることになります。

```
$ git remote add origin git@github.com:UserID/RepositotyName.git
```

実際には
```
$ git remote add origin git@github.com:etalli/rightmost-removed-4keys_thumb.git
```
とします。repository 名の後ろに.gitが必要です。

## STEP 3: remote repositoryへの追加

directory内のすべてのfileをgit管理するために git add で file や directoryを追加します。git管理の外にしたいfileやdirectoryがある場合は、個別に git add するか、.gitignore file で除隊したいfileやdirectoryを記載しておきます。

```
$ git add *
```

次に、git commit で local のgit repositoryを commit します。

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

