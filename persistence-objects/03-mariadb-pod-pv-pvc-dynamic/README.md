En este caso, utilizaremos **Dynamic Provisioning** de **PersistentVolumes**, es decir, los **PersistentVolumes** se **crearán/eliminarán** **automáticamente** al **crear/eliminar** **PersistentVolumeClaims**.

```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```



Igual que en el anterior ejemplo, el usuario:

- Crearía su **pod**, el cual pide montar un **PersistentVolumeClaim** como volumen.
- Crearía un **PersistentVolumeClaim**, donde especifica los requisitos de almacenamiento necesarios, en este caso:
  - Modo de acceso: ReadWriteOnly
  - Capacidad: 3Gi

```bash
kubectl apply -f 04-mariadb-pvc.yml -f 06-mariadb-pod.yml
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



Por último, limpiamos todos los recursos creados:

```bash
kubectl delete -f 04-mariadb-pvc.yml -f 06-mariadb-pod.yml
```



:information_source: Como vemos, también se **elimina** el **PersistentVolume** y el **almacenamiento físico** de **/opt/local-path-provisioner**

Para eliminar el **StorageClass** de **Local-Path-Provisioner**:

```bash
kubectl delete -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

