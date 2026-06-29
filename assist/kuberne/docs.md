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
