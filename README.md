### Lighthouse Helm Chart

#### Installation

```
helm install --namespace blockchain lighthouse ./helm-lighthouse
```

#### Chart Configuration

Args

```
args:
  - lighthouse
  - beacon_node
  - --network=mainnet
  - --datadir=/data
  - --http
  - --http-address=0.0.0.0
  - --http-port=5052
  - --execution-endpoint=http://plex-geth:8551
  - --execution-jwt=/jwt/jwt.hex
  - --checkpoint-sync-url=https://mainnet-checkpoint-sync.attestant.io
  - --metrics
  - --metrics-address=0.0.0.0
  - --metrics-port=5054
  
```

Ports

```
ports:
- name: rest
  port: 5052
  targetPort: rest
  protocol: TCP
- name: metrics
  port: 5054
  targetPort: metrics
  protocol: TCP
- name: p2p-tcp
  port: 9000
  targetPort: p2p-tcp
  protocol: TCP
- name: p2p-udp
  port: 9000
  targetPort: p2p-udp
  protocol: UDP
```

Volumes


```
# PVC for lighthouse data persistence

persistence:
  enabled: true
  size: 1000Gi
  storageClass:
  accessModes:
    - ReadWriteOnce
  mountPath: /data
  name: lighthouse-data

# Geth JWT Token

volumeMounts:
  - name: jwt
    mountPath: /jwt
    readOnly: true

volumes:
  - name: jwt
    secret:
      secretName: geth-jwt
      items:
        - key: jwt.hex
          path: jwt.hex

```
