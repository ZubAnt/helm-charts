# install from stable/prometheus


```bash
helm install stable/prometheus \
--name prometheus \
--set server.persistentVolume.existingClaim=prometheus-server-pvc,\
server.ingress.enabled=true,server.ingress.hosts={prometheus.tic-dev.xyz}

```


