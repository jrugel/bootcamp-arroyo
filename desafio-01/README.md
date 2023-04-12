# Desafío 1

## Pedido recibido

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

## Implementación del deasfío

1.  Empezamos por entender el pedido recibido:
    ![Investigación rápida](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/definicion-app-service-plan.png)
    Entonces, al service plan le pondremos el nombre GranjitaBootcamp.

2.  Creamos el App Service Plan: Una buena práctica es separar la plantilla de los parámetros que necesitamos. Entonces:

    1.  Creamos una carpeta llamada `01-app-service-plan` para los archivos del App Service Plan:

    2.  En la carpeta `01-app-service-plan` creamos un archivo de parámetros con el nombre `parameters.json`:

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

    3.  En la carpeta `01-app-service-plan` creamos un archivo que contendrá la definición de la plantilla con el nombre `template.json`:

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

    4.  Conexión a Azure: Ejecutamos el siguiente comando en PowerShell

        ```powershell
        Connect-AzAccount
        ```

        Luego se abrirá una ventana del navegador solicitando la cuenta con la que iniciaremos sesión
        ![Inicio de sesión](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/autorizacion-cuenta-azure.png)

    5.  Obtenemos el ID de suscripción:

        ```powershell
        Get-AzSubscription
        ```

    6.  Seleccionamos la suscripción que vamos a utilizar:

        ```powershell
        Select-AzSubscription -SubscriptionId "111efc23-4987-4881-97db-aad51bb887ed"
        ```

    7.  Obtenemos el grupo de recursos:

        ```powershell
        Get-AzResourceGroup
        ```

    8.  Se ejecuta el siguiente comando que utilizará los archivos creados anteriormente:

        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName Bootcamp -TemplateFile .\01-app-service-plan\template.json -TemplateParameterFile .\01-app-service-plan\parameters.json
        ```

        Si el proceso fue exitoso debe arrojar un mensaje similar al siguiente:

        ```
        DeploymentName          : template
        ResourceGroupName       : Bootcamp
        ProvisioningState       : Succeeded
        Timestamp               : 12/4/2023 14:39:37
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name               Type                       Value
                                =================  =========================  ==========
                                name               String                     "GranjitaBootcamp"
                                location           String                     "East US"
                                sku                String                     "Free"
                                skucode            String                     "F1"
                                workerSize         String                     "0"
                                workerSizeId       String                     "0"
                                numberOfWorkers    String                     "1"
        Outputs                 :
        DeploymentDebugLogLevel :
        ```

3.  Creamos los App Services:
    1.  Creamos una carpeta llamada `02-app-services` para los archivos de los App Services:
