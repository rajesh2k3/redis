# Redis on Okteto Cloud - Kubernetes for Developer


## Pre-requisite

- Create Okteto Cloud Acccount - (https://okteto.com/) login via Github Credentials

- Install okteto CLI for Mac/Linux User :- 
```
curl https://get.okteto.com -sSfL | sh
```
- Windows:

Download (https://downloads.okteto.com/cli/okteto.exe) and add it to your $PATH.

## connect with okteto Cli and okteto namespaces 
```
sangam:~ sangam$ okteto login
Authentication will continue in your default browser
You can also open a browser and navigate to the following address:
---- you will get link for re-direct via web browser 
 ✓  Logged in as sangam14
    Run `okteto namespace` to switch your context and download your Kubernetes credentials.
sangam:~ sangam$ okteto namespace
 ✓  Updated context 'cloud_okteto_com' in '/Users/sangam/.kube/config'
sangam:~ sangam$ 
```
Now your developer environment connected with CLI 

## Visit to UI okteto Cloud Dashboard  :- (https://cloud.okteto.com/)

## To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace sangam14 redis -o jsonpath="{.data.redis-password}" | base64 --decode)

## To connect to your Redis server:

1. Run a Redis pod that you can use as a client:
```
   kubectl run --namespace sangam14 redis-client --rm --tty -i --restart='Never' \
    --env REDIS_PASSWORD=$REDIS_PASSWORD \
   --image docker.io/bitnami/redis:5.0.5-debian-9-r141 -- bash
```
2. Connect using the Redis CLI:
```
   redis-cli -h redis -a $REDIS_PASSWORD
```
3. To connect to your database from outside the cluster execute the following commands:
```
    kubectl port-forward --namespace sangam14 svc/redis 6379:6379 &
    redis-cli -h 127.0.0.1 -p 6379 -a $REDIS_PASSWORD
```
