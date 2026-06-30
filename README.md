sudo nano /etc/containerd/config.toml


 [plugins.'io.containerd.cri.v1.images'.registry]
   config_path = "/etc/containerd/certs.d"


sudo mkdir -p /etc/containerd/certs.d/<ip address>

sudo nano /etc/containerd/certs.d/<ip address>/hosts.toml


server = "http://<ip address>"

[host."http://<ip address>"]
  capabilities = ["pull", "resolve"]

sudo systemctl restart containerd

--------------------------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: crm-gateway-service
  labels:
    app: gateway
spec:
  replicas: 3
  selector:
    matchLabels:
      app: gateway
  template:
    metadata:
      name: crm-gateway-service
      labels: 
        app: gateway
    spec:
      imagePullSecrets:
        - name: harbor-cred
      containers:
      - name: gateway-service
        image: 13.233.236.101/crm/gateway:latest
        imagePullPolicy: Always

-------------------------------------------------
apiVersion: v1
kind: Service
metadata:
  name: gateway-service
spec:
  type: NodePort
  selector:
    app: gateway
  ports:
    - port: 80
      targetPort: 2000
      nodePort: 30000

-----------------------------------------------------
kubectl create secret docker-registry harbor-cred \
  --docker-server=<ip_address> \
  --docker-username=admin \
  --docker-password=admin
 
