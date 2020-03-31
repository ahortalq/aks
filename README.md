# AKS

Vamos a ver cómo montar un cluster AKS con el objetivo de utilizar 'xl up'.

## Creación de un grupo de recursos

Primero creamos un grupo de recursos de nombre 'AKS' y en localización 'Oeste de Europa' para que se metan ahí todos los recursos del AKS.

Una vez creado el grupo de recursos, clickamos en él y empezamos a añadir contenido.

## Agregar contenido AKS

Una vez dentro del grupo de recursos, agregamos contenido. Filtramos por AKS y seleccionamos el servicio 'Kubernetes Service'. Luego, damos a 'Create'.

Tenemos que indicar:

* Nombre del cluster de K8s
* Región

El resto de opciones por defecto.

Ahora, la creación del servicio K8s es gratuíto, por lo que nos van a cobrar es por los nodos que creemos y que asociemos al cluster.

Lo siguiente es habilitar/deshabilitar el escalado.

La autenticación, networking, monitorización y tags lo dejamos por defecto. Finalmente damos a 'Create' y después de un tiempo tendremos nuestros cluster K8s creado.

Seleccionar el tipo de nodo = Standard_B4ms

## Obtener las credenciales

Primero nos logamos en la cuenta de Azure.

```
$ az login
You have logged in. Now let us find all the subscriptions to which you have access...
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "0f35c555-5162-48ba-ab57-a0235a2385e0",
    "id": "2c7e7531-85ec-400b-81b4-fe6158191c7d",
    "isDefault": true,
    "managedByTenants": [],
    "name": "2C-Pay-As-You-Go",
    "state": "Enabled",
    "tenantId": "0f35c555-5162-48ba-ab57-a0235a2385e0",
    "user": {
      "name": "xl-test-azure@deployitsupportxebialabs.onmicrosoft.com",
      "type": "user"
    }
  },
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "0f35c555-5162-48ba-ab57-a0235a2385e0",
    "id": "d4b7b057-a628-43e2-a3da-1f9ea1b3ece3",
    "isDefault": false,
    "managedByTenants": [],
    "name": "D4-Pay-As-You-Go",
    "state": "Enabled",
    "tenantId": "0f35c555-5162-48ba-ab57-a0235a2385e0",
    "user": {
      "name": "xl-test-azure@deployitsupportxebialabs.onmicrosoft.com",
      "type": "user"
    }
  }
]
```

Establecemos la suscripción por defecto, en nuestro caso:

```
$ az account set -s D4-Pay-As-You-Go
```

Una vez hecho esto, ya podemos obtener las credenciales:

```
$ az aks get-credentials --resource-group AKS --name aks-cluster-k8s
Merged "aks-cluster-k8s" as current context in /home/jcla/.kube/config
```

Ya deberíamos poder trabajar con nuestro cluster K8s.

## Iniciar el dashboard
El cluster creado usa RBAC, por tanto se debe crear un ClusterRoleBinding para que se pueda acceder al Dashboard. Para ello ejecutar:
```
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```
Pero esta opción anterior no es segura, no se recomienda. Para más información: https://docs.microsoft.com/es-es/azure/aks/kubernetes-dashboard

Para iniciar el Dashboard ejecutar:
```
az aks browse --resource-group AKS --name aks-cluster-k8s
```
