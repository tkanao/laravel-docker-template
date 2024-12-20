# Laravel Lesson レビュー①

## Todo一覧機能

### Todoモデルのallメソッドで実行しているSQLは何か
SELECT * FROM todos

### Todoモデルのallメソッドの返り値は何か
todoテーブルが持つレコードを全件、Collectionクラスのインスタンスで取得する。

### 配列の代わりにCollectionクラスを使用するメリットは
豊富なメソッドが用意されているため、データの操作や変換を簡単に行うことができ、コードの記述量を減らすことができる。

### view関数の第1・第2引数の指定と何をしているか
Laravelでは、Controllerからbladeファイルへデータを渡すためにview関数を使用する。
 - 第一引数：画面に表示したいbladeファイルを指定
 - 第二引数：渡したい変数を連想配列の形で渡すことができる

### index.blade.phpの$todos・$todoに代入されているものは何か
 - $todos：todoテーブルのレコードを全件、Collectionクラスのインスタンスで代入されている
 - $todo：foreachでCollectionクラスのインスタンスを1行ずつ順番に代入していく

## Todo作成機能
 1. TodoControllerのstoreメソッドにpostする。
 2. リクエストクラスをインスタンス化する
 3. $inputs = $request->all()にて、$inputsに、create.bladeのフォームからPostされてきたデータをすべて代入。
 4. $todo->fill($inputs)にて、$inputsの連想配列を全件取得するが、Todoモデルのfillableにて、contentのみ代入可能にしているため、$todoにはcontentのみが代入される。
 5. $todo->save();により、DBへの登録を行う。

### Requestクラスのallメソッドは何をしているか
フォームから送信されてきた値を一括で取得する。

### fillメソッドは何をしているか
 1. 引数である$inputsの連想配列の値を取得。
 2. Todoインスタンスの対応するプロパティに一括で代入。

### $fillableは何のために設定しているか
$fillableを定義することによって、
$todo->fill()で一括代入できる項目に制限をかけることができる。

### saveメソッドで実行しているSQLは何か
INSERT INTO todos (content) VALUES ($content)

### redirect()->route()は何をしているか
contentの登録処理の完了後、route()の引数で指定したURLにリダイレクトさせる。
設定しなければ、ToDoが新規作成された後に画面が動かなくなってしまう。

## その他

### テーブル構成をマイグレーションファイルで管理するメリット
直接的にSQLを書く必要がないため、学習コストも低く、簡単にDB管理をすることができる。

### マイグレーションファイルのup()、down()は何のコマンドを実行した時に呼び出されるのか
 - up() php artisan migrate
 - down() php artisan migrate:rollback

### Seederクラスの役割は何か
ダミーデータの作成を行う。

### route関数の引数・返り値・使用するメリット
 - 引数：ルート名（Web.phpでnameを指定）
 - 返り値：Web.phpでname指定したURLを生成する
 - URLを短縮ができる。URLに変更があった場合に、全ての記述を修正する必要がなくなり（Web.phpのURLの変更のみ）、保守性が上がる。

### @extends・@section・@yieldの関係性とbladeを分割するメリット
 - @extends('親Blade')：親Bladeの記述を継承する
 - @section('content')：子Bladeに記載。挿入元。
   @section〜@endsectionの記載を継承元のBladeファイルに挿入する
 - @yield('content')：親Bladeに記載、挿入先。
   @section()の内容を@yield()の内容に挿入する

#### bladeを分割するメリット
同じ記述が複数回登場する場合、一つの親Bladeにまとめることで、
継承する全てのBladeで変更を反映させることができる。

### @csrfは何のための記述か
CSRF対策として、トークンを発行する。

### {{ }}とは何の省略系か
```
<?php echo ?>
```