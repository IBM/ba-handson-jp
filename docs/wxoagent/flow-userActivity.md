# エージェント型ワークフローを実装してみよう！

AIAgentはエージェンティックな振る舞いによって様々な処理を呼び出しますが、事前にフローを定義してその振る舞いを完全に制御したいケースもあります。
エージェント型ワークフローとは、エージェントが一連の処理を再利用可能な構造として実行する仕組みです。ツール呼び出し、入力取得、分岐やループなどを定義し、開始から完了までを自動管理します。  
このLabでは、フロー・ビルダーを用いてフローを定義し、エージェントから呼び出す方法について確認します。

## フローの作成の開始
1. 左上のメニューから **エージェント型ワークフロー** を選択します。  
![alt text](flow-userActivity_images/flow_ua_image0010.png)  


2. **エージェント型ワークフローを追加** をクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0020.png)  

3. 名前に**XX_weatherFlow** (XXにはイニシャルを設定してください。) を入力し、**構築の開始**ボタンをクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0040.png)  

4. 名前の右にある**フロー設定**をクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0050.png)  

5. **説明**に次の値を設定します。
    ```    
    特定の都市の緯度、経度から天気情報を取得し、都市の気温に応じた今日のおすすめの過ごし方を表示する。  
    ```    
    ![alt text](flow-userActivity_images/flow_ua_image0060.png)  

6. 左上の**パラメーター**タブをクリックします。
    1. 次の3つの値を設定します。**入力の追加** ボタンを押して**ストリング**を選択し、名前と説明を設定する作業を繰り返してください。  

         | 名前         | 名前         | 説明      |  
         | ----------- | ----------- | -------- |  
         | ストリング（string）| city_name   | 都市名    |  
         | 10進法（decimal）  | latitude    | 緯度      |  
         | 10進法（decimal）  | longitude   | 経度      |  

        ![alt text](flow-userActivity_images/flow_ua_image0070.png)  
    2. 下図のように設定されていることを確認し、**完了** をクリックします。  
        ![alt text](flow-userActivity_images/flow_ua_image0080.png)  

## ツールの配置とデータマッピング
1. パレットの**ツール**タブを選択します。（パレットが表示されていない場合は、画面左上の![alt text](flow-userActivity_images/flow_ua_image0100.png)アイコンをクリックすると表示されます。）検索ボックスに **weather** と入力し、表示された **current weather for coordinates** を **入力** と **出力** の間に **ドラッグ&ドロップ** します.  
![alt text](flow-userActivity_images/flow_ua_image0110.png)  

2. 配置したcurrent weather for coordinatesをクリックし、表示された **データマッピングの編集** をクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0120.png)  

3. **current_weather** にはオート・マップではなくtrueを設定するため、オート・マップ の×をクリックして削除します。  
![alt text](flow-userActivity_images/flow_ua_image0130.png)  

4. さらに **値を入力してください** をクリックし、**はい**に切り替えます。  
![alt text](flow-userActivity_images/flow_ua_image0140.png)  

5. **自動マッピングが成功しない場合は、ユーザーにインプットを求めてください**を**オフ**にします。  
![alt text](flow-userActivity_images/flow_ua_image0150.png)  

6. 右上の×をクリックしてマッピング画面を閉じます。  

## ブランチ(分岐)とユーザーアクティビティの定義
ユーザーが入力した都市の気温に応じて、今日のおすすめの過ごし方を表示するように設定します。  

1. **current weather for coordinates** と **出力** の間の矢印にカーソルを合わせ、**＋** マークをクリックします。表示されたメニューから **フロー制御を追加する** → **ブランチ** を選択します。
![alt text](flow-userActivity_images/flow_ua_image0310.png)  

2. 追加した **ブランチ1** と **出力** の間の矢印(**パス1**)にカーソルを合わせて **＋** マークをクリックし、表示されたメニューから **ユーザーに提示する** → **メッセージ** を選択します。  
![alt text](flow-userActivity_images/flow_ua_image0320.png)  

4. パス2の **Add** をクリックし、表示されたメニューから同じく **ユーザーに提示する** → **メッセージ** を選択します。2つのメッセージは見やすいようにドラッグ&ドロップで位置を調整してください。  
![alt text](flow-userActivity_images/flow_ua_image0330.png)  

5. ブランチを編集します。
     1. **ブランチ1** をクリックして **条件の編集** をクリックします。  
     ![alt text](flow-userActivity_images/flow_ua_image0340.png)  
     
     2. **if**の右にある **+** ボタンをクリックし、表示されたセレクト変数において **current weather for coordinates** > **current weather temperature** を選択します。  
     ![alt text](flow-userActivity_images/flow_ua_image0350.png)  
    
     3. **オペレーター** として **<=** を選択します。  
     ![alt text](flow-userActivity_images/flow_ua_image0360.png)  

     4. 値として **15** を設定します。  
     ![alt text](flow-userActivity_images/flow_ua_image0370.png)  
     
     5. パス1の右にある鉛筆マークをクリックし、名前を **15度以下** に変更します。  
     ![alt text](flow-userActivity_images/flow_ua_image0380.png)  
     
     6. 名前の左にある **← (Back)** をクリックしてブランチ1に戻ります。  
     ![alt text](flow-userActivity_images/flow_ua_image0390.png)  

     7. パス2をクリックし、**15度より高い** に変更します。  
     ![alt text](flow-userActivity_images/flow_ua_image0400.png)  

6. 次に **メッセージ** を編集します。
     1. **メッセージ1** をクリックし、編集ボタンをクリックし、名前とエージェント・メッセージを以下のように指定します。
         名前:
         ```    
         寒い日の過ごし方
         ```  

         エージェント・メッセージ:
         ```    
         今日は肌寒いので、家で過ごすのがおすすめです。映画鑑賞などはいかがですか？
         ```   

         ![alt text](flow-userActivity_images/image.png)
     
     2. 次に **メッセージ2** をクリックし、編集アイコンをクリックし、次の値を設定します。  
         名前:
         ```    
         暖かい日の過ごし方
         ```  

         エージェント・メッセージ:
         ```    
         今日は暖かいので外出がおすすめです。お散歩やショッピングはいかがですか？
         ```   

         ![alt text](flow-userActivity_images/image-1.png)

7. 右上の **完了** ボタンをクリックして、フロー・ビルダーを閉じます。  
![alt text](flow-userActivity_images/flow_ua_image0430.png)  

## エージェントへのツール追加とテスト実行
1. エージェント・ビルダーから先ほど作成したXX-IBMInfoを開き、**ツールの追加** ボタンをクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0510.png)  

2. **ローカル・インスタンス** を選択します。  
![alt text](flow-userActivity_images/flow_ua_image0520.png)  

3. 検索ボックスに **XX_weather** と入力し、表示された **XX_weatherFlow** のチェックボックスを有効にして **エージェントに追加** ボタンをクリックします。  
![alt text](flow-userActivity_images/flow_ua_image0530.png)  

4. 前の演習で作成したcurrent weather for coordinatesを削除します。  
![alt text](flow-userActivity_images/flow_ua_image0540.png)  

5. 右側のプレビューチャットで「**今日のおすすめの過ごし方は？**」と入力すると、今日いる都市名を質問されます。その都市の気温に応じて、先ほどフロー内で設定したおすすめの過ごし方が表示されます。  
![alt text](flow-userActivity_images/flow_ua_image0550.png)  

!!!note
    エージェント型ワークフローのデフォルト動作では、フロー完了後のエージェントによる要約出力は抑制されており、フローの出力結果がJSON形式でそのまま表示されます。ユーザーに自然な形でメッセージを表示するには、このハンズオンで使用したように **ユーザーに提示する（メッセージ）ノード** で固定メッセージを表示する方法のほか、**生成プロンプトノード** や **エージェントノード** を使用してLLMによる動的なメッセージ生成を行うことも可能です。  
    なお、ADKを使ってエージェント型ワークフローをコードで定義する場合は、`@flow` デコレーターの `suppress_agent_summarization=False` を指定することで、フロー完了後のエージェントによる要約出力は抑制を解除（エージェントが結果を要約して自然な文章で出力する）できます。詳細は[**こちら**](https://developer.watson-orchestrate.ibm.com/tools/flows/building_flow#param-suppress-agent-summarization){:target="_blank"}をご参照ください。

## お疲れさまでした！
このハンズオンでは、フロー・ビルダーの使い方について説明しました。

!!!tip "さらに複雑なフローを構築するために"
    今回のハンズオンではツール・ブランチ・メッセージといった基本的なノードを中心に紹介しましたが、エージェント型ワークフローにはほかにも多彩なノードが用意されています。

    - **[Decisionsノード](https://developer.watson-orchestrate.ibm.com/tools/flows/decisions_node){:target="_blank"}**: 条件テーブルを使った複雑なビジネスルールをフロー内に組み込めます。複数のルールを上から順に評価し、最初に一致した結果を返します。
    - **[Doc Processingノード](https://developer.watson-orchestrate.ibm.com/tools/flows/document_processing_nodes){:target="_blank"}**: PDF や画像などのドキュメントからテキストやキーバリューペアを抽出し、そのデータをフローに取り込むことができます。
    - **[Promptノード](https://developer.watson-orchestrate.ibm.com/tools/flows/overview){:target="_blank"}**: LLM を呼び出して、入力データに基づく情報の抽出・分類・生成を行い、動的な処理を実現できます。
    - **[Agentノード](https://developer.watson-orchestrate.ibm.com/tools/flows/overview){:target="_blank"}**: 別のエージェントをサブタスクとして呼び出し、マルチエージェント構成のワークフローを構築できます。
    - **[Foreach / Loopノード](https://developer.watson-orchestrate.ibm.com/tools/flows/foreach_node){:target="_blank"}**: リストの各要素に対して処理を繰り返したり、条件が満たされるまでループ処理を行うことができます。

    利用可能なノードタイプの全一覧は[**こちら**](https://developer.watson-orchestrate.ibm.com/tools/flows/overview){:target="_blank"}をご参照ください。これらのノードを組み合わせることで、単純なツール連携にとどまらない、より高度な自動化シナリオを実現できます。