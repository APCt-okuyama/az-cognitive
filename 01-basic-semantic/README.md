# 基本的な利用方法とsemantic検索

やること
- blobにデータ(jsonかな)を格納
- cognitive searchでindexを作成
- semantic検索を行う

## 課題
pdfなども取り扱いたい DocumentExtractionSkill を使う
semantic検索データに正確な回答が必要？　※まずは全文検索をしっかり利用してみる
restで操作したかったけど、web uiからの操作になってしまった
AZのリソース削除をわすれないようにする

## AZ リソースの準備

```bash
#az login
az group create -l $LOCATION -g $RG_NAME
#az group delete -n $RG_NAME -y

az search service create -n $COGNITIVE_SEARCH -g $RG_NAME --sku Basic
#az search service delete --name $COGNITIVE_SEARCH --resource-group $RG_NAME

# azure storage account & container 作成
az storage account create \
  --name $STORAGE_ACCOUNT_NAME \
  --resource-group $RG_NAME \
  --location $LOCATION \
  --sku Standard_RAGRS \
  --kind StorageV2 \
  --allow-blob-public-access false

az storage container create \
    --name $CONTAINER_NAME \
    --account-name $STORAGE_ACCOUNT_NAME \
    --auth-mode login
```

# blobにデータを格納
azcopy cp sample_data/data01.json https://$STORAGE_ACCOUNT_NAME.blob.core.windows.net/$CONTAINER_NAME/data01.json --account-key $STORAGE_ACCOUNT_KEY



https://mydatasource0001.blob.core.windows.net/?sv=2022-11-02&ss=bfqt&srt=c&sp=rwdlacupiytfx&se=2023-11-06T14:09:53Z&st=2023-11-06T06:09:53Z&spr=https&sig=UarB4kdo61Qpbko2%2FI4M43lKLqOfrPg2mUQOfJZ7xXQ%3D



azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'


azcopy copy sample_data/data01.json https://$STORAGE_ACCOUNT_NAME.blob.core.windows.net/$CONTAINER_NAME/data01.json

# indexの作成

# 検索
