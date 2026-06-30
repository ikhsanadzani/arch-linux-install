# langkah awalnya 
## mengsinkronkan waktu
```
sudo timedatectl set-ntp true
```
> mengaktifkan sinkronisasi waktu otomatis
```
sudo timedatectl set-timezone Asia/Jakarta
```
> mengubah zona waktu (timezone) sistem Anda ke Waktu Indonesia Barat (WIB
```
sudo timedatectl
```
> cek statusnya

## region control
lewat admin kita ssh region control untuk dijadikan control-plane
```
curl -sfL https://get.k3s.io | sh -s server --disable traefik
```
> langsung install dari script resminya

```
sudo cat /var/lib/rancher/k3s/server/node-token 
```
> untuk cek token yang akan digunakan
> contoh output:
```
K1038fb91da987e64ac1d98b948bc00520b51f8ef4f1dca73ea14a2391a4c1fb76c::server:e08f645e38888f1dfab0766a250aceee
```

## region data 
> harus dari laptop admin di region control
```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 
```
```
sudo kubectl label node [hostname_server] role=[rolenya]
```

## region internal 
> harus dari laptop admin di region control
```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 
```
```
sudo kubectl label node [hostname_server] role=[rolenya]
```

## region public 
> harus dari laptop admin di region control
```
curl -sfL https://get.k3s.io | K3S_URL="https://ip_server:6443" K3S_TOKEN="PASTE_TOKEN_DARI_SERVER" sh -s - agent 
```
```
sudo kubectl label node [hostname_server] role=[rolenya]
```

## cek status region
> harus dari laptop admin diregion control
```
 sudo k3s kubectl get nodes   
```


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8ab9d290-21a7-49e9-95f0-5219a71236e6" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e86cfe67-13fc-42af-ac2c-31368890de97" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6af15e86-fd16-4ce6-b6ed-b727dcaf326d" />

