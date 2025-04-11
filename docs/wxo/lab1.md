# Lab1 Hello world

このLabでは、watsonx Orchestrateにログインし、カスタム・スキルを追加します。

## watsonx Orchestrateの画面を確認してみよう
このセクションでは、watsonx Orchestrateの主な画面構成について学びます。

 1. watsonx Orchestrateにログインします。
 2. IBM-idを入力して、**Continue** ボタンをクリックします。
 3. 複数テナントに所属する場合には、テナント選択画面が表示されるので、適切なテナント名を選択してください。  
　
　![alt text](lab1_images/lab1-1.png)

 4. 以下のような画面が表示されます。（ツアーのダイアログが表示された場合にはスキップしてください。）  
　![alt text](lab1_images/lab1-2.png)

 5. 詳細は以下の通りです。

    1.メニュー  
    各画面へ遷移するためのメニューが表示されます。    

    2.チャット画面  
    watsonx Orchestrateへ様々な処理を依頼するためのチャット画面    

    3.AI assistant builder 
    外部のWebサイトなどに組みこんだり、Slackなどのチャネルと統合可能なAIアシスタントの構成が可能です。  

    4.Skill studio 
    スキルの追加やスキルフローの作成、意思決定、ワークフロー、生成AIに関する処理をローコードで実装し、スキル(Automations)として公開が可能です。  

    5.ヘルプ  
    各種ヘルプへのリンクが表示されます。複数アカウントに所属する場合には、Swtich Accountからアカウントの切り替えが可能です。  

    6.タスク・リスト  
    ワークフローを使用している場合に、自分に割り当てられたタスクが表示されます。  
    
    7.プロファイル/設定  
    ユーザーのプロファイル設定が可能です。

6. テナントIDを取得するには、右上のイニシャルが表示されているアイコンをクリックし、製品情報を選択します。  
![alt text](lab1_images/lab1-3.png)

7. 以下のようなダイアログが表示されます。crnからはじまる文字列がIDです。サポートに問い合わせる際にはこのIDを伝える必要があるため、覚えておきましょう。  
![alt text](lab1_images/lab1-4.png)

## スキルの追加
watsonx Orchestrateには、プリビルド・スキルと呼ばれる1000以上の様々なスキルが付属し、すぐに使用することが可能ですが、以下の方法でスキルを追加することも可能です。：

- Open APIの定義ファイル(json/yaml)をインポートする。
- 既存スキル(ワークフローやRPA)のディスカバリー
- スキルフローの作成
- Automation Builderによるスキルの実装

このLabでは、OpenAPI定義をインポートしてスキルをカタログに追加する方法について学びます。では、始めましょう！

1. まず[こちらのリンク](https://ojnlvnhry9.execute-api.us-west-1.amazonaws.com/prod/hello-world)をクリックして、Hello world OpenAPIが利用可能であることを確認してください。 {"statusCode": 200, "body": "Hello From My Lambda!"}と表示されればOKです。このAPIはAWS上で動作するシンプルなAPIです。

2. [こちらのリンク](./files/helloworld-watsonx.json)を右クリックしてファイルに保存してください。このファイルは先ほどのAPIの定義情報です。

3. notepad, VSCode, vi/vimといったお好みのエディタでファイルを開き編集します。
```
    "openapi": "3.0.1",
    "info": {
        "title": "Your-Initials Hello World App Example",
        "description": "This is a Hello World Example",
        "version": "1.0.0"
    },
    "servers": [
        {
            "url": "https://ojnlvnhry9.execute-api.us-west-1.amazonaws.com/prod/"
        }
    ],
    "security": [
        {
            "basic_auth": []
        }
    ],
    "paths": {
        "/hello-world": {
            "get": {
                "operationId": "hello",
                "summary": "Your-Initials Hello World WatsonX",
                "responses": {
```
4. インポート時に他のハンズオン実施者とスキルが重複しないように、ファイルの中の、title,summaryをユニークなものにします。これらの値のYourInitialsの部分をあなたのイニシャルに置き換えてファイルを保存してください。更新後のファイルは以下のようになるはずです。(イニシャルの置き換え箇所は、2か所あります。)
```
    "openapi": "3.0.1",
    "info": {
        "title": "XX Hello World App Example",
        "description": "This is a Hello World Example",
        "version": "1.0.0"
    },
    "servers": [
        {
            "url": "https://ojnlvnhry9.execute-api.us-west-1.amazonaws.com/prod/"
        }
    ],
    "security": [
        {
            "basic_auth": []
        }
    ],
    "paths": {
        "/hello-world": {
            "get": {
                "operationId": "hello",
                "summary": "XX Hello World WatsonX",
                "responses": {
```

5. メニューから**スキル・スタジオ**を選択します。  
![alt text](lab1_images/lab1-5.png)

6. 右上にある**作成**ボタンをクリックし、**APIのインポート**をクリックします。  
![alt text](lab1_images/lab1-6.png)

7. **ファイルから**を選択してください。  
![alt text](lab1_images/lab1-7.png)

8. 先ほど保存したファイルをドラッグ＆ドロップするか、クリックして選択してください。
9. ファイルの検証が実施され、問題無ければ以下の様に表示されます。  
![alt text](lab1_images/lab1-8.png)

10. 右下の**次へ**をクリックしてください。追加するAPIの内容が表示されます。
11. Hello World WatsonXのチェックボックスをチェックし、右下の**追加**ボタンをクリックしてください。  
![alt text](lab1_images/lab1-9.png)

12. スキル一覧にインポートしたスキルが表示されるので、右側のメニューを展開し、**このスキルの強化**を選択してください。  
![alt text](lab1_images/lab1-10.png)

13. スキルの詳細が表示されます。スキルの強化画面から、スキルの入出力の表示、スキルを呼び出すためのフレーズ、watsonx Orchestrateが次に実行するスキルの提案を行う次善のスキルの設定などを行うことが可能です。  
![alt text](lab1_images/lab1-11.png)

14. **入力**タブをクリックしてください。入力タブでは、スキルの入力について確認することができます。スキルの入力パラメータに**name**があり、必須のパラメータで無いことが分かります。  
![alt text](lab1_images/lab1-12.png)

15. **出力**タブをクリックしてください。Outputタブでは、スキルの出力について確認することが可能です。スキルの出力パラメータとして**greeting**が定義されていることが分かります。  
![alt text](lab1_images/lab1-13.png)

16. **セキュリティー**タブをクリックしてください。セキュリティータブでは、認証情報や、URLを確認することができます。また、接続のテストを行うことができます。  
![alt text](lab1_images/lab1-14.png)

17. 接続のテストを実行してみましょう。**ユーザー名**に test@acme.com、**パスワード**にtestを入力します。今回使用するAPIについては認証不要のため、どんな値を入力しても問題ありません。

18. **送信**ボタンをクリックし、**認証に成功しました**と表示されることを確認します。  
![alt text](lab1_images/lab1-15.png)

19. **句**タブをクリックしてください。句タブでは、スキルを呼び出す際に使用するフレーズを登録します。ユーザーがここに登録した例文にマッチした文章をチャットから入力することでこのスキルが呼び出されます。複数のスキルがマッチした場合にはwatsonx Orchestrateは実行するスキルを確認してきます。既にフレーズが登録済みのはずですが、いくつかのフレーズを追加してみてください。また、入力フィールドにカーソルを移動させると表示される、**Auto-generate phrases**ボタンをクリックすることで生成AIを用いてフレーズのサンプルを自動生成することも可能です。  
![alt text](lab1_images/lab1-16.png)

20. **次善のスキル**タブをクリックしてください。このタブでは、スキルを実行した後に次に実行すべきスキルの選択肢として表示される複数のスキルを定義することが可能です。
![alt text](lab1_images/lab1-17.png)
    * 次善のスキルは10個まで登録することが可能です。
    * 次善のスキルに追加した各スキルに対して明示的に変数のマッピングを行うことが可能です。  

21. **発行**ボタンをクリックして、スキルを発行してください。  
![alt text](lab1_images/lab1-18.png)


## Personal Skillへのスキルの追加
ここまでの作業で、カタログにスキルを追加することができました。次のステップとしてPersonal Skillにカタログからスキルを追加します。

1. 左上のメニューから**チャット**を選択してチャット画面を表示します。  
![alt text](lab1_images/lab1-19.png)

2. 以下のような画面が表示されるはずです。左下の**スキルをカタログから追加**をクリックします。  
![alt text](lab1_images/lab1-20.png)

3. 検索フォームに先ほど作成したスキルの名前(hello world)を入力して検索を実行してください。表示されたアプリをクリックします。
![alt text](lab1_images/lab1-21.png)

4. スキルを選択して、**スキルの追加**をクリックしてスキルを追加します。  
![alt text](lab1_images/lab1-22.png)

5. 右上の**アプリの接続**をクリックしてアプリケーションと接続します。
![alt text](lab1_images/lab1-23.png)

6. username,passwordに任意の値を入力し、**アプリの接続**ボタンをクリックします。  
今回は、username は**test@acme.com**、passwordは**test**となります。
![alt text](lab1_images/lab1-24.png)

7. アプリケーションとの接続が完了しました！なお、接続はカタログからではなく、チャットからスキルを呼び出した際に実行することも可能です。  
![alt text](lab1_images/lab1-25.png)

8. メニューからチャット画面に戻ります。スキルが追加されていることが分かります。  
![alt text](lab1_images/lab1-26.png)

## スキルの動作確認
カタログから追加したスキルは以下の2つの方法で実行可能です:  
- チャット画面下部に表示されているスキルをクリックする  
- スキルをEnhanceした際に指定したフレーズ(watsonx Orchestrateが指定されたフレーズを元に学習するため、完全一致する必要はありません)を入力する

1. チャットの入力欄に、**Hello World WatsonX**と入力してみてください。  
![alt text](lab1_images/lab1-27.png)

2. 先ほど追加したスキルが呼び出され、入力フォームが表示されます。  
![alt text](lab1_images/lab1-28.png)

3. 自分の名前を入力し、**適用**ボタンをクリックしてください。  
![alt text](lab1_images/lab1-29.png)


4. 結果が表示されるはずです。  
![alt text](lab1_images/image-45.png)

4. チャットの内容は右上のホウキアイコンをクリックすることで削除することが可能です。  
![alt text](lab1_images/lab1-31.png)

## お疲れさまでした！
このハンズオンでは、watsonx Orchestrateにログインし、カスタムスキルを追加、強化してから発行し、カタログに追加しました。そしてカタログからスキルを追加し、チャット画面から呼び出しました。　