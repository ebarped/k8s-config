Podemos ver que el **Deployment** de **nginx** monta un **ConfigMap** y un **Secret** como ficheros.

El **ConfigMap** almacena la configuración de **nginx.**

El **Secret** almacena los **certificados** utilizados en la **config**, codificados en **base64**:

```bash
cat 02-nginx-secret.yml  | grep localhost.crt | awk '{print $2}' | base64 -d
```

```bash
cat 02-nginx-secret.yml  | grep localhost.key | awk '{print $2}' | base64 -d
```



**Desplegamos** **nginx**:

```bash
kubectl apply -f 02-nginx-secret.yml -f 03-nginx-cm.yml -f 05-nginx-svc.yml -f 06-nginx-deploy.yml
```

**Obtenemos** la **IP** de nuestra máquina **Vagrant**:

```bash
ip a | grep enp
```

**Obtenemos** los **NodePorts** de nuestro servicio **nginx**:

```bash
kubectl get svc nginx
```

**Conectamos** desde el **navegador** con ambos pares **IP:NodePort**, y comprobamos el funcionamiento.

Limpiamos los recursos:

```bash
kubectl delete -f 02-nginx-secret.yml -f 03-nginx-cm.yml -f 05-nginx-svc.yml -f 06-nginx-deploy.yml
```



### Buenas prácticas

:warning: No se deben almacenar los **Secrets** en el **Version Control System**.

```bash
kubectl create secret generic nginx-certs --from-file=certs/localhost.crt --from-file=certs/localhost.key
```

**Recreamos** el **despliegue** sin **02-nginx-secret.yml**:

```bash
kubectl apply -f 03-nginx-cm.yml -f 05-nginx-svc.yml -f 06-nginx-deploy.yml
```

**Volvemos** a **comprobar** desde el **navegador** con los nuevos **NodePorts**.

**Limpiamos** los **recursos** creados.

```bash
kubectl delete -f 03-nginx-cm.yml -f 05-nginx-svc.yml -f 06-nginx-deploy.yml
```


