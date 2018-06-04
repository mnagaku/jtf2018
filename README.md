# openforum2018

NIIオープンフォーラム/オープンハウス2018のハンズオン/デモ用

<a href="http://play-with-docker.com?stack=https://raw.githubusercontent.com/mnagaku/openforum2018/master/docker-compose-from-github.yml"><img src="https://raw.githubusercontent.com/play-with-docker/stacks/master/assets/images/button.png" /></a>

## ストーリー

### 2018/05/31

M先輩から「このサーバがちょっと怪しいくって調査したいから、インシデントレスポンス第3版のp179に載ってるサーバの調査コマンド一式の結果のレポート作って。あ、あと、ポートが1つ LISTEN になってて http サーバになってるはずなんで、それが動いてるかも確認しといて。」と指示があった。

<font color="Red">
・Notebookをターミナル代わりに使う。紆余曲折があっても全部残す。メモの類はmdセルに書く
</font>

### 2018/06/04

M先輩から前回同様の指示があった。前回作業を踏まえて今回も作業するが、今後も同様の作業がありそうなので、リファクタリングしておく。

M先輩から「サービス止めないでファイルシステム舐めたりしてるので、作業が与えるサーバの負荷は気になる」とコメントがあったので、適時ロードアベレージを取っておくことにする。

<font color="Red">
・直近の類似の作業を行ったNotebookを複製して、作業日時を接頭辞に付し、Notebookベースの作業を行う作業後に残ったNotebookがそのまま証跡となる<p />
・紆余曲折で冗⾧なNotebookは、適当なタイミングで清書する<p />
・再利用性が高く、実行結果が安定している場合、実行結果をお手本に残す<p />
・前提条件を満たしていないと失敗する確認用のセル（Design By Contract）<p />
</font>

### 今日

後輩のXに前回作業のnbのコピーを渡して、作業を依頼する。Notebookの内容だけで分からなくて追加でググったことがあったら、追記しておくように指示した。

<font color="Red">
・Notebookの複製を渡して作業依頼。スキルトランスファーとして機能<p />
・Notebookの複製で作業する時、「説明が足りていない」「例外的な振る舞いに遭遇した」「システムの振る舞いが変わっていた」場合、追記・修正して、次に備える<p />
</font>
