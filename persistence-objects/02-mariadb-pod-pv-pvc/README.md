El **administrador** creará una serie de **PersistentVolumes**, listos para ser **consumidos** por los **pods** de los usuarios.

```bash
kubectl apply -f pv-5Gi.yml
```

Podemos ver que, en este caso, tenemos un **PersistentVolume** que ofrece **5Gi** de almacenamiento en la dirección física /**tmp/data**

El usuario, abstrayéndose de estos detalles:

- Crearía su **pod**, el cual pide montar un **PersistentVolumeClaim** como volumen
- Crearía un **PersistentVolumeClaim**, donde especifica los requisitos de almacenamiento necesarios, en este caso:
  - Modo de acceso: ReadWriteOnly
  - Capacidad: 3Gi

```bash
kubectl apply -f 04-mariadb-pvc.yml -f 06-mariadb-pod.yml
```



Podemos ver el **estado** de los **pods**, **PersistentVolumes** y **PersistentVolumeClaims** con:

```
kubectl get pod,pv,pvc
```

Podemos ver el almacenamiento físico:

```bash
ls -la /tmp/data
```



Por último, limpiamos todos los recursos creados:

```bash
kubectl delete -f 04-mariadb-pvc.yml -f 06-mariadb-pod.yml -f pv-5Gi.yml
```


