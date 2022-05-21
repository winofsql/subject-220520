## Drive API

- ### [共有ドライブ一覧](https://developers.google.com/drive/api/v2/reference/drives/list)
  - ( maxResults に 100 を入力して実行します )
  - [メンバ一覧](https://developers.google.com/drive/api/v2/reference/permissions/list)
    - fileId に 取得した共有ドライブの ID を入力
    - maxResults には 100
    - supportsAllDrives を true
    - 🏃 実行

✅ ID は編集してあります
```json
{
  "kind": "drive#driveList",
  "items": [
    {
      "id": "0ANJvQi0TrWp5X123456",
      "name": "重要な情報",
      "kind": "drive#drive"
    },
    {
      "id": "0AMOvvbCoMzzSX123456",
      "name": "SE-WORK-DOWNLOAD",
      "kind": "drive#drive"
    },
    {
      "id": "0ANbnzy6oXOAkX123456",
      "name": "SE-WORK",
      "kind": "drive#drive"
    }
  ]
}
```
✅ ユニーク情報は空文字にしています
```json
{
  "kind": "drive#permissionList",
  "etag": "",
  "selfLink": "",
  "items": [
    {
      "id": "",
      "name": "【重要なシステム情報】",
    　"type": "group",  
    　"role": "fileOrganizer",  
      "kind": "drive#permission",
      "withLink": false,
      "selfLink": "",
    　"emailAddress": "",  
      "domain": "",
      "etag": "",
      "permissionDetails": [
        {
          "permissionType": "member",
          "role": "fileOrganizer",
          "inherited": false
        }
      ],
      "teamDrivePermissionDetails": [
        {
          "teamDrivePermissionType": "member",
          "role": "fileOrganizer",
          "inherited": false
        }
      ],
      "deleted": false
    }
  ]
}
```


- ### [WEB参考記事](https://qiita.com/ryosuk/items/8fdcd606d94e89e156ed)
```javascript


var SheetName = "getDriveUser";
activeSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SheetName)

function getDriveUser() {

  firstStep();
  adminListAllTeamDrives();

}

function firstStep(sheetName) {

  //初期化する 
  activeSheet.clear();

  activeSheet.getRange(1, 1).setValue("ドライブ名")
  activeSheet.getRange(1, 1).setBackground("#7169e5");
  activeSheet.getRange(1, 1).setFontColor("#ffffff");

}

function adminListAllTeamDrives() {
  //変数の宣言
  var pageTokenDrive;
  var pageTokenMember;
  var teamDrives;
  var permissions;

  //ドライブ名の一覧を取得
  do {
    teamDrives = Drive.Drives.list({ pageToken: pageTokenDrive, maxResults: 100 })
    if (teamDrives.items && teamDrives.items.length > 0) {

      for (var i = 0; i < teamDrives.items.length; i++) {

        var teamDrive = teamDrives.items[i];
        //ドライブ名の一覧情報を転記
        activeSheet.getRange(i + 2, 1).setValue(teamDrive.name)

        //ドライブごとのメンバーの権限を取得
        do {
          permissions = Drive.Permissions.list(teamDrive.id, { maxResults: 100, pageToken: pageTokenMember, supportsAllDrives: true });
          if (permissions.items && permissions.items.length > 0) {
            for (var j = 0, k = 2; j < permissions.items.length; j++, k = k + 2) {

              activeSheet.getRange(1, k).setValue("メンバー")
              activeSheet.getRange(1, k + 1).setValue("権限")
              activeSheet.getRange(1, k, 1, k).setBackground("#7169e5");
              activeSheet.getRange(1, k, 1, k).setFontColor("#ffffff");


              //権限情報を取得して変数に格納
              var permission = permissions.items[j];
              activeSheet.getRange(i + 2, k).setValue(permission.emailAddress)

              switch (permission.role) {
                case "organizer":
                  activeSheet.getRange(i + 2, k + 1).setValue("管理者")
                  break;
                case "fileOrganizer":
                  activeSheet.getRange(i + 2, k + 1).setValue("コンテンツ管理者")
                  break;
                case "writer":
                  activeSheet.getRange(i + 2, k + 1).setValue("投稿者")
                  break;
                case "commenter":
                  activeSheet.getRange(i + 2, k + 1).setValue("閲覧者(コメント可)")
                  break;
                case "reader":
                  activeSheet.getRange(i + 2, k + 1).setValue("閲覧者")
                  break;
              }
            }
          } else {
            Logger.log("メンバー/権限が見つかりませんでした。");
          }

          //次のページのpageTokenを取得する
          pageTokenMember = permissions.nextPageTokens
        } while (pageTokenMember)
      }

    } else {
      Logger.log("共有ドライブが見つかりませんでした。");
    }

    //次のページのpageTokenを取得する
    pageTokenDrive = teamDrives.nextPageToken
  } while (pageTokenDrive)
}

```

![image](https://user-images.githubusercontent.com/1501327/169460213-0f29158a-3f67-44c5-9407-c2d0a6985de7.png)

