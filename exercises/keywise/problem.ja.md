引数を３つ取る関数を１つだけエクスポートした**module**を作成してください。
引数はLevelUPデータベースのインスタンスが存在するディレクトリ、JSONファイルのファイルパス、
コールバック関数となっています。

## **module**の仕様

GitHubリポジトリとユーザーネームのマッピングが書かれたJSONファイルを読み出し、
それらのデータを検索できるようにLevelUPデータストアに保存するモジュールを書きましょう。

１つ目のコマンドライン引数はデータを書き込むLevelUPデータベースのパスになります。

２つ目のコマンドライン引数はJSONファイルへのパスになります。JSONを読み込みJavascriptオブジェクトに変換するために
`require(JSONファイルのパス)`が使えます。

JSONファイルは２種類の行を持つ配列です。１つはユーザーです。

```javascript
{
  type: "user",
  name: "maxogden"
}
```

もう１つはリポジトリです。

```javascript
{
  type: "repo",
  name: "mux-demux",
  user: "dominictarr"
}
```

このファイルのすべてのエントリをデータストアに書き込まなければいけません。

データストアを開き、`!`を区切り文字としてデータを書き込んでください。そうすることで検証用スクリプトは
以下の範囲問い合わせを実行し、**各ユーザーのリポジトリ**を読み込むことができます。

```javascript
db.createReadStream({
  start: 'rvagg!',
  end: 'rvagg!~'
})
```

ユーザーデータは以下の方法で取得できるようにして下さい。

```javascript
db.get('rvagg', function (err, user) { ... })
```

データストアの各エントリの値はファイルから取得したオリジナルのJSONオブジェクトと同じでなけれななりません。

---

## ヒント:

JSONの扱いを簡単にするために、`valueEncoding: 'json'`のオプションを使用しLevelUPストアを
開くことができます。以下は例です。

```javascript
var db = level(databaseDir, { valueEncoding: 'json' })
```

こうすることでオブジェクトをデータとして直接ストアに書き込むことが出来、自分でJSONデータの
変換をする必要が無くなります。
これは同様に、読み込み処理は再構成されたオブジェクトを受け取ることを意味します。

`levelmeup run program.js`を実行する時にデバッグ用の出力を取得したい場合は、
console.logの代わりにconsole.errorを使用してください。
