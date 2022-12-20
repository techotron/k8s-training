# CKAD
## Kind
### Running Locally
**Bring the cluster up**

This will create a cluster and add an Nginx ingress controller. The controller container takes a little while to start up so if you're planning on testing it, check the status of the pods with `kubectl get pods -n ingress-nginx`. 

```bash
make up
```

**Note:** Context is added as `kind-kind-ckad`, eg:
```bash
k get pods --all-namespaces --context kind-kind-ckad
```

Set the context so you don't have to specify it on each command
```
kubectl config use-context kind-kind-ckad
```

**Delete the cluster**

```bash
make down
```

### Ingress Controller
The controller nodes have port mappings for TCP 80 and 443. These can be used by an ingress controller running on the control plane. An Nginx ingress controller is automatically deployed when the cluster is started with `make up` but can be added separately with `make deploy-nginx-ingress`. 

**Note:** The below example creates a "catch all ingress". To make use of an ingress which specifies a hostname, you'd need to point the hostname to localhost in /etc/hosts. 

To quickly test this is working, run the following once the cluster is up:
```bash
kubectl create ns ingress-test
kubectl create deployment ingress-test --image=nginx -n ingress-test
kubectl expose deployment ingress-test -n ingress-test --port 80 --name ingress-test-svc
kubectl create ingress ingress-test-ingress -n ingress-test --annotation nginx.ingress.kubernetes.io/rewrite-target=/$2 --rule="/test=ingress-test-svc:80"
curl localhost/test
```

You should see the contents of the default nginx page

(Clean up all the objects in the namespace by deleting the namespace itself)
