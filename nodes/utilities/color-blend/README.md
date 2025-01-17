---
hide_title: true
hide_table_of_contents: true
title: Color Blendノード
---

{/*##head##*/}

# Color Blendノード

このノードは、色の値をブレンドすることができます。

<div className="ndl-image-with-background">

![](/nodes/utilities/color-blend/color-blend.png)

</div>

{/*##head##*/}

## 入力

| データ                                          | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <span className="ndl-data">Color 0..N</span>  | ミックスする色です。これらのポートは_Color 0_、_Color 1_などと番号付けされます。色が指定されると、次の色のための新しい入力が作成されます。                                                                                                                                                                                                                                                                                                             |
| <span className="ndl-data">ブレンド値</span> | 入力色がどのようにブレンドされるかを指定します。入力色は線形に補間されるので、_ブレンド値_が_0_の場合は入力ポート_Color 0_の色になり、値が_1_の場合は_Color 1_の色になります。<br/><br/>値が0.5の場合は_Color 0_と_Color 1_の50%のミックスになり、1.5の場合は_Color 1_と_Color 2_の間のミックスになります。0未満または入力色の数を超える値は、最も近い色にクランプされます。 |

## 出力

| データ                                     | 説明                 |
| ---------------------------------------- | --------------------------- |
| <span className="ndl-data">結果</span> | 結果として得られるブレンド色 |