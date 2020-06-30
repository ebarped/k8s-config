No se deben tener los secrets en el Version Control System:

kubectl create secret generic nginx-certs --from-file=certs/localhost.crt --from-file=certs/localhost.key
