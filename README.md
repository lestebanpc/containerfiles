Creando imagenes usando Podman y BuildAh


Creando imagenes usando ContainerD y BuildKit


#Inicializando
systemctl --user start containerd.service
systemctl --user start buildkit.service

#Creacion de imagen
bat ./1.0.0-alpine/Dokerfile
vim ./1.0.0-alpine/Dokerfile
nerdctl build -t lucianoepc/basic-tools:1.0.0-alpine ./1.0.0-alpine


#Prueba usando CLI de ContainerD:
nerdctl run --name cnt-tools-1.0.0 -d lucianoepc/basic-tools:1.0.0-alpine
nerdctl container ls --all
nerdctl exec -it cnt-tools-1.0.0 /bin/bash
nerdctl container kill cnt-tools-1.0.0


#Limpieza de una imagen para volver a generarla con el mismo tag
nerdctl image ls
nerdctl container ls --all

nerdctl container rm XXXXX
nerdctl image rm lucianoepc/basic-tools:1.0.0-alpine


#Subir la imagen 
nerdctl login -u lucianoepc
nerdctl push lucianoepc/basic-tools:1.0.0-alpine

#Probar en Kubernates (namespace 'ppoc-lpenac')
oc run net-tools1 -it --rm --image=lucianoepc/basic-tools:1.0.0-alpine --restart=Never -- bash

bat podo-nettools-1_alpine.yaml
bat podo-nettools-root_alpine.yaml

oc create -f podo-nettools-1_alpine.yaml
oc get pod -n ppoc-lpenac
oc exec pod/podo-nettools-1 -it -n ppoc-lpenac -- bash

oc delete pod/podo-nettools-1 -n ppoc-lpenac


