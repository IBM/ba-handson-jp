# エージェント型ワークフローを実装してみよう！

AIAgentはエージェンティックな振る舞いによって様々な処理を呼び出しますが、一連の処理を事前定義して制御したいケースもあります。
エージェント型ワークフローとは、エージェントが一連の処理を再利用可能な構造として実行する仕組みです。ツール呼び出し、入力取得、分岐やループなどを定義し、開始から完了までを自動管理します。
このLabでは、パスポートの顔写真ページをもとに銀行口座開設を受け付けるエージェント型ワークフローをフロー・ビルダーを用いて定義し、エージェントから呼び出す方法について確認します。

## 参考データについて

ここではパスポート画像の例として、こちらの論文「[SynID: Passport Synthetic Dataset for Presentation Attack Detection](https://arxiv.org/html/2505.07540v1){:target="_blank"}」に掲載されている合成パスポート画像を参照します。  
  
本サイトでは、当該論文に含まれる画像ファイルの再配布や二次利用は行いません。実際の画像および詳細は、必ず原論文をご参照ください。  


## エージェント型ワークフローの作成
1. 左上のメニューから **ビルド** を選択します。  
![alt text](flow-OpenAccount_images/flow_image0010.png)  


2. 左側の **すべてのツール** を選択し、**ツールの作成** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0020.png)

3. **エージェント型ワークフロー** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0030.png)  


4. 名前に**XX_CreateNewAccountWorkflow** (XXにはイニシャルを設定してください。) を入力し、**構築の開始**ボタンをクリックします。  
![alt text](flow-OpenAccount_images/flow_image0040.png)  

5. 名前の右にある**詳細の編集**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0050.png)  

6. 説明に次の値を設定し、**完了**をクリックします。（このワークフロー・ツールでは入出力を必要としませんが、入出力が必要な場合は**パラメーター**タブで設定できます。）
```     
このツールは、新しい銀行口座を開設するためのリクエストを作成します。入力は必要ありません。
```     
![alt text](flow-OpenAccount_images/flow_image0060.png)  

## ドキュメントからの詳細情報抽出
ここでは、パスポート画像から口座開設に必要な詳細情報（パスポート番号、国籍、氏名などの構造化データ）を収集・抽出するためのユーザー・アクティビティとドキュメント抽出・アクティビティを設定します。

1. watsonx Orchestrate のワークフローに戻り、**Add +** アイコンをクリックし、 **ユーザーに提示する** > **メッセージ** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0070.png)  

2. **ユーザー・アクティビティー1** を選択し、鉛筆アイコンをクリックして名前を **パスポートのアップロード** に変更します。  
![alt text](flow-OpenAccount_images/ドキュメントからの詳細情報抽出_2.png)  

3. **メッセージ1** を選択し、次のメッセージを追加します。
```     
開始するには、パスポートの画像をアップロードしてください。
```     
![alt text](flow-OpenAccount_images/flow_image0090.png)  

4. **メッセージ1** のボックスの下の矢印にカーソルを合わせて表示される **+** アイコンをクリックし、 **ユーザーから収集する** > **ファイルのアップロード** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0100.png)  

5. **パスポートのアップロード**のボックスの下の矢印にカーソルを合わせて表示される **+** アイコンをクリックし、 **フローアクティビティを追加する** > **ドキュメント抽出** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0110.png)  

6. **文書フォーマットの選択**では**無構造**を選択します。  
![alt text](flow-OpenAccount_images/flow_image0120.png)  

7. トレーニング用にパスポート画像のファイルをアップロードします。  
![alt text](flow-OpenAccount_images/flow_image0130.png)  

8. **フィールドの追加 +**から次の５つのフィールドを追加します。  
    - Passport number  
    - Nationality  
    - Given name  
    - Surname  
    - Date of birth  
![alt text](flow-OpenAccount_images/flow_image0140.png)  

10. **Nationality** はパスポートの母国語で記載されているため、英語の国名に統一する必要があります。**Nationality** にマウスを移動し、表示される**フィールドの詳細を表示**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0150.png)  

11. **説明** と **例** のトグルをオンにして、それぞれ次の内容を追加後に**文書に表示**をクリックします。正しく値が取得できていることを確認し、**←** をクリックして前の画面に戻ります。  
     **説明:**
     ```
     国籍を検出します。「Nationality」というフィールドで確認できます。
     国名が母国語で表示されている場合は、英語の正式な国名表記に変換してください。国籍形容詞は使用しないでください。
     ```
     **例:**

     | 入力              | 出力      |
     | ---------------- | -------- |
     | PORTUGUESA       | Portugal |
     | POLSKIE / POLISH | Poland   |
     | ESPAÑOLA         | Spain    |

     ![alt text](flow-OpenAccount_images/flow_image0160.png)

12. **Date of birth** についても誤ってマッピングされることを回避し、表記を統一させます。**Date of birth** にマウスを移動し、表示される**フィールドの詳細を表示**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0180.png)  

13. **データ・タイプ**を**日付**にします。**説明** のトグルをオンにして、次の内容を追加後に**文書に表示**をクリックします。(**例** のトグルはオフにします。)YYYY-MM-DD形式で値が正しく取得できていることを確認し、**←** をクリックして前の画面に戻ります。  
     **説明:**
     ```
     パスポート画像から生年月日を抽出します。「生年月日（Date of Birth）」「生年月日（Date de naissance）」などのラベル、または同様の多言語表記を探します。日付はYYYY-MM-DD（例：1988-08-08）という一貫した形式で返します。
     ```
     ![alt text](flow-OpenAccount_images/flow_image0190.png)

14. **文書の管理**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0210.png)  

15. 追加でトレーニングする場合には **文書の追加** をクリックし、別のパスポート画像のファイルをアップロードします。アップロード完了後、**完了**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0220.png)  

16. 追加したパスポート画像のファイルに切り替えます。  
![alt text](flow-OpenAccount_images/flow_image0230.png)  

17. 値が取得できていることを確認し、右上の **×** をクリックしてドキュメント抽出を閉じます。   
![alt text](flow-OpenAccount_images/flow_image0240.png) 

## ブランチ（分岐）の追加
ここでは、パスポートから抽出された国籍情報をもとにフローを条件分岐する処理を追加します。  

1. **ドキュメント抽出** のボックスの下の矢印にカーソルを合わせて表示される **+** アイコンをクリックし、 **フロー制御を追加する** > **ブランチ** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0300.png)  

2. 追加した**ブランチ**を選択し、鉛筆アイコンをクリックして名前を **国籍?** に変更します。また、**パス1**をクリックして名前を **スペイン** に変更し、**パス2**をクリックして名前を **その他の国籍** に変更します。
![alt text](flow-OpenAccount_images/flow_image0310.png)  

3. スペインの横にある **条件の編集** をクリックし、次の条件を設定します。  
**nationality == Spain**
![alt text](flow-OpenAccount_images/flow_image0320.png)  

4. **その他の国籍**のパスの**Add +** アイコンをクリックし、 **ユーザーに提示する** > **メッセージ** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0340.png)  

5. **ユーザー・アクティビティー2** を選択し、鉛筆アイコンをクリックして名前を **サポート対象外** に変更します。  
![alt text](flow-OpenAccount_images/flow_image0350.png)  

6. **メッセージ2** を選択し、名前を **サポート対象外メッセージ** に変更して、次のメッセージを追加します。
```     
現在、スペイン国籍のみサポートしています。その他の国籍については、最寄りの支店へお越しください。
```     
![alt text](flow-OpenAccount_images/flow_image0360.png)  

## human-in-the-loopフォームの作成
ここでは、口座開設に必要な詳細な情報をユーザーが確認、入力するためのフォームを追加します。  

1. **スペイン** のパスにカーソルを合わせて表示される **+** アイコンをクリックし、 **フォームの追加** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0370.png)

2. 名前を**アカウントの詳細**に変更し、**「キャンセル」ボタン**のトグルをオフにます。  
![alt text](flow-OpenAccount_images/flow_image0375.png)

3. **フィールドの追加** あるいは **+** アイコンをクリックし、**ユーザーから収集する**を選んで次のフィールドを追加していきます。  

     | Label        | type     |
     | ------------ | -------- |
     | 姓           | テキスト  |
     | 名           | テキスト  |
     | 国籍         | テキスト  |
     | パスポート番号 | テキスト  |
     | 生年月日      | **日付/時刻（入力タイプは日付）** |
     | 連絡先電話番号 | テキスト  |
     | 年収         | **数値**  |
     | 口座種類      | **単一選択**  |

     ![alt text](flow-OpenAccount_images/flow_image0376.png)

4. 次のようになっているはずです。  
![alt text](flow-OpenAccount_images/flow_image0380.png)

5. **口座種類** を選択して、ソース変数の **Expression** アイコンをクリックして、次の口座種類のリストを入力します。
["当座預金","普通預金"]  
![alt text](flow-OpenAccount_images/flow_image0390.png)

6. **姓**、**名**、**国籍**、**パスポート番号**、**生年月日** について、動作セクションの **初期値** を **On** に設定し、**Variable** > **ドキュメント抽出** をクリックしてパスポートから抽出した変数をマッピングします。

     | Label         | マッピングする項目  |
     | ------------- | ---------------- |
     | 姓            | surname          |
     | 名            | given_name       |
     | 国籍          | nationality      |
     | パスポート番号  | passsport_number |
     | 生年月日       | date_of_birth    |

     ![alt text](flow-OpenAccount_images/flow_image0400.png)

7. **連絡先電話番号** と **口座種類** については、**Required** を **On** にします。   
![alt text](flow-OpenAccount_images/flow_image0410.png)  

8. 右上の **×** をクリックしてアカウント作成リクエストを閉じます。   
![alt text](flow-OpenAccount_images/flow_image0420.png)  

9. **ユーザー・アクティビティー3** をドラッグ&ドロップで見やすい位置に移動し、鉛筆アイコンをクリックして名前を **アカウント作成リクエスト** に変更します。  
![alt text](flow-OpenAccount_images/flow_image0430.png)  

## 生成プロンプトの追加  
ここでは、生成 AI モデルを使用してパーソナライズされた確認メッセージを生成します。  

1. **アカウントの詳細** のボックスの下の矢印にカーソルを合わせて表示される **+** アイコンをクリックし、 **フローアクティビティを追加する** > **生成プロンプト** をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0500.png)

2. モデルは「GPT-OSS」のまま変更せず、**システム・プロンプト** に次の内容を入力します。
```     
あなたは、口座開設に関する最終確認メッセージを生成するアシスタントです。提供されたユーザー情報をもとに、送信可能な完全なメッセージを作成してください。指示やコードは含めず、最終的な文章のみを出力してください。
```     

3. **追加 +**ボタンから次の入力変数を追加します。テスト用のサンプル入力も指定してください。

     | 名前                  | データの型 | テスト値 |
     | -------------------- | -------- | -------  |
     | first_name           | ストリング | (任意)   |
     | last_name            | ストリング | (任意)   |
     | account_type         | ストリング | 普通預金 or 当座預金 |
     | contact_phone_number | ストリング | (任意)   |
 

4. ユーザープロンプトで、以下を追加します。  
     ```     
     以下の情報を使用して確認メッセージを生成してください。
     名：{first_name}
     姓：{last_name}
     お客様の電話番号：{contact_phone_number}
     口座種別：{account_type}
     メッセージは、以下の要件を満たす必要があります。
     
     1.お客様のフルネームに対する挨拶と感謝の言葉から始めること
     2.口座開設の申請が正常に送信されたことを確認すること
     3.ランダムな8桁の参照番号を含めること
     4.次の手続きについて、当社の担当者がお客様の電話番号にお電話する旨を記載すること
     5.会社名「AgentXBankingCo.」で締めくくること
     ```     
     
     ![alt text](flow-OpenAccount_images/flow_image0510.png)

5. 確認メッセージの生成をテストするには、**プレビューの生成** をクリックします。右上の **×** をクリックして生成プロンプトを閉じます。 
![alt text](flow-OpenAccount_images/flow_image0520.png)

6. 右上の **×** をクリックして生成プロンプトを閉じます。 
![alt text](flow-OpenAccount_images/flow_image0521.png)

7. **生成プロンプト** のボックスの下の矢印にカーソルを合わせて表示される **+** アイコンをクリックし、 **ユーザーに提示する** > **メッセージ** をクリックします。 
![alt text](flow-OpenAccount_images/flow_image0530.png)

8. **メッセージ2** の名前を **確認メッセージ** に変更します。さらに、**出力メッセージ** の **変数の選択** をクリックして、 **生成プロンプト** > **value** をマッピングします。
![alt text](flow-OpenAccount_images/flow_image0540.png)

9. これでエージェント型ワークフローの作成が完了し、以下のようになります。
![alt text](flow-OpenAccount_images/flow_image0550.png)

10. 「完了」をクリックします。
![alt text](flow-OpenAccount_images/flow_image0560.png)


## エージェントへのツール追加とデプロイ
1. Lab1で作成した **XX-IBMInfo** を開きます。  
![alt text](flow-OpenAccount_images/flow_image0600.png)

2. **ツールの追加** ボタンをクリックします。  
![alt text](flow-OpenAccount_images/flow_image0610.png)

3. **ローカル・インスタンス** を選択します。  
![alt text](flow-OpenAccount_images/flow_image0620.png)

4. 作成した **XX_CreateNewAccountWorkflow** のチェックボックスを有効にして **エージェントに追加** ボタンをクリックします。（自分が作成したツールが見つかりにくい場合は検索ボックスを活用してください。）  
![alt text](flow-OpenAccount_images/flow_image0630.png)  

5. **動作** の **指示** に次の内容を追記してください。  
```     
ユーザーが新しい銀行口座の開設を要求した場合は、ツール 「CreateNewAccountWorkflow」 を呼び出し、それ以外の出力は一切生成または表示しないでください。
このツールがプロセス全体の処理およびユーザーへの最終的な応答の提示をすべて担当します。
あなたの役割は、タスクをツールに委任することのみに限定されます。
ツールの出力にはすでに必要なユーザー向けメッセージが含まれているため、繰り返し、要約、またはツールの出力内容を引用しないでください。
```     
![alt text](flow-OpenAccount_images/flow_image0640.png)  

6. **デプロイ** をクリックし、**デプロイ前のサマリー**画面が表示されたらもう一度**デプロイ**をクリックします。  
![alt text](flow-OpenAccount_images/flow_image0650.png)  


## エージェント型ワークフロー・ツールの実行
1. 左上のメニューから **チャット** を選択します。  
![alt text](flow-OpenAccount_images/flow_image0700.png)

2. エージェントのドロップダウン・リストから **XX-IBMInfo** を選択します。  
![alt text](flow-OpenAccount_images/flow_image0710.png)

3. チャット欄に「**新しい銀行口座を開設する**」と入力して送信します。  
![alt text](flow-OpenAccount_images/flow_image0720.png)

4. **ファイルの追加** をクリックして、パスポート画像のファイルをアップロードして送信してください。  
![alt text](flow-OpenAccount_images/flow_image0730.png)

5. エージェント型ワークフローでパスポートの詳細情報を抽出し、抽出内容の信頼性が高くない場合には**文書抽出のレビュー**が表示され、ユーザーに確認を求めます。**文書抽出のレビュー**が表示された場合には **表示** をクリックしてください。信頼性高く抽出できた場合には**文書抽出のレビュー**は表示されませんので、このステップはスキップしてください。  
  ![alt text](flow-OpenAccount_images/flow_image0740.png)  
  
    - 抽出内容を確認し、必要に応じて修正します。  
    - 例として下の画像では **生年月日** を修正しています。  
    - 修正後、**送信** をクリックします。
    - **抽出の確認**画面が表示された場合は、 **確認して送信** をクリックして確定します。  
  ![alt text](flow-OpenAccount_images/flow_image0750.png)

6. パスポートの国籍がスペインかその他の国かで後続処理が変わります。

    - スペインの場合
          1. **アカウントの詳細** 画面が表示されます。
          2. 抽出された情報がフォームに事前入力されていることを確認します。
          3. **連絡先電話番号**、**年収**、**口座種類**を入力します。
          4. **Submit**をクリックします。  
          ![alt text](flow-OpenAccount_images/flow_image0770.png)
     - その他の国籍の場合
          1. **現在、スペイン国籍のみサポートしています。その他の国籍については、最寄りの支店へお越しください。** と表示されます。  
          ![alt text](flow-OpenAccount_images/flow_image0760.png)

## お疲れさまでした！
このハンズオンでは、フロー・ビルダーを用いたエージェント型ワークフローの作成について説明しました。