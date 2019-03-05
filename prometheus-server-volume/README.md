# prometheus hostPath volume

### create folder on server
```bash
mkdir -p /mnt/prometheus-server-volume
```

### create local volume on a specific node
```bash
helm install --name prometheus-server-volume --set node=examle-node .
```
