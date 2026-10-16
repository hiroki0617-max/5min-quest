# 5分筋トレクエスト — 公開・同期用パッケージ

このパッケージは、スマホ向けのWebアプリ、共有ルームURL、Firebase Realtime Databaseによるリアルタイム同期を含みます。

## 重要：公開URLについて
このパッケージ自体は、まだインターネット上に公開されていません。公開URLを作るには、Firebase設定値を入れたうえで、ホスティング先（例：Firebase Hosting）へデプロイしてください。デプロイ後に発行されるURLをLINEで共有できます。

## 1. Firebaseを設定する
1. Firebaseプロジェクト `my-training-52b74` を使用。
2. プロジェクトの「Authentication」→「Sign-in method」で **Anonymous（匿名）** を有効化。
3. 「Realtime Database」を作成。
4. 「プロジェクトの設定」→「マイアプリ」からWebアプリを登録し、`apiKey`, `authDomain`, `databaseURL`, `projectId`, `appId` を確認。
5. この配布版の `index.html` には、今回のFirebaseプロジェクトの設定値を入力済みです。
6. Realtime Database のルールに `database.rules.json` の内容を設定する。

## 2. Firebase Hostingで公開する
PCにNode.jsを入れ、ターミナルでこのフォルダに移動して実行：

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
```

設定時は、既存のFirebaseプロジェクトを選び、公開ディレクトリを現在のフォルダ（または `public` にしてファイルをそこへ移動）にし、シングルページアプリとしての設定は「No」で構いません。`index.html` と `manifest.json` を公開ディレクトリに配置します。

次に：

```bash
firebase deploy
```

成功すると `https://<project-id>.web.app` のような公開URLが表示されます。これをLINEで共有してください。

## 3. 2人で同じ記録を使う
- 最初に公開URLを開くと、URL末尾に `?room=...` の共有ルームIDが作られます。
- 画面の「リンクをコピー」または「共有」を押して、友達へ送ります。
- 友達はその同じURLを開きます。同じルームIDのデータがリアルタイム同期されます。
- ルームIDが異なるURLは別々の記録になります。

## セキュリティ上の注意
このサンプルは匿名ログイン＋推測しにくいルームIDで簡易共有する試作です。データベースルールはログイン済み匿名ユーザーに読み書きを許すため、本番・機密情報用途には不十分です。公開利用を広げる場合は、参加者のUIDをルームごとに登録してルールで検証する設計へ強化してください。共有URLは信頼できる相手にだけ送ってください。

## 含まれるファイル
- `index.html` — アプリ本体
- `manifest.json` — ホーム画面追加向けのWeb App Manifest
- `database.rules.json` — Firebase Realtime Databaseの簡易ルール
- `README.md` — この手順書

## 現時点の制約
- 本パッケージは公開・同期に必要なコードと手順を用意するものです。こちらからユーザーのFirebaseアカウントにアクセスしてデプロイすることはできないため、公開URLはデプロイ完了後に発行されます。
- Firebase未設定の状態では、同期ステータスに設定が必要と表示されます。
