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
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server" sh -s - --flannel-backend none --token 12345
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
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8ab9d290-21a7-49e9-95f0-5219a71236e6" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e86cfe67-13fc-42af-ac2c-31368890de97" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6af15e86-fd16-4ce6-b6ed-b727dcaf326d" />

