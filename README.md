# 概要
- Gemini APIを使用して当該施設の最寄り駅や、高速道路、電車経路などを取得
- Spreadsheetへの書き出しが出来ないかをチェック
- 実務での応用可能性を探るための実験的スクリプト

# 関連リンク
- [Geminiの書き出し先・Google Spreadsheet](https://docs.google.com/spreadsheets/d/1GgazOc_oCEfbLaIj254TpGJrKBNxQc_0IpLC25L78rs/edit?gid=0#gid=0)
- [Gemini 2.0 Python SDKのアップグレード方法](https://ai.google.dev/gemini-api/docs/migrate?hl=ja&_gl=1*8mvw7x*_up*MQ..*_ga*OTk1MTkzNzcwLjE3Mzk4NzgxOTQ.*_ga_P1DBVKWT6V*MTczOTg3ODE5My4xLjAuMTczOTg3ODE5My4wLjAuMTcwNjc0NzQyOQ..)

# 補足
- 2024年7月時点では、地名に東京と付く場所の場合、住所が東京都～と出力したり、緯度経度の精度がイマイチだった。
- ド田舎でWEB上の情報が少ない場合も住所などの出力に誤りが散見された。　

# Gemini 2.0へのアップグレードで緯度経度の出力の精度が向上？
- [東京ディズニーランドのGoogle MapsのURL](https://www.google.com/maps/search/%E6%9D%B1%E4%BA%AC%E3%83%87%E3%82%A3%E3%82%BA%E3%83%8B%E3%83%BC%E3%83%A9%E3%83%B3%E3%83%89/@35.632897,139.880387,15z?hl=ja)
- 実務にも応用できる精度になったかもしれない

