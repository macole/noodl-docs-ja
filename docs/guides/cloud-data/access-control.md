---
title: アクセス制御 / ACL
hide_title: true
---

# アクセス制御 / ACL

## このガイドで学べること

この時点で、データベース内のレコードの作成、更新、クエリ方法について十分な理解を得ているはずです。スキルを新鮮に保つ必要がある場合は、以下のガイドを確認してください。

- [バックエンドの作成](/docs/guides/cloud-data/creating-a-backend)
- [クラスの作成](/docs/guides/cloud-data/creating-a-class)
- [レコードの作成](/docs/guides/cloud-data/creating-new-database-records)
- [レコード関係](/docs/guides/cloud-data/record-relations)

デフォルトでは、作成したすべてのレコードは完全に公開されており、ログインしているユーザーでも匿名ユーザーでも読み書きできます。これはアプリケーションの開発中は問題ありませんが、チームの外にリリースする準備ができたら、アクセス制御について考える必要があります。つまり、誰がどのレコードを読み書きできるかです。幸いにも、Noodlでこれを実現するためのかなり確かな方法があります。

このガイドでは、作成したレコードへのアクセス制御を特定のユーザーに限定する方法を学びます。

## クラスレベルの権限

クラウドデータへのアクセス制御を指定できるレベルには2つあります。**クラスレベルの権限**は特定のクラスのすべてのレコードに共通するものであり、**アクセス制御ルール**は特定のレコードのみに適用されます。まず前者から始めましょう。クラウドサービスダッシュボードを通じて特定のクラスの**クラスレベルの権限**にアクセスできます。

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/clp-1.png)

</div>

これにより、次のようなポップアップが表示されます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-2.png)

</div>

ここでは、特定のクラスの**読み取り**、**書き込み**、**フィールドの追加**（クラスのプロパティを変更する可能性）権限を制御できます。次の対象に対して有効/無効を切り替えることができます：

* **Public** これは、サインインしていないアプリケーションのユーザーを指します。つまり、誰でも公開で、このクラスのレコードに対してこれらのアクションを実行できることを意味します。デフォルトでは、読み取り、書き込み、フィールドの変更が完全に公開されています。

* **Authenticated** これは、アプリケーションにサインアップしてログインしているユーザーを指します。これも、このクラスのすべてのレコードに適用されます。

以下の例では、誰でもこのクラスのレコードを読むことができます（これにはクエリも含まれます）が、ログインユーザーのみが書き込み（新規作成および変更）でき、誰も（もちろんダッシュボードを使用する場合を除く）このクラスのプロパティを変更できないように設定されています。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-3.png)

</div>

ロールとポインターを使用して、**クラスレベルの権限**でさらに詳細なアクセス制御を指定できます。公開ユーザーと認証済みユーザーのアクセスを指定したように、特定のロールに属するユーザーのアクセスを指定することができます（ロールについては後述）。これには、リストの下部にあるテキスト入力を使用します：

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/clp-4.png)

</div>

`role:あなたのロールの名前`と入力し、そのロールの権限を指定します。このアプローチを使用することで、例えば`admin`というロールを持ち、そのロールに属するユーザーに特別な権限を持たせることができます。以下の例では、このクラスのレコードに公開読み取りアクセス権がありますが、`admin`ロールに属するユーザーのみがレコードを変更できます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-5.png)

</div>

右上の`Simple / Advanced`スイッチを使用して、さらに細かい制御を有効にすることができます。これはいくつかのユースケースに非常に役立

ちます。これで、権限が分割されます：

* `Get` レコードの**Id**を持っている場合、この権限によりユーザーはレコードのプロパティを取得できます。
* `Find` このクラスのレコードが[レコードのクエリ](/nodes/data/cloud-data/query-records)に表示されます。この権限により、このクラスのすべてのレコードが読み取り可能になります。
* `Count` このクラスでカウントを実行できますが、直接すべてのレコードを読むことはできません。
* `Create` 新しいレコードを作成する権限。
* `Update` 既存のレコードを更新する権限。
* `Delete` 既存のレコードを削除する権限。
* `Add Field` このクラスのプロパティを変更する権限、プロパティ全体を追加/削除します。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-6.png)

</div>

ポインターを使用して、さらに詳細なアクセス制御を指定する方法もあります。たとえば、ブログ投稿を含む`Post`クラスのレコードクラスがあるとします。誰もがレコードを読むことができるようにしたいですが、`Post`レコードの作成者のみがそれを変更できるようにしたいとします。`User`レコードへのポインターを`Post`クラスに追加すると（`User`へのポインターでなければなりません）、ポインターが指すユーザーに対して権限を指定できます。`Owner`というポインターをクラスに設定し、投稿を作成するときに現在のユーザーに設定すると、次のようなアクセス制御を持つことができます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-7.png)

</div>

**クラスレベルの権限**を使用すると、ある程度のレベルまでアクセス制御を指定できますが、すべての権限はクラスレベルであり、つまりクラスのすべてのレコードに均等に適用されます。**アクセス制御ルール**を使用すると、さらに詳細な権限を提供できます。以下で詳しく説明します。

## アクセス制御ルール

レコードのアクセス制御は、**Create New Record**ノードで作成される際に設定することも、後で**Set Record Properties**ノードを使用して更新することもできます。これはプロパティパネルのアクセス制御ルール部分を使用して行われます。以下に例を示します。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-1.png)

</div>

しかし、新しい**Create New Record**ノードを配置し、プロパティを表示すると、ルールは空になります。**(+)**アイコンをクリックして新しいルールを作成できます。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-empty.png)

![](/docs/guides/cloud-data/access-control/acl-first.png)

</div>

必要な数だけアクセス制御ルールを持つことができ、各ルールには選択する必要がある特定の**ターゲット**があります：

- **User**（デフォルト）これは、このルールが特定のユーザーを対象とすることを示します。**User**ターゲットを選択した場合にのみ利用可能なルールの**User Id**入力に接続してユーザーを提供することができます。または、ユーザーIDを明示的に提供しない場合は、現在ログインしているユーザーが使用されます。
- **Everyone** これは、ルールがログインしているユーザーでも匿名ユーザーでもすべてのユーザーを対象とすることを意味します。これは、公開だが読み取り専用のレコードを作成するために使用できます。
- **Role** このレコードをユーザーグループにアクセス可能にしたい場合は、このターゲットを使用する必要があります。ロールについては以下で詳しく見ていきます。

まず、レコードを作成するユーザーがレコードの読み書き権限を持つことを確認する一般的なルールを見てみましょう。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-creator.png)

</div>

これは単にルールのデフォルト設定です。ルールのラベルを編集して短い説明的な名前を付け、後でそれが何を達成することを意図しているかを知ることが推奨されます。このルールを使用してレコードを作成すると、現在のユーザーのみがそれらにアクセスできます。これには、ユーザーが[ログイン](/nodes/data/user/log

-in/)している必要があります。

今度は、誰もがレコードを読むことができるが、変更はできないようにしたいとしましょう。その場合は、**Everyone**ターゲットを持つルールを追加します。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-public-read.png)

</div>

このパターンを使用して、レコードにアクセスできる人を制御する方法は多くあります。たとえば、**Message**レコードを作成して、現在のユーザー（送信者）がメッセージを読み書きでき、受信者がそれを読むことができるようにしたいとします。非常に似たセットのルールを使用しますが、2つの**User**ターゲットを使用します。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-msg-1.png)

</div>

しかし、受信者の**User Id**も提供する必要があります（送信者は現在ログインしているユーザーを使用します）。これは、新しい**User**ターゲットルールを追加すると作成される**User Id**入力への接続を介して行われます。

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/acl-msg-2.png)

</div>

## ロール

これは素晴らしいですが、時には多くのユーザーによってアクセス可能なレコードを持ちたい場合があり、これらのユーザーが時間とともに変更される場合、すべてのレコードをそれに応じて更新するのは面倒です。ここでロールが登場します。ロールの本質は、単にユーザーのリストです（これはロールの**users**という関係プロパティを介して確立されます）。**Add Record Relation**ノードと**Remove Record Relation**ノードを使用して、ロールからユーザーを追加および削除できます。ロールはクラウドサービスダッシュボードを介して追加できます。

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/acl-role-1.png)

![](/docs/guides/cloud-data/access-control/acl-role-2.png)

</div>

ロールには**名前**を提供する必要があります（これはすべてのロールの中で一意である必要があります）、また、**ACL**（アクセス制御）を指定し、一般的にはダッシュボードで作成するロールに対して**Master Key Only**に限定する必要があります。ロールが作成されると、ダッシュボード内の**User**関係を直接使用してユーザーを追加できます。

ロールがあり、それに割り当てられたユーザーがいる場合、単純に**Role**アクセスルールを作成できます：

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-role-3.png)

</div>

**Role**ターゲットを選択し、説明的なラベルを付けてアクセス権を選択します。次に、**Role Name**を指定する必要があります。これは、作成時にロールに与えた一意の名前です。これは大文字と小文字を区別します。

ほとんどの場合、特定のレコードにアクセスする必要があるユーザーチームを作成するなど、ロールを動的に作成して管理したいと思うでしょう。その場合、そのチームのロールを作成し、すべてのチームメンバーに関係を追加します。

これは、**Create New Record**ノードを使用して他のレコードと同様に新しいロールを作成することで達成されます。**Role**クラスを選択します。**Role**のアクセス制御を制限しないと、作成に成功しません。ここでは、作成者、つまり現在のユーザーに完全なアクセス権を与え、さらに"チーム内の全員"にアクセス権を与えます。これは、以下に示すように接続を介して提供するロール名、つまり作成時にロールに与える同じ名前を介して行われます。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-create-role-1.png)

</div>

新しいロールのための一意の**名前**を提供する必要があります。以下の例では、これは単に**Unique Id**ノードで行われます。これは、ロールの**name**入力と上述の**Everyoone in Team**アクセスルールの両方に提供されます。

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/acl-create-role-2.png)

</div>

新しい**Role**が作成されたら、**Add Record Relation**ノードを使用して関係を追加することにより、現在ログインしているユーザーをロールに追加します。現在のユーザーは、上記の**The creator**ルールを介してすでにロールに対する読み書きアクセス権を持っていますが、チームメンバーとしてリストされるべきであ

るため、ユーザーをロールに追加します。これが**Add Record Relation**ノードの設定方法です。

<div className="ndl-image-with-background m">

![](/docs/guides/cloud-data/access-control/acl-create-role-3.png)

</div>

**Id**は新しく作成されたロールから来ます（それは関係を追加したいレコードです）、クラスは**Role**に設定され、追加したい関係は**users**です。

最後に、**Target Record Id**はロールに追加したいユーザーであり、上のノードグラフで見ることができるように、現在ログインしているユーザーに関する情報を含む**User**ノードから取得します。
---
title: アクセス制御 / ACL
hide_title: true
---

# アクセス制御 / ACL

## このガイドで学べること

これまでに、データベース内のレコードの作成、更新、クエリの方法について十分な理解を得ているはずです。スキルを新鮮に保つ必要がある場合は、以下のガイドを確認してください。

- [バックエンドの作成](/docs/guides/cloud-data/creating-a-backend)
- [クラスの作成](/docs/guides/cloud-data/creating-a-class)
- [レコードの作成](/docs/guides/cloud-data/creating-new-database-records)
- [レコード関係](/docs/guides/cloud-data/record-relations)

デフォルトでは、作成したすべてのレコードは完全に公開されており、ログインしているユーザーでも匿名ユーザーでも読み書きできます。これはアプリケーションの開発中は問題ありませんが、チームの外にリリースする準備ができたら、アクセス制御について考える必要があります。つまり、誰がどのレコードを読み書きできるかです。幸いにも、Noodlでこれを実現するかなり確かな方法があります。

このガイドでは、作成したレコードへのアクセス制御を特定のユーザーに限定する方法を学びます。

## クラスレベルの権限

クラウドデータへのアクセス制御を指定できるレベルには2つあります。**クラスレベルの権限**は特定のクラスのすべてのレコードに共通するものであり、**アクセス制御ルール**は特定のレコードのみに適用されます。まず前者から始めましょう。クラウドサービスダッシュボードを通じて特定のクラスの**クラスレベルの権限**にアクセスできます。

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/clp-1.png)

</div>

これにより、次のようなポップアップが表示されます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-2.png)

</div>

ここでは、特定のクラスの**読み取り**、**書き込み**、**フィールドの追加**（クラスのプロパティを変更する機能）権限を制御できます。次の対象に対して有効/無効を切り替えることができます：

* **Public** これは、サインインしていないアプリケーションのユーザーを指します。つまり、誰でも公開で、このクラスのレコードに対してこれらのアクションを実行できることを意味します。デフォルトでは、読み取り、書き込み、フィールドの変更が完全に公開されています。

* **Authenticated** これは、アプリケーションにサインアップしてログインしているユーザーを指します。これも、このクラスのすべてのレコードに適用されます。

以下の例では、誰でもこのクラスのレコードを読むことができます（これにはクエリも含まれます）が、ログインユーザーのみが書き込み（新規作成および変更）でき、誰も（もちろんダッシュボードを使用する場合を除く）このクラスのプロパティを変更できないように設定されています。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-3.png)

</div>

ロールとポインターを使用して、**クラスレベルの権限**でさらに詳細なアクセス制御を指定できます。公開ユーザーと認証済みユーザーのアクセスを指定したように、特定のロールに属するユーザーのアクセスを指定することができます（ロールについては後述）。これには、リストの下部にあるテキスト入力を使用します：

<div className="ndl-image-with-background l">

![](/docs/guides/cloud-data/access-control/clp-4.png)

</div>

`role:あなたのロールの名前`と入力し、そのロールの権限を指定します。このアプローチを使用することで、例えば`admin`というロールを持ち、そのロールに属するユーザーに特別な権限を持たせることができます。以下の例では、このクラスのレコードに公開読み取りアクセス権がありますが、`admin`ロールに属するユーザーのみがレコードを変更できます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-5.png)

</div>

右上の`Simple / Advanced`スイッチを使用して、さらに細かい制御を有効にすることができます。これはいくつかのユースケースに非常に役立ち

ます。これで、権限が分割されます：

* `Get` レコードの**Id**を持っている場合、この権限によりユーザーはレコードのプロパティを取得できます。
* `Find` このクラスのレコードが[レコードのクエリ](/nodes/data/cloud-data/query-records)に表示されます。この権限により、このクラスのすべてのレコードが読み取り可能になります。
* `Count` このクラスでカウントを実行できますが、直接すべてのレコードを読むことはできません。
* `Create` 新しいレコードを作成する権限。
* `Update` 既存のレコードを更新する権限。
* `Delete` 既存のレコードを削除する権限。
* `Add Field` このクラスのプロパティを変更する権限、プロパティ全体を追加/削除します。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-6.png)

</div>

ポインターを使用して、さらに詳細なアクセス制御を指定する方法もあります。たとえば、ブログ投稿を含む`Post`クラスのレコードクラスがあるとします。誰もがレコードを読むことができるようにしたいですが、`Post`レコードの作成者のみがそれを変更できるようにしたいとします。`User`レコードへのポインターを`Post`クラスに追加すると（`User`へのポインターでなければなりません）、ポインターが指すユーザーに対して権限を指定できます。`Owner`というポインターをクラスに設定し、投稿を作成するときに現在のユーザーに設定すると、次のようなアクセス制御を持つことができます。

<div className="ndl-image-with-background xl">

![](/docs/guides/cloud-data/access-control/clp-7.png)

</div>

**クラスレベルの権限**を使用すると、ある程度のレベルまでアクセス制御を指定できますが、すべての権限はクラスレベルであり、つまりクラスのすべてのレコードに均等に適用されます。**アクセス制御ルール**を使用すると、さらに詳細な権限を提供できます。以下で詳しく説明します。