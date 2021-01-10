# README

「devise 機能」

(1)devise 導入

①gemfileに追加。
　gem 'devise'

②bundle install

③devise 初期設定コマンド実行
　rails g devise:install
　ルーティング・デフォルトのview等が自動作成される。


(2)devise モデル/マイグレーションファイル作成

①ユーザーモデルの作成
　rails g devise User　(rails g devise モデル名)
　モデル/マイグレーションファイルが作成される。
　(rails g devise モデル名)

②追加したいカラムがあれば、Userのマイグレーションファイルに追記する。
　(例)ユーザー名のカラムを追加　→ t.string :name

③rails db:migrate


(3)devise のルーティング

　devise_for :users
　deviseを使用する際に、URLとしてusersを含むことを示す記述。
　(1)③の時点で自動作成されている。


(4)devise モデルの記述
　(2)①の時点で自動作成される。

　devise :database_authenticatable, :registerable,
　       :recoverable, :rememberable, :validatable

作成したUserモデルにdeviseで使用する機能が記述されている。
①database_authenticatable　パスワードの正確性を検証
②registerable　ユーザー登録や編集、削除
③recoverable　パスワードをリセット
④rememberable　ログイン情報を保存
⑤validatable　emailのフォーマットなどのバリデーション


(5)devise　view作成

①カスタマイズできるViewファイルの作成
　rails g devise:views

②form_forの記述
　<%= form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
・resource　は　devise独自の記述。
・urlは、form_forのオプション。<%= f.submit %>をクリックした時のアクションを設定できる。
・registration_path(resource_name)で、registration_pathへUserモデルを渡す。
・registration_path　は、deviseの新規登録を行うコントローラ

③エラーメッセージを呼び出す。
　<%= render "views/devise/shared/error_messages", resource: :resource %>
・デフォルトのエラーメッセージは、
　"views/devise/shared/error_messages"に入っている。
　これをrenderで呼び出す。


(6)form_forのオプション
①autofocus: true
　ページを読み込んだら①を記述している部分にカーソルが移動して入力状態になる。

②placeholder: "表示させたい文字(変数も使える)"
　入力フォームに予めテキストが表示される。
　入力者が何をどのように入力すれば良いか分かりやすくなる。

③autocomplete: "off"
　自動補完機能の無効化。
　パスワードのフォームなどに記述。


(7)devise　コントローラの追記・修正
　deviseのコントローラはライブラリで用意されているので直接修正できない。
　修正したい時は、application_controller.rbに記述する。

・example.　初期設定にないnameカラムを追加した場合。
①before_action :configure_permitted_parameters, if: :devise_controller?

②protected

③def configure_permitted_parameters
　　　devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
　end


①deviseの機能が使われる時、その前に configure_permitted_parameters が実行される。
②privateは、自分のコントローラ内でしか参照できない。
　protectedは、呼び出された他のコントローラからも参照できる。
③devise_parameter_sanitizer.permit　は、()内のデータ操作を許可するアクションメソッドを指定している。
　()内はいつ、何のデータ操作を許可するのかを定めている。









