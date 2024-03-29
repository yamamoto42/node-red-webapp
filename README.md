# node-red-webapp
```
myloc="eastus"
myrg="nrrg"
myasp="nrasp"
myapp="nrapp"
mysta="nrsta"
mystc="nrstc"
mysti="nrsti"

az login
az account set --subscription <subscription_id>
az configure --defaults location=$myloc
az configure --defaults group=$myrg

az group create -n $myrg -l $myloc
az appservice plan create -n $myasp -g $myrg --is-linux --sku B1

az webapp create  \
-g $myrg  \
-p $myasp \
-n $myapp \
-i nodered/node-red-docker

az webapp stop -n $myapp

az webapp config appsettings set \
-g $myrg  \
-n $myapp \
--settings PORT=1880

az storage account create -n $mysta -g $myrg
az storage container create -n $mystc --account-name $mysta

az storage account keys list -n $mysta

az webapp config storage-account add \
-g $myrg \
-n $myapp \
-i $mysti \
-a $mysta \
-k "<* key ********************************>" \
--sn $mystc \
-t AzureBlob \
-m /data

az webapp restart -n $myapp
```
