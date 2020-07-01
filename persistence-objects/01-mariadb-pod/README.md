Podemos ver que, al no usar la abstracción **PersistentVolumeClaim-PersistentVolume**, el **manifest** del **pod** mantiene los **detalles** del **almacenamiento**, en este caso, la **localización física** del mismo (/var/mariadb)

Creamos los recursos

```bash
kubectl apply -f 06-mariadb-pod.yml
```

Podemos ver el **estado** de los **pods**, **PersistentVolumes** y **PersistentVolumeClaims** con:

```
kubectl get pod,pv,pvc
```

Podemos ver el almacenamiento físico:

```bash
ls -la /var/mariadb
```



Por último, limpiamos todos los recursos creados:

```bash
kubectl delete -f 06-mariadb-pod.yml
```

:information_source: Vemos que, al **eliminar** el **pod**, **no** se **elimina** el **directorio** donde se encuentra la **persistencia**.


