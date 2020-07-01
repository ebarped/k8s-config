## Despliegue básico

Podemos ver que el **StatefulSet** de **MariaDB** monta pares key-value de un **Secret** como variables de entorno.


El **Secret** almacena las **credenciales** utilizadas en la **config**, codificadas en **base64**:

```bash
cat 02-mariadb-secret.yml | grep mysql_database | awk '{print $2}'| base64 -d
```

```bash
cat 02-mariadb-secret.yml | grep mysql_user | awk '{print $2}'| base64 -d
```

```bash
cat 02-mariadb-secret.yml | grep mysql_password | awk '{print $2}'| base64 -d
```

```bash
cat 02-mariadb-secret.yml | grep mysql_root_password | awk '{print $2}'| base64 -d
```

**Desplegamos** **MariaDB**:

```bash
kubectl apply -f 02-mariadb-secret.yml -f 05-mariadb-svc.yml -f 06-mariadb-sts.yml
```

**Obtenemos** la **IP** de nuestra máquina **Vagrant**:

```bash
ip a | grep enp
```

**Obtenemos** el **NodePort** de nuestro servicio **mariadb**:

```bash
kubectl get svc mariadb
```

**Conectamos** desde el cliente **mysql** y comprobamos el funcionamiento.

```bash
mysql --host=localhost --port=<NodePort> --user=mariadb --password=mariadb_pass mariadb --protocol=TCP
```

Limpiamos los recursos:

```bash
kubectl delete -f 02-mariadb-secret.yml -f 05-mariadb-svc.yml -f 06-mariadb-sts.yml
```
