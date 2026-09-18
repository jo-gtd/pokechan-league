# ポケチャン最強決定戦 — セットアップ手順

このフォルダには、Claudeアカウント不要で誰でもURLを踏むだけで使える対戦表アプリが入っています。
`index.html` 1枚だけの静的サイトで、データの保存には無料のFirebase（Firestore）を使います。

以下の手順を上から順に行ってください。すべて無料・クレジットカード登録不要です。

## 1. Firebaseプロジェクトを作る

1. https://console.firebase.google.com/ を開き、Googleアカウントでログイン
2. 「プロジェクトを作成」→ 好きな名前（例：pokechan-league）を入力 → 作成
3. 左メニューの「構築」→「Firestore Database」→「データベースを作成」
   - ロケーションは `asia-northeast1`（東京）などお好きな場所でOK
   - セキュリティルールは「テストモード」でいったん開始してOK（後で上書きします）
4. 作成できたら、左メニューの「Firestore Database」→上部タブ「ルール」を開き、
   このフォルダの `firestore.rules` の中身を丸ごと貼り付けて「公開」

## 2. Webアプリの設定情報を取得する

1. Firebaseコンソールの左上、歯車アイコン →「プロジェクトの設定」
2. 下の方の「マイアプリ」→ `</>`（ウェブ）アイコンをクリックしてアプリを追加
3. アプリのニックネームを適当に入力（例：pokechan-web）→ アプリを登録
4. 表示された `firebaseConfig` の中身（`apiKey` や `projectId` など）をコピー

## 3. `index.html` に設定情報を貼り付ける

`index.html` を開き、`firebaseConfig` と書かれている部分（下記）を、
手順2でコピーした本物の値に書き換えて保存してください。

```js
var firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## 4. GitHub Pagesに公開する

1. https://github.com/ でアカウントを作成（お持ちでなければ）してログイン
2. 右上の「+」→「New repository」→ リポジトリ名を入力（例：pokechan-league）
   - 公開範囲は「Public」を選んでください（Privateだと無料のGitHub Pagesが使えません）
   - 「Create repository」を押す
3. 作成した画面の「uploading an existing file」（または「Add file」→「Upload files」）から、
   このフォルダの `index.html`・`firestore.rules`・`README.md` の3つをまとめてドラッグ＆ドロップ
   → 「Commit changes」で保存
4. リポジトリ画面上部の「Settings」タブ →左メニュー「Pages」
5. 「Build and deployment」の「Source」を「Deploy from a branch」、
   「Branch」を「main」／「/ (root)」に設定 →「Save」
6. 1〜2分待つと、同じ画面に
   `https://ユーザー名.github.io/リポジトリ名/` のURLが表示されます

このURLをClaudeを使っていない友人・家族にもそのまま共有できます。
サインインは一切不要で、開けばすぐに使えます。

以後、`index.html` を書き換えたくなったときは、リポジトリの該当ファイルを
開いて鉛筆アイコン（Edit）で直接編集し、「Commit changes」を押すだけで
自動的に公開ページにも反映されます。

## 動作の仕組み（参考）

- 誰かが結果（勝/分/負）を選ぶと、自動的にFirestoreというデータベースに保存されます
- 他の人の画面は、ページを再読み込みしなくてもリアルタイムで自動的に更新されます
  （claude.ai版であった「保存のたびに画面が切り替わる」問題はこの方式では起きません）
- サーバーは自分で起動しておく必要はありません。GitHub PagesもFirebaseも、
  アクセスがあったときだけ自動で動く仕組みです

## 困ったときは

- 画面に赤字でエラーが出ている場合は、`index.html` の `firebaseConfig` の
  値が正しいか、Firestoreのルールが正しく貼り付けられているかを確認してください
- 何か崩れていたり動かない場合は、エラーメッセージのスクリーンショットを
  共有してもらえれば調べます
