---
title: Azure Data Box Disk の返送に関するチュートリアル | Microsoft Docs
description: このチュートリアルでは、Azure Data Box Disk を Microsoft に返送する方法について説明します。
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 03/03/2020
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 1003b862d7fbe19545cf705570866479132c1f03
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "78273170"
---
::: zone target="chromeless"

## <a name="return-azure-data-box-disk"></a>Azure Data Box Disk を返送する 

::: zone-end

::: zone target="docs"

# <a name="tutorial-return-azure-data-box-disk"></a>チュートリアル:Azure Data Box Disk を返送する 

このチュートリアルでは、Azure Data Box Disk を返送するための集荷をスケジュールする方法について説明します。 集荷の手順は、デバイスを返送する場所によって決まります。 

このチュートリアルでは、次の内容を学習します。

> [!div class="checklist"]
> * Data Box Disk を Microsoft に発送する
> * さまざまなリージョンで Data Box Disk を集荷する

## <a name="prerequisites"></a>前提条件

開始する前に、「[チュートリアル:Azure Data Box Disk へのデータのコピーと検証](data-box-disk-deploy-copy-data.md)」を参照してください。


## <a name="ship-data-box-disk-back"></a>Data Box Disk を返送する

::: zone-end

1. データの検証が完了したら、ディスクの接続を解除します。 接続ケーブルを取り外してください。
2. すべてのディスクと接続ケーブルをエアー クッションで包んで梱包箱に詰めます。 アクセサリが不足している場合は、料金がかかることがあります。
    - 最初の配送時に使われていた梱包を再利用してください。  
    - ディスクはエアー クッションでしっかりと包んで梱包することをお勧めします。
    - 箱内の物があまり動かないように、すき間が少なくなるようにしてください。

次の手順は、デバイスを返送する場所によって決まります。 手順は、米国およびカナダ、欧州連合 (EU)、オーストラリア、アジア諸国で異なります。

### <a name="in-us-or-canada"></a>[米国またはカナダ](#tab/in-us-or-canada)

米国またはカナダでデバイスを返送するには、次の手順を実行します。

1. 梱包箱に貼り付けられている透明のビニール袋に入った返送ラベルを使用します。 ラベルを破損または紛失した場合:
    - **[概要] > [出荷ラベルをダウンロード]** に移動して、返送ラベルをダウンロードしてください。
    - デバイスにラベルを貼り付けます。

2. 梱包箱を封印し、返送ラベルが見えることを確認します。
3. UPS で集荷のスケジュールを設定します。 集荷のスケジュールを設定するには:

    - 最寄りの UPS (国/地域固有のフリー ダイヤル) に連絡します。
    - 電話で、印刷ラベルに表示されている返送追跡番号を伝えます。
    - 追跡番号を伝えないと、集荷時に UPS から追加料金が請求されます。
    - 集荷のスケジュールを設定する代わりに、最寄りの持ち込み場所に Data Box Disk を持ち込むこともできます。

### <a name="in-europe"></a>[ヨーロッパ](#tab/in-europe)

ヨーロッパでデバイスを返送するには、次の手順を実行します。

1. 梱包箱に貼り付けられている透明のビニール袋に入った返送ラベルを使用します。 ラベルを破損または紛失した場合:
    - **[概要] > [出荷ラベルをダウンロード]** に移動して、返送ラベルをダウンロードしてください。
    - デバイスにラベルを貼り付けます。

2. 梱包箱を封印し、返送ラベルが見えることを確認します。
3. ヨーロッパで DHL を使ってデバイスを返送する場合は、DHL の Web サイトにアクセスし、航空貨物運送状番号を指定して、DHL に集荷を依頼します。
4. 該当する国/地域の DHL Express の Web サイトにアクセスし、 **[Book a Courier Collection]\(宅配便の予約\) > [eReturn Shipment]\(電子返送\)** の順に選択します。    
3. 貨物運送状番号を指定し、 **[Schedule Pickup]\(集荷のスケジュール\)** をクリックして集荷の手配を行います。

### <a name="in-australia"></a>[オーストラリア](#tab/in-australia)

オーストラリアの Azure データセンターには、追加のセキュリティ通知があります。 すべての国内配送には事前通知が必要です。 オーストラリアで集荷する場合は、次の手順を実行します。

1. 提供された返送ラベルを使用して、それに TAU コード (参照番号) が記載されていることを確認します。 提供された配送先住所ラベルが見つからない場合やその他の問題が発生している場合は、[Data Box アジア オペレーション](mailto:adbo@microsoft.com)までメールでお問い合わせください。 件名ヘッダーの注文名と直面している問題の詳細を記入します。
3. 箱にラベルを貼り付けます。 
4. リンク先 (https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference ) からオンラインで集荷を予約します。 

### <a name="in-japan"></a>[日本](#tab/in-japan)

1. 伝票に送信元の情報としてお客様の会社名と住所の情報を記入します。
2. 次のメール テンプレートを使用して Quantium Solutions にメールを送信します。

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Microsoft Azure Data Box Disk｜Job Name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
        a. 08：00-13：00 
        b. 13：00-15：00 
        c. 15：00-17：00 
        d. 17：00-19：00 
    ```
    - **大阪で集荷する場合**は、電子メール テンプレートの件名を `Pickup request for Microsoft Azure OSA` に変更します。
    - 日本郵便の着払伝票が含まれていなかった場合、または紛失した場合は、このメールにそのことを記載します。 Quantium Solutions Japan が日本郵便に集荷を依頼し、集荷時に伝票を持って行くように伝えます。
    - 複数の注文がある場合は、必ず個別に集荷するようにメールを送信します。

3. 集荷を予約した後、Quantium Solutions からメールの確認を受信します。 確認のメールには、着払伝票に関する情報も含まれています。

必要に応じて、次の情報で Quantium Solutions のサポート (日本語) にお問い合わせください。 

- メール: Customerservice.JP@quantiumsolutions.com 
- 電話: 03-5755-0150 

### <a name="in-korea"></a>[韓国](#tab/in-korea)

1. 必ず返品用の伝票を同封してください。
2. 伝票があるときに集荷を依頼するには:
    1. 営業時間中 (月曜日から金曜日の午前 10 時から午後 5 時) に *Quantium Solutions International* ホットライン (070-8231-1418) に電話をかけます。 "*Microsoft Azure の集荷*" であることとサポート リクエスト番号を伝え、集荷を手配します。  
    2. ホットラインにつながらない場合は、`microsoft@rocketparcel.com` にメールを送信します。メールの件名に「*Microsoft Azure Pickup (Microsoft Azure の集荷)* 」、参照としてサポート リクエスト番号を入力します。
    3. 配送業者が集荷に来ない場合は、*Quantium Solutions International* ホットラインに別の手配を依頼します。 
    4. 集荷スケジュールの確認メールが届きます。
3. 伝票がない場合にのみ、この手順を実行してください。 集荷を依頼するには:
    1. 営業時間中 (月曜日から金曜日の午前 10 時から午後 5 時) に *Quantium Solutions International* ホットライン (070-8231-1418) に電話をかけます。 "*Microsoft Azure の集荷*" であることとサポート リクエスト番号を伝え、集荷を手配します。 集荷を手配するには新しい伝票が必要であることを指定します。 送付元 (お客様)、受取先の情報 (Azure データセンター)、および参照番号 (サービス リクエスト番号) を指定します。 
    2. ホットラインにつながらない場合は、`microsoft@rocketparcel.com` にメールを送信します。メールの件名に「*Microsoft Azure Pickup (Microsoft Azure の集荷)* 」、参照としてサポート リクエスト番号を入力します。
    3. 配送業者が集荷に来ない場合は、*Quantium Solutions International* ホットラインに別の手配を依頼します。 
    4. 電話でのリクエストの場合は、口頭で確認を受け取ります。


### <a name="in-singapore"></a>[シンガポール](#tab/in-singapore)

1. 配送先住所ラベルを印刷し、箱に貼り付けます。 ラベルを破損または紛失した場合:
    - **[概要] > [出荷ラベルをダウンロード]** に移動し、返送ラベルを取得します。
    - デバイスにラベルを貼り付けます。 ラベルが見えることを確認します。

2. 集荷を要求するには、次のテンプレートに追跡番号を記入して SingPost のカスタマー サービスにメールを送信してください (追跡番号は、納品パッケージに同梱されている返送用ラベルに記載されています)。

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pick-up - XZ00001234567 
    Body: 
     a. Requestor name
     b. Requestor contact number
     c. Requestor collection address
     d. Preferred collection date
    ```

   > [!NOTE]
   > 営業日に受信した予約の取り扱い:
   > - 午後 3 時より前に受信した場合、集荷は翌営業日の午前 9 時から午後 1 時の間になります。
   > - 午後 3 時より後に受信した場合、集荷は翌営業日の午後 2 時から午後 6 時の間になります。

   問題が発生した場合は、Data Box Operations Asia (adbo@microsoft.com) にご連絡ください。 ジョブ名を件名ヘッダーに指定し、発生している問題についてお知らせください。

3. 配送業者に渡します。

### <a name="in-self-managed"></a>[自己管理型](#tab/in-selfmanaged)

日本、シンガポール、韓国、西ヨーロッパで Data Box Disk を使用しており、なおかつ注文の作成時に自己管理型の配送オプションを選択した場合は、次の手順に従います。 

1. Azure portal 上で対象の注文の **[概要]** ブレードにアクセスします。 **[集配のスケジュール]** を選択したときに表示される手順に目を通します。 注文の配送時に使用される認証コードが表示されていると思います。

2. デバイスを返送する準備が整ったら、次のテンプレートを使用して Azure Data Box オペレーション チームにメールを送信します。

    ```
    To: adbops@microsoft.com
    Subject: Request for Azure Data Box Disk drop-off for order: ‘orderName’
    Body: 
     a. Order name
     b. Contact name of the person dropping off. You will need to display a Government approved ID during the drop off.
    ```
3. Azure Data Box オペレーション チームがお客様と連絡を取り、Azure Datacenter への配送を手配します。

::: zone target="docs"

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、Azure Data Box Disk に関する次のようなトピックについて説明しました。

> [!div class="checklist"]
> * Data Box Disk を Microsoft に発送する
> * さまざまなリージョンで Data Box Disk を集荷する

次のハウツー記事に進み、Data Box Disk から Azure Storage アカウントへのデータ アップロードを検証する方法について学習してください。

> [!div class="nextstepaction"]
> [Azure Data Box Disk からのデータのアップロードを確認する](./data-box-disk-deploy-picked-up.md)

::: zone-end




