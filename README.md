sudo nano /etc/containerd/config.toml


 [plugins.'io.containerd.cri.v1.images'.registry]
   config_path = "/etc/containerd/certs.d"



sudo mkdir -p /etc/containerd/certs.d/<ip address>

sudo nano /etc/containerd/certs.d/<ip address>/hosts.toml


server = "http://<ip address>"

[host."http://<ip address>"]
  capabilities = ["pull", "resolve"]

sudo systemctl restart containerd
