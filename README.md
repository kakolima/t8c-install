# t8c-install

Procedimentos para Instalar o Turbonomic em k8s (Se for OCP, NÃO SEGUIR ESTES PASSOS).

1- Criar um namespace para o Turbonomic
kubectl create namespace turbonomic

2- Instalar o CRD, Service Account e Role Binding
Se k8s 1.22 ou maior:
kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/config/crd/bases/charts.helm.k8s.io_xls.yaml
Se for k8s de 1.11 a 1.21:
kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/deploy/crds/charts_v1alpha1_xl_crd.yaml

kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/deploy/service_account.yaml -n turbonomic

kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/deploy/role.yaml -n turbonomic

kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/deploy/role_binding.yaml -n turbonomic

3- Instalar o Operator
kubectl create -f https://raw.githubusercontent.com/turbonomic/t8c-install/master/operator/deploy/operator.yaml -n turbonomic

4- Checar se o pod do operator fica 1/1
kubectl get pods -n turbonomic -w

5- Baixar Fazer o deploy dos charts:
curl -O https://github.com/kakolima/t8c-install/blob/main/pov_charts_v1alpha1_xl_cr.yaml
(Atenção: alterar o storageClass para um que seja Retain e WaitForFirstConsumer)
kubectl apply -f pov_charts_v1alpha1_xl_cr.yaml -n turbonomic

6- Aguardar todos os pods ficarem 1/1
kubectl get pods -n turbonomic -w

7- Acessar a URL que o Ingress criou como serviço
https://algumacoisa...
