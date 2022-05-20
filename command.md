- ### コマンドプロンプトの起動
  - cmd.exe
    - C:\Windows\System32\cmd.exe
    - C:\Windows\System32 にはパスが通っているので cmd で実行可能( システム環境変数 )\
    ![image](https://user-images.githubusercontent.com/1501327/169431083-312362de-e689-4cf7-945a-24fdf08d8953.png)
  - ファイル名を指定して実行
  - エクスプローラのアドレスバーから cmd を入力する
  - タスクマネージャのファイルメニューから新しいタスクの実行\
  ![image](https://user-images.githubusercontent.com/1501327/169431675-438cbe0c-715e-4df1-8476-feaa3e4a0818.png)
  
- ### Excel で多くのフォルダを一気に作成
  - セルに作成したいフォルダの一覧を縦に作成
  - 右隣のセルの一番上に ="mkdir " & A1
  - セルの順番通りに作成したい場合\
  ![image](https://user-images.githubusercontent.com/1501327/169435062-5fb79243-84e0-4c3d-9ffe-3279686e174a.png)

- ### Excel で多くの空のファイルを一気に作成
  - リダイレクトを使ってコマンドを作成\
  ![image](https://user-images.githubusercontent.com/1501327/169435957-e7e97daf-ca06-4f8b-b545-9cbfbce7b1a6.png)

- ### 作成した Excel から CSV を二種類作成
  - shift_jis の csv
    - sjis.csv
  - UTF-8 with BOM の csv
    - utf8.csv
  - 上記 BOM を外した UTF-8 のファイルを作成( VSCode の右下タスクバーでキャラクタセットをクリックして、指定したキャラクタセットで保存 )
    - utf8-normal.csv

- ### BOM 等のバイトレベルの違いを確認する拡張( ms-vscode.hexeditor )
![image](https://user-images.githubusercontent.com/1501327/169437554-84dddcc3-567e-498a-bc16-1b1c22f891c7.png)
 
