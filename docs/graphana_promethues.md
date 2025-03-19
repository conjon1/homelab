# Installing Grafana and Prometheus 


So far we have k3s on ubuntu server lts

I want a small prometheus and grafana stack for monitoring and pretty dashboards that 

## installing using helm
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
then 
```
helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace=prometheus-stack --create-namespace
```


Lets test it and make sure everything looks alright

` kubectl --namespace prometheus-stack port-forward $POD_NAME 3000 `


`kubectl --namespace prometheus-stack get pods -l "release=prometheus-stack"`

Then you go type `localhoast:3000` in your browser
And cross your fingers that it works and the Grafana dashboard pops up

This grabs your password
`kubectl --namespace prometheus-stack get secrets prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo`
The default should be 
admin
prom-operator



### The hosts file
So now that everything works we need ot simplify the process a bit because constantly running the kubectl port-forward command on port 3000 with a active terminal open in the background is annoying 

One option is to edit your zshrc or bachrc for a quick action which i will do as well but i want to be even more lazy in the future

What I want: 
  1 the service running all the time
  2 to be able to access it easily on any device on the local network - im not to worried about exposing it yet

There is a magical file named **/etc/hosts** this bad-boy is amazing when locally developing in a personal homelab

```
sudo nano /etc/host
```
and int he file you write somthing like this

<your-K3s-server-ip> grafana.local

the *grafana.local* can be anything you want really

now we need to to make some yamls


This is /homelab/grafana-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
  namespace: prometheus-stack
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30001 # Ch
```
This is /homelab/grafana-ingressroute.yaml
```
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: grafana-ingressroute
  namespace: prometheus-stack
spec:
  entryPoints:
    - web
  routes:
    - match: Host(`grafana.local`) # or your chosen domain
      kind: Rule
      services:
        - name: grafana-service
          port: 3000
```

`kubectl apply -f grafana-service.yaml grafana-ingressroute.yaml`

and boom this should be up
check http://grafana.local and see if its up and going 



