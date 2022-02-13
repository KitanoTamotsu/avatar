#### 開発メモ
ワークフロー
<br><img width="600" src="https://user-images.githubusercontent.com/40127279/126859618-eabe4143-8516-4786-a969-543967259814.png">

### 1.APIを使ってみる
　APIはhttps://joeschmoe.ioにありますが、今回使うのは3種類
<br>
<br>　ランダムアバター　https://joeschmoe.io/api/v1/random
<br>　女性アバター　　　https://joeschmoe.io/api/v1/female/xxxxx  (xxxxxは任意の文字)
<br>　男性アバター　　　https://joeschmoe.io/api/v1/male/xxxxx  (xxxxxは任意の文字)
<br>　このアドレスならv1/より後ろの部分をAlfredから入力させればいいかな。
<br>　指定がない場合はrandomその他は入力の文字列そのまま使う感じ。
<br>　　
<br>　cURLで何が返ってくるのかみてみると、svgタグです
<br>　初めてみましたが、ベクトル描画をするタグのようです
<br>　なるほど、svgタグを返すAPIなので簡単にHTMLにとりこめるのでしょう
### 2.SVGが表示できない
　ところがAPIからリターンされるSVGタグのテキストファイルをOpenURLに渡しても
<br>　何も表示されません
<br>　どうしましょう
### 3.ローカルファイルを作成する
　で、考えついたのがローカルファイルを作成すること
<br>　htmlを作ってオープンすればいけるのかなと
<br>　
<br>　スクリプトでリターンされたSVGタグをもとにhtmlを作ってみます
<br>　せっかくhtmlにするので、呼んだときのパラメータの表示と、サイズの設定をしてみました
<br>　サイズは、sedでwidthとheightをviewBoxの前に入れています
<br>　最後にcatで全体を{query}として出力します
<br>　
<br>　ScriptFilter
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/126859716-42760936-ed2d-4e8e-8925-73dcfc3856f9.png">
<br>　
<br>　次のステップはWriteTextFileでローカルファイルを作成します
<br>　このとき作成するファイルはalfredワークフローの環境変数とするとよいでしょう
<br>　ワークフロー右上の[χ]ボタンを押して登録します
<br>　nameとしてfilename、valueとして~/documents/joeschmoeavatar.htmlを設定してます
<br>　　　
<br>　Configure workflow and variables
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126859804-3f3b21d6-f885-41ea-a040-3ee39a7cb397.png">
<br>　
<br>　WriteTextFileステップの設定は簡単です
<br>　filename欄は{var:filename}、書き出す内容は{query}とするだけです
<br>　if existsはappendとして複数回実施した場合は、リターンを追加するようにしてみました
<br>　追加するということは使うたびにHTMLタグが増える形になりますがブラウザで普通に
<br>　表示できました
<br>　
<br>　WriteTextFile (Append to File)
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126859758-1d27c7e7-feed-4692-8cc6-e7de2909fdf8.png">
<br>　　　
<br>　最後は出来上がったhtmlの表示です
<br>　OpenFileを使いますが、このステップの設定も簡単で、Fileを{var:filename}とするだけです
<br>　前のステップでwriteしたファイルを開いてくれます
<br>　
<br>　OpenFile
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126859778-e53421f0-f81e-4a0f-9bcd-1b2110f0ea5b.png">

#### 背景
　Githubのページに自分の画像を載せれることに気づき何かないかと探していたら
<br>　joe schmoeのアバター作成APIを発見して、遊ぶ感じで作成してみました
#### 取扱説明
### 機能:
　avatar作成
### インストール:
　1.[alfredworkflow](https://github.com/KitanoTamotsu/avatar/releases/download/1.2/avatar.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　キーワード『avatar』＋パラメータを入力
<br>　　パラメータなし　→ ランダムにアバター作成
<br>　　female/xxxx → 女性のアバター作成
<br>　　male/xxxx → 男性のアバター作成
### 動き方：
　コアはhttps://joeschmoe.ioのアバター作成APIを使っています
<br>　APIのリターンからhtmlを作成して表示します

#### 修正履歴
### ver1.1(2021-04-04)：
　{var:filename}のパスを修正しました。
<br>　変更前）　~/documents/joeschmoeavatar.html
<br>　変更後）　~/documents/alfred/avatar/joeschmoeavatar.html
<br>　
<br>　AppendtoFileオブジェクトの"Create any intermediate folders"オプションを
<br>　オンにしました　
<br>　これによってファイルがない場合（＝初回起動）、ファイルを作成します
<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

