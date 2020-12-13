# python_line_bot
PYTHON で Line Botオウム返しのデプロイ。
HerokuとGitを紐づけてLine Bot を作る


【Line Developersに登録する】
	・プロバイダ作成 - チャネル設定（MessagingAPI）- 各種登録
	・チャネル基本設定
		チャネルシークレット発行
	・Messaging API設定
		・Webhook URL　→　Herokuから
		・チャネルアクセストークン（長期）発行
		・基本設定 - [応答モード]BOT - [挨拶メッセージ]OFF - [応答メッセージ]OFF - [Webhook]ON
		・Webhook エラーが返ってハマったので手順注意。
			→環境変数設定時、Herokuは意図したアプリ、リポジトリをいじれているか？


【PCにGitをDL、Githubの設定を行う】
	・いきなりGitのコマンド出てきて積んだ。
	・GithubとGitは別物だと思うけど、GitをインストールしたらシェルでGitうごかせたから必須。
	・パスに注意。カレントディレクトリをファイルの置いてあるディレクトリに指定してね。


【Heroku】
	・Herokuのアカウントを取得。
	・Heroku にログインする。
	・Heroku CLIインストール
		・$ heroku login
	・Herokuにリポジトリを作成
		・$ heroku create watson3344　
			watson3344はHerokuのリポジトリ名
		※Herokuのリポジトリ変更をするためのコマンドは下記。旧リポジトリの環境変数を変更して壊した。
			heroku git:remote -a　【watson3344】
	・環境変数を変更する。（リポジトリ名確認）
		heroku config:set YOUR_CHANNEL_SECRET="Channel Secretの欄の文字列" --app 【Herokuのアプリ名】
		heroku config:set YOUR_CHANNEL_ACCESS_TOKEN="アクセストークンの欄の文字列" --app 【Herokuのアプリ名】
		→これ、まじでハマった。
			元のコードの'YOUR_CHANNEL_ACCESS_TOKEN', 'YOUR_CHANNEL_SECRET'の部分にそのままアクセストークン・チャンネルシークレットの文字列を入れても動作します。
			しかし、プログラムに直接認証情報を書くのはセキュリティ上良くないので、環境変数に認証情報を設定し、os.environによって環境変数を取得するコードを17～18行目に追記します。
	・Herokuのマイページから、 Deployタブに飛んで、Githubのリポジトリとコネクト。


【設定ファイルの作成】
	・ [runtime.txt] というファイルを作る。記載内容は以下。
		python-3.8.6　　#バージョンを記載する。
	・ [Procfile] というファイルを作る。
		web: python main.py　　#ファイル名の大文字小文字注意。拡張子いらない。
	・ [requirements.txt] というファイルを作る。
		pip freeze > requirements.txt　　#もしくは手動でバージョン記載。（現在は手動）
		Flask==1.1.2
		line-bot-sdk==1.17.0　　#20201211現在。シェル上で pip freeze と入力して情報確認


【参考URL】
	https://www.casleyconsulting.co.jp/blog/engineer/3028/
		→一連の流れがよくわかる。　ソースコードダメ。
		コードブロックが化けていて注意だけど大変参考になる。（&amp;amp;amp;amp;とか不要。）
	https://qiita.com/Dexctersu/items/2243db615a1be7186acc
		→　やり方わかったけど、ソースコード動かない
	https://teratail.com/questions/197758
		→　Gitコマンドでエラーが発生したため調べたら、GitHubのリポジトリ名とHerokuのアプリ（リポジトリ？）名の違いでエラーになっていた。
 		heroku git:remote -a 【watson2233】
 	https://myafu-python.com/line-basic-1/
 		→動作しているのはこのソースコード。一番ためになる。


 【手順】
 	必要なライブラリインストールしてからHerokuにサインイン　
	 	pip install flask
		pip install line-bot-sdk  →　これもはまった。　python関係のツールを再インストールして解決。
		heroku create 任意のアプリケーション名　でherokuにアプリを作る　heroku create 【watson1122とかHerokuアプリ名】

	【環境変数入力】

		heroku config:set YOUR_CHANNEL_SECRET="Channel Secretの欄の文字列" --app 【Herokuのアプリ名】
		heroku config:set YOUR_CHANNEL_ACCESS_TOKEN="アクセストークンの欄の文字列" --app 【Herokuのアプリ名】

	【ファイルの作成】
		mian.pyと同じ階層にruntime.txtとrequirements.txtとProcfileを作成します。
		runtime.txt
			python-3.6.3（使用しているPyのバージョンを記載）
		Procfile（拡張子ナシ）
			python-3.6.3

	【GitのインストールとHerokuの紐づけ】
		GitHubに作ったリポジトリをHerokuに紐づけるんだけど、HerokuにもGitみたいな機能があってわけわからなかった。
		ブランチ名やリポジトリ名などが違ってエラー吐くことおおいから要注意。

	【Webhook】
		Webhook URL ：　https://【Herokuアプリ名】.herokuapp.com/callback
		Helokからデプロイ。



		$ git push -u origin --all

		$ git push heroku main　　　（main だったり、 masterだったりするので気を付ける）

		$ heroku logs　　　　ログしっかり読む