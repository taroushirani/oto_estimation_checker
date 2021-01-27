# Oto Estimation Checker
日本語だと「自動原音設定エラーチェッカー」とか？

moresampler や setParam の原音推定で残念ながらズレた部分を検出するツール（連続音のみ対応）

## 注意

- 連続音音源のみです。
- REAPERのリージョンによる原音切断想定の設計です。（OREMO収録でもある程度いけるとは思います）
- 重複エイリアスが削除されて、モーラ間に空白があると誤検出されてしまいます。全部残した状態で検査してください。

## 使い方

1. oto.ini をexeファイルにドラッグアンドドロップして起動、またはダブルクリックして起動
2. 検査結果が result.txt として出力される。

## 要件

- moresampler や setParam で推定した原音設定ファイル（oto.ini）を読み込み、先行発声位置を点検する。
- 本来あるべき位置から一定以上ずれていた場合に警告を発する。
  - 本来あるべき位置は全エイリアスの中央値をつかうとよさそう。ただし休符を除く。
- 原音設定ファイルは改変しないで手動で直させる。

## 設計

1. moresampler で原音設定した oto.ini を読み取る。
2. 「R」が含まれるエイリアス情報をすべて無視する。単独音や「を」などの複製エイリアスも無視する。
3. 最初の先行発声位置の中央値を求める。
4. 先行発声と先行発声の距離を計算し、中央値を求める。これが1拍あたりの長さ基準になる。
5. 最初の発声開始位置を点検する。あるべき位置に設定されてしているかチェックする。（スレッショルドは要検討）
6. 各wavファイルについて、前から順に1拍ごとになってるか点検する。最初の時刻は基準値ではなく実際の値を用いる。

## 判定の厳しさについて

- 明らかにずれてる原音を検出：基準位置±0.9拍以上のずれ
- 緩く検出：基準位置±0.3拍
- そこそこ検出：基準位置±0.25拍
- 厳しめに検出：基準位置±0.2拍
