# Desafío 1

## Pedido recibido:

> Buenos dias para todos, espero se encuentren bien por parte del grupo de facilitadores del Bootcamp, queremos que Uds trabajen en lo siguiente:
>
> Usando al menos uno de las tecnología de Infrastructure as Code vista en la sesion 2 (si usan las 3 seria el mejor escenario), desplegar en sus cuentas gratuitas de azure los siguientes recursos:
>
> 1. Desplegar App Service Plan (se sugiere usar un SKU free que es soportado para este recurso)
> 2. Desplegar 2 App services vinculados a este app service plan
>
> Recuerden que el equipo del Bootcamp esta disponible para cualquier duda que surja
>
> https://learn.microsoft.com/en-us/azure/app-service/overview-hosting-plans

## Implementación del deasfío:

1. Empezamos por entender el pedido recibido:
   ![Investigación rápida](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/definicion-app-service-plan.png)

2. Creamos el App Service Plan: Una buena práctica es separar la plantilla de los parámetros que necesitamos. Entonces:

   1. Creamos un archivo de parámetros `parameters.json`:

      ```json
      {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "name": {
            "value": "GranjitaBootcamp"
          },
          "location": {
            "value": "East US"
          },
          "numberOfWorkers": {
            "value": "1"
          },
          "sku": {
            "value": "Free"
          },
          "skuCode": {
            "value": "F1"
          },
          "workerSize": {
            "value": "0"
          },
          "workerSizeId": {
            "value": "0"
          }
        }
      }
      ```

   2. Creamos un archivo de parámetros `template.json`:

      ```json
      {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "name": {
            "type": "string"
          },
          "location": {
            "type": "string"
          },
          "sku": {
            "type": "string"
          },
          "skucode": {
            "type": "string"
          },
          "workerSize": {
            "type": "string"
          },
          "workerSizeId": {
            "type": "string"
          },
          "numberOfWorkers": {
            "type": "string"
          }
        },
        "resources": [
          {
            "apiVersion": "2018-11-01",
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "kind": "linux",
            "tags": {
              "bootcamp": ""
            },
            "properties": {
              "name": "[parameters('name')]",
              "workerSize": "[parameters('workerSize')]",
              "workerSizeId": "[parameters('workerSizeId')]",
              "numberOfWorkers": "[parameters('numberOfWorkers')]",
              "reserved": true,
              "zoneRedundant": false
            },
            "sku": {
              "Tier": "[parameters('sku')]",
              "Name": "[parameters('skuCode')]"
            }
          }
        ]
      }
      ```
