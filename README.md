# NetPilot — Private Network Service Platform

A fully functional private network infrastructure built on 4 macOS machines.

## Architecture
- **Mac1** (10.7.26.16) — Primary DNS (dnsmasq) + test client
- **Mac2** (10.7.31.188) — Edge: nginx reverse proxy, load balancer, TLS termination, Certificate Authority
- **Mac3** (10.7.19.137) — Backend A (port 3001)
- **Mac4** (10.7.8.65) — Backend B (port 3002) + backup DNS + main test client

## How to Run

### Mac1 — Start DNS
```bash
sudo brew services start dnsmasq
```

### Mac2 — Start nginx edge
```bash
sudo brew services start nginx
```

### Mac3 — Start Backend A
```bash
cd ~/netproj/backend && BACKEND=A PORT=3001 python3 server.py
```

### Mac4 — Start Backend B
```bash
cd ~/netproj/backend && BACKEND=B PORT=3002 python3 server.py
```

## Team Members
- Member 1 — Mac1 (DNS)
- Member 2 — Mac2 (Edge/CA)
- Member 3 — Mac3 (Backend A)
- Member 4 — Mac4 (Backend B)
