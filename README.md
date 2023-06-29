# GithubでSSH通信を出来るようにする

## 🚀 SSH Keyを作成 (作成したことがない場合)
GitBashだと混乱がないのでGitBashを起動してください
```bash
ssh-keygen -t ed25519 -C "<GitHub二登録したメールアドレス>"
-t は鍵の暗号化アルゴリズム (ed25519が使用できないPCだった場合、`-t rsa`に変更)
-C はコメント 複数のSSH keyを併用しているとどれがどれかわからなくなるので

# 色々質問が出てくるので、Enterを押していく
# Q SSH Keysの保存先を聞かれているので、特に気にしなければそのままEnter
# Q パスワードつけるなら入力してEnter (入れなくてもOKですが、セキュアではない)
# Q もう一度パスワード入力してEnter

> Generating public/private ALGORITHM key pair. 作成成功！
```

何も指定しなければWindowsの場合
`C:¥Users¥ユーザー名¥.ssh`　の中に

`id_rsa`(秘密鍵)、`id_rsa.pub`(公開鍵)
が作成されます。

---  

## 🚀 GitHubに公開鍵を登録する
1. カウントの情報欄から [settings] から[SSH and GPG keys]を選びます。  
2. [Title]に自分がわかりやすい名前(Eihan-PCとか)を入力し、[key]の入力欄に`id_rsa.pub`の内容をコピペしてください。  
3. [Add SSH Key]ボタンで登録

---  

## 🚀 通信できるかチェック
再びGitBashを開く
```bash
# SSH接続ができているかを確認
ssh -T git@github.com

# 初めてGithubとSSH接続する際は
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?'
# と表示されるかもしれませんが、`yes`でOK

# 下記が表示されたらSSH通信ができています
Hi ユーザー名! You've successfully authenticated, but GitHub does not provide shell access.

```
---

## 🚀 実際にGithubを使ってみる
#### 👀 まずはテスト用にリポジトリを作成
[【Git初心者向け】リポジトリの作成からpushまでを解説](https://www.sejuku.net/blog/70775)
「まずはリポジトリを作成しよう」を参考に適当なリポジトリを作成してください。

GitBashを再び開き、
```bash
# 適当なディレクトリに移動
# ドラック&ドロップでディレクトリパスGitBsh内に放り投げるとパスが入力できます
cd <ワークスペースのパス> 

# 移動先ディレクトリから下層をgitの管理下とする
# つまり初期化->.gitが作られる->こいつが差分とかを管理してる
git init

# 管理下としたディレクトリは、どのリポジトリを管理していくのか登録する
git remote add origin git@github.com:<githubのユーザー名>/<最初に作成したリポジトリの名前>.git

# 適当に空のindex.htmlを作成(
# 手動で適当なファイルを入れても良いです
touch index.html

# ステージする(インデックスに登録) 
# git add -A でもOK : 編集したやつ全部登録するコマンド
git add index.html

# コミットする
# -m : メッセージを付けてあげる
git commit -m 'SSHテスト'

# リポジトリにpushする
# リポジトリのmainブランチに変更内容を送りつけるイメージ
git push origin main
```

---  

## 🚀 完了
問題なければSSHの通信ができていますし、Githubも使えることが分かりました！終了!  
今回使用したディレクトリは削除してもOKです
