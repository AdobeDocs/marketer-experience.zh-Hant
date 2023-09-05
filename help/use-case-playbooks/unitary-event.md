---
title: 單一事件
description: 這是模擬「[!UICONTROL 單一事件]」類型歷程驗證的指令頁面。
source-git-commit: 0a52611530513fc036ef56999fde36a32ca0f482
workflow-type: ht
source-wordcount: '749'
ht-degree: 100%

---


# 單一事件

## 需要進行的步驟 {#steps-to-follow}

>[!CONTEXTUALHELP]
>id="marketerexp_sampledata_unitaryevent"
>title="如何使用？"
>abstract="請依照連結了解更多詳情"

>[!IMPORTANT]
>
>**[!UICONTROL 教戰手冊]**&#x200B;這些說明可能會有變動，因此，請隨時參考個別&#x200B;**[!UICONTROL 教戰手冊]**&#x200B;的「範例」資料部份。

## 先決條件

* 您必須安裝 Postman 軟體
* 使用教戰手冊並建立實例資產，例如&#x200B;**[!UICONTROL 歷程]**、**[!UICONTROL 結構描述]**、**[!UICONTROL 區段]**、**[!UICONTROL 訊息]**&#x200B;等。

已建立的資產將顯示在`Bill Of Material`頁面

![物料清單頁面](../assets/bom-page.png)

## 收集必要資產為 Postman 做準備

1. 瀏覽&#x200B;**[!UICONTROL 使用案例教戰手冊]**&#x200B;應用程式。
1. 按一下個別的&#x200B;**[!UICONTROL 教戰手冊]**&#x200B;卡片，即可瀏覽&#x200B;**[!UICONTROL 教戰手冊]**&#x200B;詳細資訊頁面。
1. 瀏覽&#x200B;**[!UICONTROL 物料清單]**&#x200B;頁面，並尋找&#x200B;**[!UICONTROL 範例資料]**&#x200B;部份。
1. 按一下 UI 的個別按鈕，即可下載 `postman.json`。
1. 匯入 `postman.json` (在 **[!DNL Postman Software]** 中)。
1. 為此驗證建立專用的 Postman 環境 (例如 `Adobe <PLAYBOOK_NAME>`)。

## 擷取 IMS 語彙基元

>[!NOTE]
>
>所有環境變數都區分英文大小寫，因此請一定使用正確的變數名稱。

1. 請依照[驗證和存取 Experience Platform API](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html) 文件來生成存取權語彙基元。
1. 將存取權語彙基元儲存在名為 `ACCESS_TOKEN` 的環境變數中。
1. 將其他與身份驗證相關的值儲存在環境變量中，例如 `API_KEY`、`IMS_ORG` 和 `SANDBOX_NAME`。

>[!IMPORTANT]
>
>從 Postman 執行任何 API 之前，請確保必須新增所有必要環境變數。

## 發佈教戰手冊建立的歷程

發佈歷程有 2 種方式；您可以選擇其中一種：

1. **使用 AJO UI**- `Bill Of Material Page`上，按一下歷程連結；這會將您重新導向歷程頁面，您可以在該頁面按一下「**[!UICONTROL 發佈]**」按鈕即可發佈歷程。

   ![歷程對象](../assets/journey-object.png)

1. **使用 Postman API**

   1. 觸發 **[!DNL Publish Journey]** 請求 (來自&#x200B;**[!DNL Journey Publish]**>**[!DNL Queue journey publish job]**)。
   1. 歷程發佈可能需要一些時間，因此若為了檢查狀態，請執行「檢查」歷程發佈狀態 API，直到`response.status``SUCCESS`為止；如果歷程發佈需要時間，請確保等待 10-15 秒。

   >[!NOTE]
   >
   >所有環境變數都區分英文大小寫，因此請一定使用正確的變數名稱。

## 擷取客戶個人資料

>[!TIP]
>
>您可以在電郵件加入 `+<variable>` 來重複使用相同的電子郵件地址，例如`usertest@email.com` 可以重複使用為 `usertest+v1@email.com` 或者 `usertest+24jul@email.com`。這樣可在每次有新的個人資料時，仍然使用相同的電子郵件 ID。

1. 首次使用的使用者需要建立 **[!DNL customer dataset]** 和 **[!DNL HTTP Streaming Inlet Connection]**。
1. 如果您已經建立 **[!DNL customer dataset]** 和 **[!DNL HTTP Streaming Inlet Connection]**，請跳至步驟 `5`。
1. 觸發「**[!DNL Customer Profile Ingestion]** > **[!DNL Create Customer Profile InletId]** > **[!DNL Create Dataset]**」以建立 **[!DNL customer dataset]**；這可將 `CustomerProfile_dataset_id` 儲存在 Postman 環境變數中。
1. 建立 **[!DNL HTTP Streaming Inlet Connection]**，使用 Postman API (在「**[!DNL Customer Profile Ingestion > Create Customer Profile InletId]**」下面)。

   1. `CustomerProfile_dataset_id` 必須在 Postman 環境變數中，如果沒有，請參考步驟 `3`。
   1. 觸發 **[!DNL `CREATE Base Connection`]** 至 [!DNL create base connection]。
   1. 觸發 **[!DNL `CREATE Source Connection`]** 至 [!DNL create source connection]。
   1. 觸發 **[!DNL `CREATE Target Connection`]** 至 [!DNL create target connection]。
   1. 觸發 **[!DNL `CREATE Dataflow`]** 至 [!DNL create dataflow]。
   1. 觸發 **[!DNL `GET Base Connection`]**- 這樣可將 `CustomerProfile_inlet_id` 自動儲存在 Postman 環境變數中。

1. 在這個步驟中，您的 Postman 環境變數中必須有 `CustomerProfile_dataset_id` 和 `CustomerProfile_inlet_id`；如果沒有，請個別參考步驟 `3` 或 `4`。
1. 若要擷取客戶，使用者需要將 `customer_country_code`、`customer_mobile_no`、`customer_first_name`、`customer_last_name` 和 `email` 儲存在 Postman 環境變數中。

   1. `customer_country_code` 將是手機號碼的國家代碼，例如 `91` 或 `1`
   1. `customer_mobile_no` 將是手機號碼，例如 `9987654321`
   1. `customer_first_name` 將是使用者的名字
   1. `customer_last_name` 將是使用者的姓氏
   1. `email` 將是使用者的電子郵件地址，這對於使用不同的電子郵件 ID 至關重要，以便可以擷取新的個人資料。

1. 更新 Postman 請求 **[!DNL Customer Ingestion]**>**[!DNL Customer Streaming Ingestion]**，以變更客戶的首選頻道；[!DNL `email`] 會依預設設定在請求中。

   ```js
   "consents": {
       "marketing": {
           "preferred": "email",
           "email": {
               "val": "y"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "n"
           }
       }
   }
   ```

1. 將首選頻道變更為 `sms` 或 `push`，並將個別頻道值變更為 `y` 和 `n` 至其他值，例如

   ```js
   "consents": {
       "marketing": {
           "preferred": "sms",
           "email": {
               "val": "n"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "y"
           }
       }
   }
   ```

1. 最後，觸發 **[!DNL `Customer Profile Ingestion > Customer Profile Streaming Ingestion`]** 以擷取客戶個人資料。

## 擷取事件

1. 首次使用的使用者需要建立 **[!DNL event dataset]** 和 **[!DNL HTTP Streaming Inlet Connection for events]**。
1. 如果您已經建立 **[!DNL event dataset]** 和 **[!DNL HTTP Streaming Inlet Connection for events]**，請跳至步驟 `5`。
1. 觸發「**[!DNL `Schemas Data Ingestion > AEP Demo Schema Ingestion > Create AEP Demo Schema InletId > Create Dataset`]**」以建立 **[!DNL event dataset]**；這樣可將 `AEPDemoSchema_dataset_id` 儲存在 Postman 環境變數中
1. 建立 **[!DNL HTTP Streaming Inlet Connection for events]**，使用 Postman API (在「**[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL Create AEP Demo Schema InletId]** 下面)。

   1. `AEPDemoSchema_dataset_id` 必須在 Postman 環境變數中，如果沒有，請參考步驟 `3`
   1. 觸發 **[!DNL `CREATE Base Connection`]** 至 [!DNL create base connection]
   1. 觸發 **[!DNL `CREATE Source Connection`]** 至 [!DNL create source connection]
   1. 觸發 **[!DNL `CREATE Target Connection`]** 至 [!DNL create target connection]
   1. 觸發 **[!DNL `CREATE Dataflow`]** 至 [!DNL create dataflow]
   1. 觸發 **[!DNL `GET Base Connection`]**- 這樣可將 `AEPDemoSchema_inlet_id` 自動儲存在 Postman 環境變數中

1. 在這個步驟中，您的 Postman 環境變數中必須有 `AEPDemoSchema_dataset_id` 和 `AEPDemoSchema_inlet_id`；如果沒有，請個別參考步驟 `3` 或 `4`
1. 若要擷取事件，使用者需要變更 Postman 中 **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL AEP Demo Schema Streaming Ingestion]** 請求內文的時間變數 `timestamp`。

   1. `timestamp` 事件發生時，使用目前的時間戳 (例如`2023-07-21T16:37:52+05:30`) 根據您的需要調整時區。

1. 觸發 **[!DNL Schemas Data Ingestion > AEP Demo Schema Ingestion > AEP Demo Schema Streaming Ingestion]** 以擷取事件，這樣便可以觸發歷程

## 最終驗證

您必須在所選的首選頻道上收到一條消息 (在 **[!DNL Ingest the Customer Profile]** 步驟 `8`

* `SMS` - 如果首選頻道是 `sms` (在 `customer_country_code` 和 `customer_mobile_no`)
* `Email` - 如果首選頻道是`email` (在`email`)

或者，你可以查看 `Journey Report`；若要查看，請按一下`Journey Object` (在`Bill of Materials page`)，這樣會將您重新導向至`Journey Details page`。

若是任何已發佈的歷程，使用者必須取得「**[!UICONTROL 查看報告]**」按鈕
![歷程報告頁面](../assets/journey-report-page.png)


## 清理

請不要同步執行多個`Journey`實例；如果僅用於驗證，請在驗證完成後停止歷程。
