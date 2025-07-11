# flowを定義して呼び出してみよう！

AIAgentはエージェンティックな振る舞いによって様々な処理を呼び出しますが、事前にflowを定義してその振る舞いを完全に制御したいケースもあります。
このLabでは、Flow Builderを用いてflowを定義し、Agentから呼び出す方法について確認します。

## シンプルなflowの作成と呼び出し
1. 左上のメニューからBuildを選択し、Agent Builderを選択します。  
![alt text](flow_images/flow_image0010.png)


2. 左側のAll toolsを選択し、Create toolをクリックします。  
![alt text](flow_images/flow_image0020.png)


3. Create a new flowをクリックします。  
![alt text](flow_images/flow_image0030.png)


4. 左上のUntitled横の編集ボタンをクリックします。  
![alt text](flow_images/flow_image0040.png)

    1. 次の値を設定します。
        - Name: XX_weatherFlow (XXにはイニシャルを設定してください。)
        - Description: get weather of specific city
    2. Add Inputボタンを押して次の値を設定してください。
        - Name: city_name
        - Type: string	
        - Description: name of the city
        - Required: on
    3. Add Outputボタンを押して次の値を設定してください。
        - Name: temp
        - Type: string	
        - Description: city temperature
    4. Save changesをクリックします。  
    ![alt text](flow_images/flow_image0050.png)


5. 画面左上のToolsタブを選択し、検索ボックスにweatherを入力します。表示されたcurrent weather for coordinatesをStartとEndの間にドラッグ&ドロップします.  
![alt text](flow_images/flow_image0060.png)  
![alt text](flow_images/flow_image0070.png)


## データマッピング
1. 配置したcurrent weather for coordinatesをクリックし、表示されたEdit data mappinngをクリックします。  
![alt text](flow_images/flow_image0080.png)

2. current_weatherはAuto-mapではなくtrueを設定するために、Auto-mapの×をクリックして削除します。  
![alt text](flow_images/flow_image0090.png)  
![alt text](flow_images/flow_image0100.png)  

3. さらにEnter a valueをクリックし、On-Offのスライドボタンを表示させてON(true)に切り替えます。  
![alt text](flow_images/flow_image0110.png)  

4. 右上の×をクリックしてマッピング画面を閉じます。

5. 右上のDoneボタンをクリックしてflow builderを閉じます。  
![alt text](flow_images/flow_image0120.png)  

## AgentへのTool追加とテスト実行
1. Agent Builderから先ほど作成したXX-IBMInfoを開き、Add toolボタンをクリックします。  
![alt text](flow_images/flow_image0130.png)

2. Add form local instanceを選択します。  
![alt text](flow_images/flow_image0140.png)  

3. 検索ボックスにXX_weatherと入力し、表示されたXX_weatherFlowのチェックボックスを有効にしてAdd to agentボタンをクリックします。  
![alt text](flow_images/flow_image0150.png)  

4. 前の演習で作成したcurrent weather for coordinatesをRemoveします。  
![alt text](flow_images/flow_image0160.png)  

5. 右側のPreviewで「大阪の気温は？」と尋ねると、フローが呼び出されます。処理は非同期に実施され、応答を待っている間は追加の入力ができません。  
![alt text](flow_images/flow_image0170.png)  


## Python Codeblockの定義
XX_weatherFlowに気温を摂氏から華氏表記に変更するコードブロックを追加します。  

1. Agent BuilderでXX-IBMInfoを開いた状態で、XX_weatherFlowの縦三点リーダーをクリックして表示されたメニューからOpen in flow builderをクリックします。  
![alt text](flow_images/flow_image0180.png)

2. current weather for coordinatesとEndの間の矢印にカーソルを合わせると＋マークが表示されます。それをクリックし、表示されたメニューからCode Blockを選択します。  
![alt text](flow_images/flow_image0190.png)  

3. 配置したCode blockをクリックし、表示されたOpen code editorをクリックします。  
![alt text](flow_images/flow_image0200.png)  

4. Outputsタブを選択し、Add outputボタンをクリックします。  
![alt text](flow_images/flow_image0211.png)  

5. 次の値を設定後、Saveボタンをクリックします。  
    - Name: Fahrenheit  
    - Type: Integer  
    - Description: temperature in degrees Fahrenheit  
![alt text](flow_images/flow_image0221.png)  
![alt text](flow_images/flow_image0222.png)  

6. 摂氏を華氏に変換するコードをCode editorに入力します。Code editorタブを選択し、次のコードを設定後に右上の×をクリックして閉じます。    
```
self.output.Fahrenheit = (flow["current weather for coordinates"].output.current_weather.temperature*9/5)+32
```  
![alt text](flow_images/flow_image0232.png)   



## 分岐の定義
東京が指定された場合は摂氏で表示するように分岐を設定します。

1. current weather for coordinatesとCode blockの間の矢印にカーソルを合わせて、表示された＋マークをクリックし、表示されたメニューからBranchを選択します。  
![alt text](flow_images/flow_image0250.png)

2. Code blockをドラッグ&ドロップで左へ移動します。  
![alt text](flow_images/flow_image0260.png)  

3. Branchにカーソルを合わせると⚫️が表示されます。  
![alt text](flow_images/flow_image0270.png)  

4. それをEndの⚫️にドラッグ&ドロップして矢印を追加します。  
![alt text](flow_images/flow_image0280.png)  
![alt text](flow_images/flow_image0290.png)  

5. Branchをクリックして編集画面を表示させます。  
![alt text](flow_images/flow_image0300.png)  

6. Expressionの入力欄をクリックし、キーボードからctrl+spaceを入力するとコードアシストが表示されます。flow.input.city_nameを選択します。  
![alt text](flow_images/flow_image0310.png)

7. Case 2のValueとしてTokyoを設定します。  
![alt text](flow_images/flow_image0320.png)

8. 右上のDoneボタンをクリックしてflow builderを閉じます。

9. IBMInfoのAgent Builderの画面に戻るので、右側のPreviewで再度「大阪の気温は？」と尋ねると華氏で表示され、「東京の気温は？」と尋ねると摂氏で表示されます。  
![alt text](flow_images/flow_image0330.png)

## お疲れさまでした！
このハンズオンでは、flow builderの使い方について説明しました。