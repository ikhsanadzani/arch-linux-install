# Firewalld
> It is recommended to turn off firewalld:
```
systemctl disable firewalld --now
```
> If you wish to keep firewalld enabled, by default, the following rules are required:
```
firewall-cmd --permanent --add-port=6443/tcp #apiserver
```
```
firewall-cmd --permanent --zone=trusted --add-source=10.42.0.0/16 #pods
```
```
firewall-cmd --permanent --zone=trusted --add-source=10.43.0.0/16 #services
```
```
firewall-cmd --reload
```


# Install K3s using the official script
```bash
curl -sfL https://get.k3s.io | sudo sh -
```

# Enable and start K3s
```bash
sudo systemctl enable k3s
```
```bash
sudo systemctl start k3s
```

# Check status
```
sudo systemctl status k3s
```

# Set up kubectl access
```
mkdir -p ~/.kube
```
```
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```
```
sudo chown $(id -u):$(id -g) ~/.kube/config
```
```
export KUBECONFIG=~/.kube/config
```
