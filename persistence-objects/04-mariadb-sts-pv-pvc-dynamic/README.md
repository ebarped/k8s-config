En el último caso, desplegaremos un **StatefulSet** con **Dynamic Provisioning** de **PersistentVolumes**.

```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```



Igual que en el anterior ejemplo, el usuario:

- Crearía su **StatefulSet**, el cual pide montar un **PersistentVolumeClaim** como volumen.
- En vez de crear un objeto **PersistentVolumeClaim**, se embebe la configuración del mismo en el apartado **VolumeClaimTemplates** del manifest del **StatefulSet**.
  - Modo de acceso: ReadWriteOnly
  - Capacidad: 3Gi

:eyes: Como sabemos, los **StatefulSet** tienen **identidad** de **hostname**, de **red** y de **almacenamiento**. Debido a esto último, un **StatefulSet** siempre intentará utilizar el mismo **PV** asociado a su **PVC**, por lo tanto **si no utilizamos VolumeClaimTemplates**, todas las **réplicas** intentarán utilizar el **mismo** **PV**. Utilizar **VolumeClaimTemplate** hace que se **cree** un par **PVC-PV** para cada réplica.

```bash
kubectl apply -f 02-mariadb-secret.yml -f 05-mariadb-svc.yml -f 06-mariadb-sts.yml
```

Podemos ver que, automágicamente, se crea un **PV** asociado al **PVC**, y el almacenamiento físico en **/opt/local-path-provisioner/**

Podemos ver el **estado** de los **pods**, **PersistentVolumes** y **PersistentVolumeClaims** con:

```
kubectl get pod,pv,pvc
```

Podemos ver el almacenamiento físico:

```bash
ls -la /opt/local-path-provisioner/*
```



Como decíamos, podemos **escalar** el nº de **réplicas** gracias a **VolumeClaimTemplate**:

```bash
kubectl scale statefulset mariadb --replicas=2
```



Por último, limpiamos los recursos creados:

```bash
kubectl delete -f 02-mariadb-secret.yml -f 05-mariadb-svc.yml -f 06-mariadb-sts.yml
```

:grey_question: Podemos ver que los PVC-PV no se eliminan, aunque la **Reclaim Policy** sea **Delete**. Esto es debido a la propiedad de **identidad de almacenamiento** de los **StatefulSet**.

Para eliminar completamente estos objetos, debemos hacerlo de forma imperativa:

```bash
kubectl delete pvc storage-mariadb-1 storage-mariadb-0
```



:information_source: Como vemos, también se **elimina** el **PersistentVolume** y el **almacenamiento físico** de **/opt/local-path-provisioner**



Para eliminar el **StorageClass** de **Local-Path-Provisioner**:

```bash
kubectl delete -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```


