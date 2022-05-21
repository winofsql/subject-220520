## Drive API

- ### [共有ドライブ一覧](https://developers.google.com/drive/api/v2/reference/drives/list)
  - ( maxResults に 100 を入力して実行します )
  - [メンバ一覧](https://developers.google.com/drive/api/v2/reference/permissions/list)
    - fileId に 取得した共有ドライブの ID を入力
    - maxResults には 100
    - supportsAllDrives を true
    - 🏃 実行

<b>✅ ID は編集してあります</b>
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
<b>✅ ユニーク情報は空文字にしています</b>
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
function sharedDriveList() {

  activeSheet = SpreadsheetApp.getActiveSheet()

  activeSheet.clear();

  activeSheet.getRange(1, 1).setValue("ドライブ名")
  activeSheet.getRange(1, 1).setBackground("#7169e5");
  activeSheet.getRange(1, 1).setFontColor("#ffffff");

  activeSheet.getRange(1, 2).setValue("メンバー")
  activeSheet.getRange(1, 2).setBackground("#7169e5");
  activeSheet.getRange(1, 2).setFontColor("#ffffff");

  //変数の宣言
  var pageTokenDrive;
  var pageTokenMember;
  var teamDrives;
  var permissions;
  var row = 0;  // 行

  //ドライブ名の一覧を取得
    while (pageTokenDrive || row == 0 ) {
    // teamDrives = Drive.Drives.list({pageToken:pageTokenDrive,maxResults:100,useDomainAdminAccess:true})
    teamDrives = Drive.Drives.list({ pageToken: pageTokenDrive, maxResults: 100 })
    if (teamDrives.items && teamDrives.items.length > 0) {

      for (var i = 0; i < teamDrives.items.length; i++) {

        var teamDrive = teamDrives.items[i];
        //ドライブ名の一覧情報を転記
        activeSheet.getRange(row + 2, 1).setValue(teamDrive.name)

        //ドライブごとのメンバーの権限を取得
        while (true) {
          // permissions = Drive.Permissions.list(teamDrive.id, {maxResults:100,pageToken:pageTokenMember,supportsAllDrives:true,useDomainAdminAccess:true}) ;
          permissions = Drive.Permissions.list(teamDrive.id, { maxResults: 100, pageToken: pageTokenMember, supportsAllDrives: true });
          if (permissions.items && permissions.items.length > 0) {
            permissions.items.sort( compare );
            for (var j = 0, k = 2; j < permissions.items.length; j++, k = k + 2) {

              //権限情報を取得して変数に格納
              var permission = permissions.items[j];
              activeSheet.getRange(row + 2, 2).setValue(permission.emailAddress)

              switch (permission.role) {
                case "organizer":
                  activeSheet.getRange(row + 2, 3).setValue("管理者")
                  break;
                case "fileOrganizer":
                  activeSheet.getRange(row + 2, 3).setValue("コンテンツ管理者")
                  break;
                case "writer":
                  activeSheet.getRange(row + 2, 3).setValue("投稿者")
                  break;
                case "commenter":
                  activeSheet.getRange(row + 2, 3).setValue("閲覧者(コメント可)")
                  break;
                case "reader":
                  activeSheet.getRange(row + 2, 3).setValue("閲覧者")
                  break;
              }

              row++;
            }

          } else {
            Logger.log("メンバー/権限が見つかりませんでした。");
          }

          //次のページのpageTokenを取得する
          pageTokenMember = permissions.nextPageTokens
          if ( !pageTokenMember ) {
            break
          }
        }

        row++;
      }

    } else {
      Logger.log("共有ドライブが見つかりませんでした。");
    }

    //次のページのpageTokenを取得する
    pageTokenDrive = teamDrives.nextPageToken
  }
}

// 比較関数
function compare( a, b ){

  if ( a.role == b.role ) {
    return 0;
  }

  if( a.role < b.role ){
     return  -1; 
  }
  else {
     return 1;
  }

}

```

![image](https://user-images.githubusercontent.com/1501327/169460213-0f29158a-3f67-44c5-9407-c2d0a6985de7.png)


