# NetPilot — Private Network Service Platform

A fully functional private network infrastructure built on 4 macOS machines,
simulating real-world cloud architecture with DNS, TLS, load balancing, and caching.

## Architecture

| Mac | IP | Role |
|-----|----|------|
| Mac1 | 10.7.26.16 | Primary DNS (dnsmasq) + test client |
| Mac2 | 10.7.31.188 | Edge: nginx reverse proxy, load balancer, TLS, Certificate Authority |
| Mac3 | 10.7.19.137 | Backend A (port 3001) |
| Mac4 | 10.7.8.65 | Backend B (port 3002) + backup DNS + main test client |

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
cd ~/NetPilot/backend && BACKEND=A PORT=3001 python3 server.py
```

### Mac4 — Start Backend B
```bash
cd ~/NetPilot/backend && BACKEND=B PORT=3002 python3 server.py
```

## Team Members

| Member | Name | Role | Mac | GitHub |
|--------|------|------|-----|--------|
| 1 | Mausam Kumar Dwivedi | Primary DNS + test client | Mac1 | [Mausam](https://github.com/mausam-005) |
| 2 | Krish Dabas | Edge: nginx + TLS + CA | Mac2 | [Krish](https://github.com/krish-2509) |
| 3 | Daniel Tayal | Backend A (port 3001) | Mac3 | [Daniel](https://github.com/danieltayal07) |
| 4 | Ritik Atri | Backend B (port 3002) + backup DNS | Mac4 | [Ritik](https://github.com/ritikattri773) |
