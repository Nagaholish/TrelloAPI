# TrelloAPI
TrelloAPIお作法メモ

## 手順

### APIを使う準備

* PowerUpページを通して、ボードにWebAPIアプリを使う事を登録する

登録ページ：https://trello.com/power-ups/admin

例
<img width="765" alt="trello-powerup-admin" src="https://user-images.githubusercontent.com/20953672/208375257-04ef0e00-e7a5-4a7c-90b4-c825756fe589.png">

* 次に、APIキー、秘密トークン、APIトークン（個人用トークン）を生成する

例
<img width="751" alt="trello-powerup-apikey" src="https://user-images.githubusercontent.com/20953672/208376002-a2358f70-e11a-4a63-8dde-139187c38d2b.png">

APIキー、トークンは他人に公開しないようにする

これでひとまずAPIが使えるようになる

### APIを使う

* どのAPIにも↑で取得した「APIキー」と「APIトークン」を使用するので覚える
* APIリファレンス：https://developer.atlassian.com/cloud/trello/rest/

例
```
curl 'https://api.trello.com/1/members/me/boards?key={yourKey}&token={yourToken}'

※
yourKey=APIキー
yourToken=APIトークン
```

* APIを呼ぶ際の引数がidを指定してくるので、id（メンバー、ボード、カード、、、）を把握する必要がある

* ボードのidとnameを取得する（fields=nameを指定する必要がある）
```
curl 'https://api.trello.com/1/members/me/boards?key={APIキー}&token={APIトークン}&fields=name'
```
実行すると
```
{"id":"xxxxxxxxxxxxxxxxxxxxxxxx","name":"ボード名"},
{"id":"yyyyyyyyyyyyyyyyyyyyyyyy","name":"ボード名"}
```
のようなレスポンスを得る

このidがidBoardに当たる

* ボードにあるリストを取得する

```
コマンド
curl --request GET --url 'https://api.trello.com/1/boards/{idBoard}/lists?key={APIキー}&token={APIトークン}&fields=name' --header 'Accept: application/json'

レスポンス
{"id":"aaaaaaaaaaaaaaaaaaaaaaaa","name":"リスト名"},
{"id":"bbbbbbbbbbbbbbbbbbbbbbbb","name":"リスト名"}
このidがidListに当たる
アーカイブされているリストは含まれない
```

* リストにあるカードを取得する

```
コマンド
curl --request GET --url 'https://api.trello.com/1/lists/{idList}/cards?key={APIキー}&token={APIトークン}&fields=name' --header 'Accept: application/json'

レスポンス
[
{"id":"cccccccccccccccccccccccc","name":"カード名"},
{"id":"dddddddddddddddddddddddd","name":"カード名"},
{"id":"eeeeeeeeeeeeeeeeeeeeeeee","name":"カード名"}
]
このidがidCardに当たる
```

## まとめ
* まずは各id（アカウント、ボード、リスト、カード）を知る必要がある
* 特定のカードを追いかけたいのに、idを知るための手順が多い
  * アカウント＞アカウントが持つボードを特定＞ボードが持つリストを特定＞リストがもつカードを特定<br>のように、検索に検索をするしかなさそう
* idがわかってしまえば、あとはAPIで状態を追いかけられるっぽい
