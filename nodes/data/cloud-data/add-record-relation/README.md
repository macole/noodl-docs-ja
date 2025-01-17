---
hide_title: true
hide_table_of_contents: true
title: レコード関係ノードを追加
---

{/*##head##*/}

# レコード関係ノードを追加

このアクションノードは、所有レコードと対象レコードの間の関係を作成するために使用されます。

<div className="ndl-image-with-background l">

![](/nodes/data/cloud-data/add-record-relation/add-relation.png)

</div>

1つのレコードが所有レコード（この場合は多くの**Post**レコードに関係を持つことができる**Group**レコード）で、**Relation**タイプのプロパティを持っている必要があります。

所有レコードの<span className="ndl-data">Id</span>を提供する必要があります。また、関係を追加したいレコードの<span className="ndl-data">Id</span>、つまりターゲットレコードIdを提供する必要があります。

最後に、アクションを実行するために<span className="ndl-signal">Do</span>への<span className="ndl-signal">シグナル</span>を送信します。

{/*##head##*/}

## 入力

| データ                                                 | 説明                                                                                                                                                                                             |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <span className="ndl-data">クラス</span>                | 関連オブジェクトを追加したい所有レコードの**クラス**。                                                                                                                                             |
| <span className="ndl-data">Id</span>                    | {/*##input:id##*/}関係を追加する所有レコードとして使用するレコードの**Id**を指定します。{/*##input##*/}この入力は**Id Source**が**明示的に指定**に設定されている場合にのみ有効です。              |
| <span className="ndl-data">関係</span>                  | 関係作成時に使用する所有クラスの**関係**プロパティを選択する必要があります。                                                                                                                     |
| <span className="ndl-data">ターゲットレコードId</span>  | {/*##input:target record id##*/}この入力は新しい関係のターゲットレコードの**Id**に接続されるべきです。{/*##input##*/}                                                                            |

@include "../_id-source.md"

| シグナル                                   | 説明                                                                                                         |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| <span className="ndl-signal">実行</span>   | {/*##input:do##*/}この入力にシグナルが受信されると、関係はバックエンドで作成されます。{/*##input##*/}               |

## 出力

| データ                                           | 説明                                                                                                                                                   |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| <span className="ndl-data">Id</span>              | {/*##output:id##*/}これは新しい関係を受け取る/受け取った所有レコードの**Id**です。単に**Id**入力のミラーです。{/*##output##*/}                          |
| <span className="ndl-data">エラー</span>         | {/*##output:error##*/}バックエンドで関係を追加しようとした際に何か問題が発生した場合のエラーメッセージです。{/*##output##*/}                              |

| シグナル                                         | 説明                                                                                                                                                           |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <span className="ndl-signal">成功</span>         | {/*##output:success##*/}関係がバックエンドで成功裏に追加された場合、この出力にシグナルが送信されます。{/*##output##*/}                                              |
| <span className="ndl-signal">失敗</span>         | {/*##output:failure##*/}バックエンドで関係を追加しようとした際に何か問題が発生した場合、この出力にシグナルが送信されます。**エラー**出力にエラーメッセージが出力されます。{/*##output##*/} |