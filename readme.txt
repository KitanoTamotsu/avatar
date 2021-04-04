avatar.alfredworkflowのメモ

機能：avatar作成

設定：
1.ワークフローをダウンロード
2.ファイルをダブルクリックしてワークフローに登録
3.キーワードavatar	を入力
　以下のパラメータを指定できます
　パラメータなし　→ ランダムにアバター作成
　female/xxxx → 女性のアバター作成
　male/xxxx → 男性のアバター作成


動き方：
コアはhttps://joeschmoe.ioのアバター作成APIを使っています
APIのリターンからhtmlを作成して表示します


開発メモ：

　1.APIを使ってみる
　　APIはhttps://joeschmoe.ioにありますが、今回使うのは3種類

　　ランダムアバター　https://joeschmoe.io/api/v1/random
　　女性アバター　　　https://joeschmoe.io/api/v1/female/xxxxx  (xxxxxは任意の文字)
　　男性アバター　　　https://joeschmoe.io/api/v1/male/xxxxx  (xxxxxは任意の文字)

　　キーワードのパラメータをオプションとして、指定がない場合はランダムその他はそのままAPIに渡します
　
　　cURLで何が返ってくるのかみてみると、svgタグです
　　初めてみましたが、ベクトル描画をするタグのようです
　　なるほど、<svg>タグを返すAPIなので簡単にHTMLにとりこめるのでしょう
   

　2.<SVG>が表示できない
　
　　ところがAPIからリターンされる<SVG>をqueryとしてOpen URLをしてみても表示されません
   どうしましょう


　3.ローカルファイルを作成する

　　で、考えついたのがローカルファイルを作成すること
　　htmlを作ってオープンすればいけるのかなと

　　スクリプトでリターンされた<SVG>タグをもとにhtmlを作ってみます
　　せっかくhtmlにするので、呼んだときのパラメータと表示させるサイズを埋め込んでいます
　　サイズは、sedでwidthとheightをviewBoxの前に入れています
　　最後にcatで全体を{query}として出力します

　　次のステップはWrite Text Fileでローカルファイルを作成します
　　このとき作成するファイルはalfredワークフローの環境変数とするとよいでしょう
　　ワークフロー右上の[χ]ボタンを押して登録します
　　nameとしてfilename、valueとして~/documents/joeschmoeavatar.htmlを設定してます
　　
　　Write Text Fileステップの設定は簡単です
　　filename欄は{var:filename}、書き出す内容は{query}とするだけです
　　if existsはappendとして複数回実施した場合は、リターンを追加するようにしてみました
   追加するということは使うたびに<HTML>タグが増える形になりますがブラウザで普通に表示できました
　　
　　最後は出来上がったhtmlの表示です
   Open Fileを使いますが、このステップの設定も簡単で、Fileを{var:filename}とするだけです
　　前のステップでwriteしたファイルを開いてくれます


　4.背景
　　Githubのページに自分の画像を載せれることに気づき何かないかと探していたら
　　joe schmoeのアバター作成APIを発見して、遊ぶ感じで作成してみました


Ver1.1(2021-03-20)：

　 {var:filename}のパスを修正しました。
　　変更前）　~/documents/joeschmoeavatar.html
　　変更後）　~/documents/alfred/avatar/joeschmoeavatar.html

   Append to Fileオブジェクトの"Create any intermediate folders"オプションをオンにしました　
   これによってファイルがない場合（＝初回起動）、ファイルを作成します


ver1.2(2021-04-04)：
   シェルスクリプトをbashからzshに変更




  
