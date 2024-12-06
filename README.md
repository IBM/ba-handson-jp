# ba-handson-jp
Business Automation 製品のハンズオンです。

Business Automation製品のハンズオンのためのリポジトリです。
[MkDocs](https://www.mkdocs.org/)と[material for MkDocs](https://squidfunk.github.io/mkdocs-material/)を使用しています。  
ビルド、デプロイを行うためには上記ツールをインストールすることが必要です。編集だけならVSCodeがあれば十分です。  
生成されたサイトは[https://pages.github.ibm.com/ba-techsales-jp/wxo_handson_jp](https://pages.github.ibm.com/ba-techsales-jp/wxo_handson_jp)にデプロイされます。


## 環境構築

### Gitのインストール
お使いの環境に応じたGitをのインストールが必要です。

### MkDocsのインストール
 1. [こちら](https://www.mkdocs.org/user-guide/installation/)を参照してMkDocsをインストールします。Pythonのインストール後、以下のコマンドでインストール可能です。  
 ```
 pip install --upgrade pip
 python get-pip.py
 pip install mkdocs
 ```
 2. 以下のコマンドでインストールの確認が可能です。
 ```
 $ mkdocs --version
 mkdocs, version 1.2.0 from /usr/local/lib/python3.8/site-packages/mkdocs (Python 3.8)
 ```

### Material for MkDocsのインストール
以下のコマンドでMaterial for MkDocsをインストールします。また、クリック時に画像を拡大するプラグインであるglightboxについてもインストールを行います。
```
pip install mkdocs-material
pip install mkdocs-glightbox
```

## ビルド
```
mkdocs build
```

## github pagesへのデプロイ
以下のコマンドで、直接git hub pageへデプロイ可能です。gh-deployブランチにPushされたものが公開されます。  
```
mkdocs gh-deploy
```

## Markdownファイルの編集について
ファイルの編集にはVisualCode Studioの利用が便利です。Ctrl+Shift+vでプレビューを表示したり、画像をコピー＆ペーストで貼り付けることも可能です。  
画像を貼り付けると、デフォルトではファイルと同一のフォルダに画像ファイルが生成され、管理が難しくなるため、*Markdownのファイル名_images*フォルダへの格納を行います。  
Visula Code Studioの設定で指定することが可能です。ファイル＞ユーザー設定＞設定＞Markdown　から　を探し、項目の追加ボタンをクリックし、以下の項目と値を追加してください。  
 - 項目: *
 - 値: `${documentDirName}/${documentBaseName}_images/`

![alt text](README_images/image.png)

## その他
 - 文字列の後に改行したい場合には文末にスペースを2つ入力してください。
 - 特殊文字をエスケープする際にはバックスラッシュを使用してください。
 - github.ibm.comではなく、github.comで公開するには以下の手順を踏んでください。
```  
1. git clone https://github.ibm.com/ba-techsales-jp/wxo_handson_jp.git リポジトリ名

2. github.comで新しいリポジトリを作る

3. git remote set-url origin 新しいリポジトリのURL

4. git push origin main
```

