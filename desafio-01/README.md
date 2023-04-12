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

## Investigación sobre el deasfío

1.  Empezamos por entender el pedido recibido:
    ![Investigación rápida](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/definicion-app-service-plan.png)

2.  Elaboramos una lista de las tareas a realizar:

    1. Desplegar un App Service Plan gratuito
    2. Desplegar dos App Services gratuitos vinculados a este App Service Plan
    3. Como tarea extra los dos App Services tendrán el dominio personalizado

       > **OBSERVACIÓN:** Aunque los parámetros sean los correctos, los App Services gratuitos **_no soportan_** dominios personalizados y por tanto se asignarán dominios aleatorios.

## Implementación del deasfío

1.  Creamos el App Service Plan: Una buena práctica es separar la plantilla de los parámetros que necesitamos. Entonces:

    1.  Creamos una carpeta llamada `01-app-service-plan` y en la misma creamos los siguientes archivos:

        `parameters.json`:

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

        `template.json`:

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

    2.  Conexión a Azure: Ejecutamos el siguiente comando en PowerShell

        ```powershell
        Connect-AzAccount
        ```

        Luego se abrirá una ventana del navegador solicitando la cuenta con la que iniciaremos sesión
        ![Inicio de sesión](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/autorizacion-cuenta-azure.png)

    3.  Obtenemos el ID de suscripción:

        ```powershell
        Get-AzSubscription
        ```

    4.  Seleccionamos la suscripción que vamos a utilizar:

        ```powershell
        Select-AzSubscription -SubscriptionId "111efc23-4987-4881-97db-aad51bb887ed"
        ```

    5.  Obtenemos el grupo de recursos:

        ```powershell
        Get-AzResourceGroup
        ```

    6.  Se ejecuta el siguiente comando que utilizará los archivos creados anteriormente:

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

        Para verificar que se haya creado el App Service Plan correctamente buscamos el recurso en el portal de Azure y lo abrimos.
        ![Búsqueda](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/busqueda-granjita.png)

        ![App Service Plan creado](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/app-service-plan-creado.png)

2.  Dominio personalizado:

    1. Creamos una carpeta llamada `02-dominio-personalizado` y en la misma creamos los siguientes archivos:
       `template.json`:

       ```json
       {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
           "dnsZonesName": {
             "type": "string"
           }
         },
         "resources": [
           {
             "apiVersion": "2018-05-01",
             "type": "Microsoft.Network/dnsZones",
             "name": "[parameters('dnsZonesName')]",
             "location": "global",
             "dependsOn": [],
             "tags": {
               "bootcamp": ""
             },
             "properties": {}
           }
         ]
       }
       ```

       `parameters.json`:

       ```json
       {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
           "dnsZonesName": {
             "value": "azure.javierrugel.com"
           }
         }
       }
       ```

    2. Se ejecuta el siguiente comando que utilizará los archivos creados anteriormente:

       ```powershell
       New-AzResourceGroupDeployment -ResourceGroupName Bootcamp -TemplateFile .\02-dominio-personalizado\template.json -TemplateParameterFile .\02-dominio-personalizado\parameters.json
       ```

       Si el comando es satisfactorio mostrará un mensaje como este:

       ```
        DeploymentName          : template
        ResourceGroupName       : Bootcamp
        ProvisioningState       : Succeeded
        Timestamp               : 12/4/2023 19:24:27
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name             Type                       Value
                                ===============  =========================  ==========
                                dnsZonesName     String                     "azure.javierrugel.com"

        Outputs                 :
        DeploymentDebugLogLevel :
       ```

3.  Aplicaciones Web:

    Para este ejemplo voy a crear 2 WebApp con Next.js que previamente resguardaré en mi repositorio GitHub.

    ```bash
    npx create-next-app@latest --js --src-dir --use-npm rugelwebapp1
    npx create-next-app@latest --js --src-dir --use-npm rugelwebapp2
    ```

4.  App Services:

    1. En GitHub creamos los repositorios y subimos las aplicaciones. Luego se debe crear un Access Token con permiso total sobre los repositorios en cuestión.
       ![lista-access-token](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/lista-access-token.png)

       ![detalle-access-token](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/detalle-access-token.png)

    2. Creamos una carpeta llamada `03-app-services` y en la misma creamos los siguientes archivos:

       `template.json`:

       ```json
       {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
           "name": {
             "type": "string"
           },
           "domainName": {
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
           "repositoryUrl": {
             "type": "string"
           },
           "branch": {
             "type": "string"
           },
           "repositoryToken": {
             "type": "securestring"
           },
           "appLocation": {
             "type": "string"
           },
           "apiLocation": {
             "type": "string"
           },
           "appArtifactLocation": {
             "type": "string"
           }
         },
         "resources": [
           {
             "apiVersion": "2021-01-01",
             "name": "[parameters('name')]",
             "type": "Microsoft.Web/staticSites",
             "location": "[parameters('location')]",
             "tags": {
               "bootcamp": ""
             },
             "properties": {
               "domainName": "[parameters('domainName')]",
               "repositoryUrl": "[parameters('repositoryUrl')]",
               "branch": "[parameters('branch')]",
               "repositoryToken": "[parameters('repositoryToken')]",
               "buildProperties": {
                 "appLocation": "[parameters('appLocation')]",
                 "apiLocation": "[parameters('apiLocation')]",
                 "appArtifactLocation": "[parameters('appArtifactLocation')]"
               }
             },
             "sku": {
               "Tier": "[parameters('sku')]",
               "Name": "[parameters('skuCode')]"
             }
           }
         ]
       }
       ```

       `RugelWebApp1.json`:

       ```json
       {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
           "name": {
             "value": "RugelWebApp1"
           },
           "domainName": {
             "value": "rugelwebapp1.azure.javierrugel.com"
           },
           "location": {
             "value": "centralus"
           },
           "sku": {
             "value": "Free"
           },
           "skucode": {
             "value": "Free"
           },
           "repositoryUrl": {
             "value": "https://github.com/jrugel/RugelWebApp1"
           },
           "branch": {
             "value": "main"
           },
           "repositoryToken": {
             "value": "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"
           },
           "appLocation": {
             "value": "/"
           },
           "apiLocation": {
             "value": ""
           },
           "appArtifactLocation": {
             "value": ".next"
           }
         }
       }
       ```

       `RugelWebApp2.json`:

       ```json
       {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
           "name": {
             "value": "RugelWebApp2"
           },
           "domainName": {
             "value": "rugelwebapp2.azure.javierrugel.com"
           },
           "location": {
             "value": "centralus"
           },
           "sku": {
             "value": "Free"
           },
           "skucode": {
             "value": "Free"
           },
           "repositoryUrl": {
             "value": "https://github.com/jrugel/RugelWebApp2"
           },
           "branch": {
             "value": "main"
           },
           "repositoryToken": {
             "value": "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"
           },
           "appLocation": {
             "value": "/"
           },
           "apiLocation": {
             "value": ""
           },
           "appArtifactLocation": {
             "value": ".next"
           }
         }
       }
       ```

    3. Se ejecutan los siguientes comandos que utilizarán los archivos creados anteriormente:

       ```powershell
       New-AzResourceGroupDeployment -ResourceGroupName Bootcamp -TemplateFile .\03-app-services\template.json -TemplateParameterFile .\03-app-services\RugelWebApp1.json
       New-AzResourceGroupDeployment -ResourceGroupName Bootcamp -TemplateFile .\03-app-services\template.json -TemplateParameterFile .\03-app-services\RugelWebApp2.json
       ```

    4. Si los recursos fueron creados se verán dos resultados como estos:

       ```
        DeploymentName          : template
        ResourceGroupName       : Bootcamp
        ProvisioningState       : Succeeded
        Timestamp               : 12/4/2023 20:08:11
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name                   Type                       Value
                                =====================  =========================  ==========
                                name                   String                     "RugelWebApp1"
                                domainName             String                     "rugelwebapp1.azure.javierrugel.com"
                                location               String                     "centralus"
                                sku                    String                     "Free"
                                skucode                String                     "Free"
                                repositoryUrl          String                     "https://github.com/jrugel/RugelWebApp1"
                                branch                 String                     "main"
                                repositoryToken        SecureString               null
                                appLocation            String                     "/"
                                apiLocation            String                     ""
                                appArtifactLocation    String                     ".next"

        Outputs                 :
        DeploymentDebugLogLevel :
       ```

       ```
        DeploymentName          : template
        ResourceGroupName       : Bootcamp
        ProvisioningState       : Succeeded
        Timestamp               : 12/4/2023 20:09:14
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name                   Type                       Value
                                =====================  =========================  ==========
                                name                   String                     "RugelWebApp2"
                                domainName             String                     "rugelwebapp2.azure.javierrugel.com"
                                location               String                     "centralus"
                                sku                    String                     "Free"
                                skucode                String                     "Free"
                                repositoryUrl          String                     "https://github.com/jrugel/RugelWebApp2"
                                branch                 String                     "main"
                                repositoryToken        SecureString               null
                                appLocation            String                     "/"
                                apiLocation            String                     ""
                                appArtifactLocation    String                     ".next"

        Outputs                 :
        DeploymentDebugLogLevel :
       ```

    5. Buscamos en el Portal de Azure los recursos recientemente creados.
       ![busqueda-webapps](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/busqueda-webapps.png)

    6. Al abrirlos obtenemos la URL que se le asignó al servicio

       ![RugelWebApp1](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/RugelWebApp1.png)

       ![RugelWebApp2](https://raw.githubusercontent.com/jrugel/bootcamp-arroyo/main/desafio-01/RugelWebApp2.png)

       Las URL aleatorias generadas son:

       - [RugelWebApp1](https://purple-stone-098ac3b10.3.azurestaticapps.net)
       - [RugelWebApp2](https://witty-meadow-05e0f2610.3.azurestaticapps.net)
