# Kubernete
1-Descargar minikube:

curl -Lo minikube
https://storage.googleapis.com/minikube/releases/v0.30.0/minikube-linux-amd64 &&

chmod +x minikube &&

sudo cp minikube /usr/local/bin/ &&

rm minikube

2-Descargar kubectl:

curl -Lo kubectl
https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/linux/amd64/kubectl
&&

chmod +x kubectl &&

sudo cp kubectl /usr/local/bin/ &&

rm kubectl

3-Inicie la máquina virtual con minikube

minikube start --vm-driver = virtualbox

Práctica 1.- Tolerancia a fallos

1-Crear una vaina:

kubectl run pagweb --image jlr2/aplicacionesweb:v1

2-En caso de que el pod deje de funcionar, K8S crea otro:

2.1-Vamos a eliminar el pod:

kubectl delete pod/pagweb-bb599dd-pxs2d

2.2-Podemos comprobar que se ha creado un nuevo pod:

kubectl get pod

3-También podemos comprobar el despliegue de recursos :

kubectl get deploy

4-Y el recurso ReplicaSet:

kubectl get rs

Práctica 2.- Escalabilidad.

1-ReplicateSet puede replicar los pods:

kubectl scale deploy pagweb --replicas=3

2-Comprobar (cada pod se ejecuta en un nodo diferente):

kubectl get pod -o wide

Práctica 3.- Equilibrio de carga

1-Crea un servicio de recursos para acceder a la aplicación:

kubectl expose deployment pagweb --port=80 --type=NodePort

2-Obtener el puerto de la aplicación:

kubectl get services

3-Obtenga las direcciones IP del clúster donde se ejecuta la aplicación:

minikube ip

4-Compruebe que el acceso sea correcto con el navegador web local:

Poner en navegador la IP dad en el paso anterior

Práctica 4.- Actualizaciones continuas.

1-Modificar la aplicación (creamos una nueva imagen + subir a dockerhub)

echo "\<h1\>Prueba2\</h1\>" \> index.html

docker build -t jlr2/aplicacionweb:v2

docker push jlr2/aplicacionesweb:v2

2-Después de modificar la aplicación, el proceso de actualización en la
implementación es muy fácil:

kubectl set image deployment pagweb pagweb=jlr2/aplicacionesweb:v2

Práctica 5.- Rollback

1-En caso de que tengamos que volver a una versión anterior:

kubectl rollout undo deployment/pagweb

Práctica 6.- Enrutamiento.

1-Es posible acceder a la aplicación por un nombre DNS. Utilizaremos el dominio
nip.io. Es necesario crear un archivo ingress.yaml.

2-Crear el ingreso de recursos:

kubectl create -f ingress.yaml

3-Comprobar:

kubectl get ingress
