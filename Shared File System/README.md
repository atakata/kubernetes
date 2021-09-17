# Kubernetes

## Shared File System
Quando um Persistent Volume é criado e assinalado à um Block Storage, apenas um node do cluster Kubernetes consegue acessá-lo. O StorageClass: oci não permite o AccessMode: ReadWriteMany.
Para criar um armazenamento compartilhado entre pods de diferentes nodes, basta criarmos um File System e um Persistent Volume ligado à ele.

## Steps
- Criar File System no OCI
- Criar Mount Target no OCI
- Criar Persistent Volume
	> kubectl apply -f pv.yaml
- Validar Criação do PV
	> kubectl get pv
- Criar Persistent Volume Claim
	> kubectl apply -f pvc.yaml
- Validar Criação do PVC
	> kubectl get pvc
- Deploy de aplicação em Nodes diferentes com acesso ao PV
	> kubectl apply -f deployment.yaml
- Validar que os dois Pods subiram em Nodes diferentes
	> kubectl get pods -o wide
- Criar Load Balancer para a aplicação
	> kubectl apply -f lb.yaml
- Validar a criação do Load Balancer
	> kubectl get services
- Validar acesso ao File System pelo pod 1
	> kubectl exec -it \<nome do pod 1> -- bash
	> cd /usr/share/nginx/html/
- Criar arquivo a partid do pod 1
	> echo "Arquivo criado pelo Pod1" > index.html
	> exit
- Validar acesso ao File System pelo pod 2
	> kubectl exec -it \<nome do pod 2> -- bash
	> cd /usr/share/nginx/html/
- Atualizar arquivo a partir do pod 2
	> echo "Arquivo atualizado pelo Pod2" > index.html
	> exit
- Validar que o arquivo foi atualizado acessando o endereço
	> http://\<IP Externo do Load Balancer>