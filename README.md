# !Commands of certificate generations.

## CREATE USER (generate user authentication credentials)!

```bash
  mkdir  redhat
```


```bash
  cd redhat
```
## Copy certificate authority key and certificate to our user's directory.

```bash
cp /etc/kubernetes/pki/ca.crt /root/redhat
```
```bash
cp /etc/kubernetes/pki/ca.key /root/redhat
```
## Generate key for our user.

```bash
 openssl genrsa -out redhat.key 2048
```
## Generate csr for our user.

```bash
 openssl req -new -key redhat.key -out redhat.csr
```
## Generate crt of our user.

```bash
 openssl x509 -req -in redhat.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out redhat.crt -days 365
```
## View the certificate.

```bash
 openssl x509 -noout -text -in redhat.crt
```
## Create config file for our user.

```bash
cp /etc/kubernetes/admin.conf redhat.conf
```
##  (edit it according to our user)

- change name, user, current-context in context section.
- change name, certificate data in user section. 

## Decode the crt,key files.

```bash
cat redhat.crt | base64 -w0 
```

```bash
cat redhat.key | base64 -w0 
```
## Share this file to get access of the user.

```bash
useradd redhat
```

```bash
cp /root/redhat/redhat.conf /home/redhat/
```
```bash
chown redhat:redhat /home/redhat/redhat.conf
```

```bash
su - redhat
```
```bash
mkdir .kube
```
```bash
cd .kube
```
```bash
cp ~/redhat.conf config
```
## This command help to see the config yaml file intendation.
```bash
kubectl config view
```
## Create role-bindings with existing roles.

```bash
kubectl create ns uber
```
```bash
kubectl create rolebinding redhat-bind1 --clusterrole=admin --user=redhat -n uber
```
```bash
kubectl create clusterrolebinding redhat-bind1 --clusterrole=admin --user=redhat
```
```bash
kubectl get roles --all-namespaces
```
```bash
kubectl get clusterrole --all-namespaces
```
```bash
kubectl describe clusterrole admin
```
## Create roles.

```bash
kubectl create role node-reader --verb=get --verb=list --verb=watch --resource=nodes
```
```bash
kubectl get role
```
```bash
kubectl describe role node-reader
```
```bash
kubectl create rolebinding (name) --role=node-reader --user=redhat
```
## Switch to the redhat user (we can create any user according to your choice)

```bash
su - redhat
```
```bash
kubectl get pods
```
