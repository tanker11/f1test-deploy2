# Deploy teszt az eredeti (lokális docker-compose) projekthez

## Deploy manuálisan, ACR-ben

Legyártjuk a konténereket és kipróbáljuk, hogy működnek-e lokálisan:
docker-compose up --build


az login
az acr login --name deploy2tanker11


EZ LEHET HOGY CSAK DOCKER-COMPOSE KONVERTÁLÁSÁHOZ KELL:
az extension add --name containerapp
(NINCS MÉG STABIL VERZIÓJA!)

container registry létrehozása
az acr create --resource-group myResourceGroup --name myContainerRegistry --sku Basic

ACR-be bejelentkezés docker CLI-vel
az acr login --name myContainerRegistry

Taggelés:

```docker tag f1test-deploy2-apiload1:latest deploy2tanker11.azurecr.io/f1test-deploy2-apiload1:latest
docker tag f1test-deploy2-transform2:latest deploy2tanker11.azurecr.io/f1test-deploy2-transform2:latest
docker tag f1test-deploy2-display3:latest deploy2tanker11.azurecr.io/f1test-deploy2-display3:latest```

Pusholás:

```docker push deploy2tanker11.azurecr.io/f1test-deploy2-apiload1:latest
docker push deploy2tanker11.azurecr.io/f1test-deploy2-transform2:latest
docker push deploy2tanker11.azurecr.io/f1test-deploy2-display3:latest```

Ezzel felpusholódnak a konténerek az ACR-be.

Csinálunk egy VNET-et, aztán kézzel létrehozunk egy-egy Container Instance-t. Networkingnél VNET és portok megengedése!
Load Balancert csinálunk, ami adja majd a publikus IP címet.

Manuálisan létrehozunk Azure Container Instance-okat, figyelve a portokra, és a VNET-hez adásra.
A Load Balancer publikus címével, az 5000, 5001 és 5002 portokon a /health /data és /plot működnek.

Újra pusholt image esetén újraépül a konténer is, és fut benne az új kód.

Alkalmazásnevek:
apiload1
transform2
display3

ACR neve: deploy2tanker11
címe: deploy2tanker11.azurecr.io
