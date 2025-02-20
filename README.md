# 概要
- Gemini APIを使用して当該施設の最寄り駅や、高速道路、電車経路などを取得
- Spreadsheetへの書き出しが出来ないかをチェック
- 実務での応用可能性を探るための実験的スクリプト

# 関連リンク
- [Geminiの書き出し先・Google Spreadsheet](https://docs.google.com/spreadsheets/d/1GgazOc_oCEfbLaIj254TpGJrKBNxQc_0IpLC25L78rs/edit?gid=0#gid=0)
- [note Google Colabのチートシート](https://note.com/nepia_infinity/n/n71df2c1c99b2)
- [Gemini 2.0 Python SDKのアップグレード方法](https://ai.google.dev/gemini-api/docs/migrate?hl=ja&_gl=1*8mvw7x*_up*MQ..*_ga*OTk1MTkzNzcwLjE3Mzk4NzgxOTQ.*_ga_P1DBVKWT6V*MTczOTg3ODE5My4xLjAuMTczOTg3ODE5My4wLjAuMTcwNjc0NzQyOQ..)

# 補足
- 2024年7月時点では、地名に東京と付く場所の場合、住所が東京都～と出力したり、緯度経度の精度がイマイチだった。
- 2025年2月時点でも、田舎だと緯度経度、所要時間共に大幅な狂いがあり実用的ではない。
- 東京都内であれば、それなりの精度がある印象

# Gemini 2.0へのアップグレードで緯度経度の出力の精度が向上？
![image](https://github.com/user-attachments/assets/0b3a44b9-6b65-48c3-ba9a-958be6abfd09)

- [東京ディズニーランドのGoogle MapsのURL](https://www.google.com/maps/search/%E6%9D%B1%E4%BA%AC%E3%83%87%E3%82%A3%E3%82%BA%E3%83%8B%E3%83%BC%E3%83%A9%E3%83%B3%E3%83%89/@35.632897,139.880387,15z?hl=ja)



# GASでMap Serviceを使用して緯度経度を求める
- この方法で出力した方が正確な緯度経度を算出できる

``` Javascript
/**
 * @description スプレッドシートの住所データをもとに、緯度・経度・Google Maps URLを取得し、スプレッドシートに書き込む。
 */
function setGeoCode() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheetByName('緯度経度出力');
  const values = sheet.getDataRange().getDisplayValues();
  const headers = values[0];
  // console.log(values);

  const column = {
    'address': headers.indexOf('住所'),
    'latitude': headers.indexOf('緯度'),
    'longitude': headers.indexOf('経度'),
    'googleMaps': headers.indexOf('Google Maps Url',)
  }

  values.forEach((record, index) => {
    const address = record[column.address];
    let row = index + 1;

    if ( address && 0 < index ) {
      console.log(record);

      const obj = getGeoData_(address); // 住所から緯度、経度、Google Maps URLを取得
      console.log(obj); // { latitude: 緯度, longitude: 経度, googleMapsUrl: Google Maps URL }

      sheet.getRange(row, 2, 1, 3).setValues([[obj.latitude, obj.longitude, obj.googleMapsUrl]]);
    }
  });
}



/**
 * 住所から緯度、経度、Google MapsのURLを取得する。
 *
 * @param {string} address 住所
 * @return {object} 緯度、経度、Google MapsのURL
 *
 */
function getGeoData_(address) {
  const maps = Maps.newGeocoder()
  .setLanguage("ja")
  .geocode(address);
  console.log(maps);

  const geoData = maps.results[0].geometry;
  console.log(geoData);

  const latitude  = geoData.location.lat;//緯度
  const longitude = geoData.location.lng;//経度
  const common  = 'https://www.google.com/maps/search/';
  const googleMapsUrl = common + latitude + ',' + longitude;

  console.log(googleMapsUrl);

  return {
    'latitude': latitude,
    'longitude': longitude,
    'googleMapsUrl': googleMapsUrl,
  }
}

```

