# Helm commands

* Create configuration `helm create <name>`
* Apply initial configuration: `helm install <name> .`
* Apply updates: `helm upgrade <name> .`

# K8s commands

* Get pods, deployments and services: `kubectl get <pods | deployments | services>`
* Check all pods: `kubectl describe pods`
* Check a pod: `kubectl describe pod <name>`
* Delete pod: `kubectl delete pod <name>`
* Check logs: `kubectl logs <name>`



# Create deployment:
```
kubectl create deployment <name> --image=<registry/url/image> --dry-run=client -o yaml > deployment.yml
```

# Create service
```
kubectl create service clusterip <name> --tcp=<8888> --dry-run=client -o yaml > service.yml 
**kubectl create service nodeport <name> --tcp=<3000> --dry-run=client -o yaml > service.yml**
```
* **clusterip**: can only be accessed from within the cluster
* **nodeport**: can be accessed from outside the cluster


# Secrets

* Create secrets, several at once, or one by one.
```
kubectl create secret generic <name> --from-literal=key=value

kubectl create secret generic secret1 --from-literal=key1=value1 --from-literal=key2=value2
```
* Get secrets `kubectl get secrets`
* View secret content `kubectl get secrets <name> -o yaml`

## Edit a secret
The easiest way is to delete it and recreate it, but if there are more than one secret, we don't want to lose the others.
Remember that secrets are in `base64`, so if we want to edit a secret, we must do it in `base64`.

1. Edit the secret with `kubectl edit secret <name>` this will invoke the editor
2. Change the value (you can use an [online editor to convert to base64](https://www.rapidtables.com/web/tools/base64-decode.html))
3. Press **i** to insert lines and edit the file
4. Put the value to decode on a new line
5. Press **esc** and then `:. ! base64 -D` to decode the value
6. Press **i** to insert or edit the value
7. Press **esc** and then `:. ! base64` to encode the value
8. Edit the file again **i** and leave the line in its position
9. Press **esc** and then **:wq** to save and exit



## Configure Google Cloud secrets to get images

1. Create secret:
```
kubectl create secret docker-registry gcr-json-key --docker-server=GOOGLE-SERVER-docker.pkg.dev --docker-username=_json_key --docker-password="$(cat 'PATH/TO/Store Microservices IAM.json')" --docker-email=YOUR_EMAIL@gmail.com
```

2. Secret path to use the key:
```
kubectl patch serviceaccounts default -p '{ "imagePullSecrets": [{ "name":"gcr-json-key" }] }'
```


## Export and apply configurations with files (secrets in this case)
* To export configuration files

```
kubectl get secret <name> -o yaml > <name>.yml
```

* Apply configuration based on file
```
kubectl create -f <name>.yml
```

## Port forwarding for local access

If you have trouble accessing NodePort services, you can create a direct port-forward tunnel:

```
kubectl port-forward service/client-gateway 3000:3000
```