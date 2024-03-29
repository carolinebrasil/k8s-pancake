1 - Criar um pod com um volume não persistente.

	01-pod-emptydir.yaml
	

2 - Criar um service/ep apontando para um pod.

	01-pod-simples.yaml // 02-pod-para-service.yaml
	01-svc-simples.yaml // 02-service-para-pod.yaml
	- kubectl get svc (ClusterIP) and curl + IP svc returns index.html from NGINX
	

	
3 - Colocar um node para que não execute nenhum containers.

-------

PT 1 - include noschedule on the node k8s-03

-------

``` bash
	#kubectl taint nodes k8s-03 key1=value1:NoSchedule
	node/k8s-03 tainted
	# kubectl describe no k8s-03 
	Name:               k8s-03
	Roles:              <none>
	Labels:             beta.kubernetes.io/arch=amd64
        	            beta.kubernetes.io/os=linux
	                    dc=BR
	                    disk=HDD
	                    kubernetes.io/arch=amd64
	                    kubernetes.io/hostname=k8s-03
	                    kubernetes.io/os=linux
	                    network-pub=Fibra
	Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
	                    node.alpha.kubernetes.io/ttl: 0
	                    volumes.kubernetes.io/controller-managed-attach-detach: true
	CreationTimestamp:  Wed, 21 Apr 2021 20:55:35 +0000
	Taints:             key1=value1:NoSchedule
```
-------



PT 2 - create deployment with 10 replicas

	03-node-without-pod_no-schedule_.yaml




------- 


``` bash
# kubectl get node
NAME     STATUS   ROLES                  AGE   VERSION
k8s-01   Ready    control-plane,master   67d   v1.21.0
k8s-02   Ready    <none>                 67d   v1.21.0
k8s-03   Ready    <none>                 67d   v1.21.0
# kubectl get pods -o wide
NAME                                         READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
03-deploy-test-taint-node-5f8bd444db-4dhb4   1/1     Running   0          55s   10.40.0.7    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-67dp8   1/1     Running   0          55s   10.40.0.6    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-bpfq4   1/1     Running   0          55s   10.40.0.4    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-cff2d   1/1     Running   0          55s   10.40.0.11   k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-cktsw   1/1     Running   0          55s   10.40.0.9    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-l4g2r   1/1     Running   0          55s   10.40.0.1    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-lwrhh   1/1     Running   0          55s   10.40.0.8    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-n7s45   1/1     Running   0          55s   10.40.0.3    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-vs6db   1/1     Running   0          55s   10.40.0.2    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-wgckp   1/1     Running   0          55s   10.40.0.10   k8s-02   <none>           <none>
```   
  
-------

PT 3 - remove the noschedule on node k8s-03 (untaint) and add + 5 replicas in deployment

-------

``` bash
# kubectl taint nodes k8s-03 key1=value1:NoSchedule-
node/k8s-03 untainted
# kubectl get pods -o wide
NAME                                         READY   STATUS              RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
03-deploy-test-taint-node-5f8bd444db-45j9s   0/1     ContainerCreating   0          3s      <none>       k8s-03   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-4dhb4   1/1     Running             0          7m34s   10.40.0.7    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-67dp8   1/1     Running             0          7m34s   10.40.0.6    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-bpfq4   1/1     Running             0          7m34s   10.40.0.4    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-cff2d   1/1     Running             0          7m34s   10.40.0.11   k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-cktsw   1/1     Running             0          7m34s   10.40.0.9    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-fzhh2   0/1     ContainerCreating   0          3s      <none>       k8s-03   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-l4g2r   1/1     Running             0          7m34s   10.40.0.1    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-lwrhh   1/1     Running             0          7m34s   10.40.0.8    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-n7s45   1/1     Running             0          7m34s   10.40.0.3    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-pz4x4   0/1     ContainerCreating   0          3s      <none>       k8s-03   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-vhwvv   0/1     ContainerCreating   0          3s      <none>       k8s-03   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-vs6db   1/1     Running             0          7m34s   10.40.0.2    k8s-02   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-wdxmp   0/1     ContainerCreating   0          3s      <none>       k8s-03   <none>           <none>
03-deploy-test-taint-node-5f8bd444db-wgckp   1/1     Running             0          7m34s   10.40.0.10   k8s-02   <none>           <none>
```

-------

4 - Criar um PV Hostpath.

	04-pv-hostpath.yaml
	04-pod-with-pv-hostpath.yaml


5 - Criar um initcontainer para executar uma tarefa necessária para a subida do container principal.

	05-init-container-other.yaml
	05-init-container-svc.yaml
	
	
6 - Criar um daemonset.

	06-deamonset-fluentd.yaml

7 - Criar um deployment do nginx com 5 réplicas.

	07-deploy-simples-5-replicas.yaml
	
8 - Ver quais os pods que mais estão consumindo cpu através do kubectl top.

	kubectl top pod XXXX --containers
	
9 - Organizar a saída do comando "kubectl get pods" pelo tamanho do capacity storage.

	PD

10 - Qual a quantidade de nodes que estão aceitando novos containers?

	kubectl describe nodes | grep -i taint


11 - Criar um secret e dois pods, um montando o secret em filesystem e outro como variável
	
	Step 01

	echo "motherlode" > 11-secret.txt

	kubectl create secret generic 11-secret --from-file=11-secret.txt

	11-print-secret.yaml // objeto criado
	
	11-pod-with-secret-file.yaml


	Step 02

	11-pod-with-secret-env.yaml


12 - Fazer a instalação do nginx em determinada versão, atualizar e depois realizar o rollback com o --record.



13 - EXTRA - Realizar o backup do etcd.


14 - Identificar quais pods fazem parte de determinado services.


15 - Usar o nslookup e/ou outras ferramentas para pegar o dns do pod e do service.


16 - Adicionar mais um node no cluster.


17 - Adicionar um label no node.

18 - Subir um pod com afinidade de node.

19 - Ajustar o nome de uma imagem com nome errado de um deployment

20 - Criar um cronjob.

21 - Criar Pod com o parametro containerport.

22 - Declarar a variável NGINX_PORT no env do container.

23 - Declarar a variável na configmap e passar para container.

24 - Declarar a variável no secret e passar para o container.

25 - Configurar resources limits no deployment.

26 - Configurar liveness e readiness no deployment.

27 - Criar um volume Emptydir e compartilhar entre 2 containers.

28 - Customizar o parâmetro command do container.

29 - Configurar um nodeselector para o Pod.

30 - Executar o kubectl rollback pause e resume.

31 - Utilizar o edit ou outro comando para corrigir o selector de um service para que ele funcione corretamente.

32 - Criar uma secret generic, outra from file e outra literal.

33 - Criar um configmap generic, outro from file e outro literal.

34 - Verifique a saúde de todos os nodes e seus componentes, como kubelet, proxy, api, controllers, schedullers, etc.

