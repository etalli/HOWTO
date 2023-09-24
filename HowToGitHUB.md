# git on GitHUB.com HOWTO

Mac の local の folder の内容を git の管理下にして GitHUB の remote repository と紐づけする方法です。つまり、source code や文章を先に作成し、その後、GitHUB の管理下におきたいときに使う方法です。

考え方としては二段階で、まず local の folder を git の管理下におきます。次に remote reposity を指定し、そこに登録するという流れです。具体的には git の初期化をして管理下に加えて、GitHUBのリモートサイトを登録し、そのリモートサイトに保存する、という流れです。

## STEP 1: git管理下の設定

Mac の Termanal から対象の folder に移動し、folder を git管理下にします。ここではまだ github.com は関係なく、local site の作業になります。

```
$ cd target_directory
```

target_directory とは、登録したい file がある directory, folder のことです。

最初に git のすでに管理下に入っていないか念のため確認します。

```
$ ls -a
```

として、あるいは、$ git status で確認します。

.git の direcotry がなければ大丈夫です。もしすでに .git が存在するのであれば、

```
$ rm -rf .git/
```

で削除します。

そして、初期化します。

```
$ git init
```

これで .git という directory が作成されます。

正常に実行さると、例えば下記のように表示されます。
```
$ git init
Initialized empty Git repository in /Users/k/Library/CloudStorage/Dropbox/MyProjects/180_BluetoothKeyboard/.git/
```

場合によっては、こんな waring も表示されたかもしれません。

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

この場合には、次のコマンドを実行しておくと、この warning は表示されなくなります。

```
$ git config --global init.defaultBranch main
```

あるいは、後で次のようにします。コマンドの意味は現在の branch 名を ”main” に変更するというものです。-m とは違い、大文字の -M で強制的に変更します。

```
$ git branch -M main
```

参考までに -m と -Mの違いは、man git-branchより

    With a -m or -M option, <oldbranch> will be renamed to <newbranch>. If
    <oldbranch> had a corresponding reflog, it is renamed to match <newbranch>, and a
    reflog entry is created to remember the branch renaming. If <newbranch> exists,
    -M must be used to force the rename to happen.


## STEP 2: remote repository の設定

STEP 1で gitの初期化ができたら、次に gitの管理化にしたい directory に remote reposity を設定し紐付けます。

今度は gitHUB.com で作業します。適当に名前を入れて新しい reposity を作成します。今回は名前を次のようにします。
Web site での詳細は略しますが、流れとして、github top / Repogitries / New / として、repository name, Licenseの指定、READMEの追加の有無などを入力して作ります。

```
180_bluetooth_keyboard
```

上記のようにできたとします。

次に作った reposity と local repository を紐付けます。format としては次のように ID と repository の名前が入ることになります。

```Mac
$ git remote add origin git@github.com:UserID/RepositotyName.git
```

実際には、私のIDの例なら

```Mac:Terminal
$ git remote add origin git@github.com:etalli/180_bluetooth_keyboard.git
```
とします。repository 名の後ろに.gitが必要です。
これは成功しても何も表示されません。

このときに

```
$ git remote add origin git@github.com:etalli/168_LMT.git
error: remote origin already exists.
```

となることがあります。
この対処法としては次のように削除します。

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

STEP1 で local repository の初期化、STEP2 で remote site を作り、local と remote の紐づけができたので、
次に local directory の file を git管理するために git add で file や directory を追加します。
git の管理外にしたい file や directory がある場合は、個別に管理したいものを git add するか、逆に .gitignore file で除外したい file や directory を記載する方法があります。

まず加えたいものを足す方法です。

```
$ git add *
```

これはlocal のfileが多い場合には少々時間がかかることがあります。ただ、これだと全部登録されてしまいますので、backup などが不要なら .gitignore を用いて、除外したいfile, directory の指定をするとよいかと思います。

次に、git commit で local の git repository を commit します。

```
$ git commit -m 'first commit.'
```

すとる次のようになります。
```
❯ git commit -m "first commit"                                             [09/24/23 /Users/k/Dropbox/MyProjects/[main (root-commit) e12faf2] first commit
 3 files changed, 52 insertions(+)
 create mode 100644 .gitignore
 create mode 100755 RPI_pico_W_keyboard/RPI_pico_W_password.ino
 create mode 100644 RPI_pico_W_keyboard/readme.md
```

この段階ではまだgithub.comには登録されていません。あくまで管理下においたという処理までです。

ここではreferences以下はあくまで参考で管理下に置きたくないので、.gitignorに登録して削除しています。

```
❯ git add .gitignore
```
.gitignore file自体も忘れずに登録します。


## STEP 4: remote repositoryへのpush

```
$ git push -u origin mainkk
Enter passphrase for key '/Users/k/.ssh/id_rsa':
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 1.00 KiB | 1.00 MiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:etalli/180_bluetooth_keyboard.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

このように local の folder を GitHUB 管理下において、remote repository へ file を登録できます。
本当に登録されたかgithub.comをweb browserで確認します。
```
https://github.com/etalli/180_bluetooth_keyboard
```

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
