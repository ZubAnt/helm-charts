# ElasticSearch hostPath volume

### create folder on server
```bash
mkdir -p /mnt/elasticsearch-volume
```

### set user and group for this path
See [docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
```bash
chown -R 1000:1000 /mnt/elasticsearch-volume
```
 
### add permissions for group
```bash
chmod -R g+w /mnt/elasticsearch-volume
```
