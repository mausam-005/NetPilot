# NetPilot — Private Network Service Platform

A fully functional private network infrastructure built on 4 macOS machines,
simulating real-world cloud architecture with DNS, TLS, load balancing, and caching.

## Architecture

| Mac | IP | Role |
|-----|----|------|
| Mac1 | 10.7.26.16 | Primary DNS (dnsmasq) + test client |
| Mac2 | 10.7.31.188 | Edge: nginx reverse proxy, load balancer, TLS, Certificate Authority |
| Mac3 | 10.7.6.148 | Backend A (port 3001) |
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

## Testing

Run these from any Mac on the LAN. Mac2 (Edge) must have nginx running, and Mac1 must have dnsmasq running.

| Mac | Role | IP |
|-----|------|----|
| Mac1 | Primary DNS (dnsmasq) | `10.7.26.16` |
| Mac2 | Nginx Edge | `10.7.31.188` |
| Mac3 | Backend A | `10.7.6.148` |
| Mac4 | Backend B | `10.7.8.65` |

### 1. DNS resolution
```bash
dig app.team1.test +short
```
Expected: the Edge IP, `10.7.31.188`.

### 2. HTTPS
```bash
curl -I https://app.team1.test
```
Expected: `HTTP/2 200` with no certificate errors.

### 3. Load balancing
```bash
for i in {1..6}; do curl -s https://app.team1.test/api/status; echo; done
```
Expected: responses alternate between backend A and backend B.

### 4. HTTP caching
```bash
curl -sI https://app.team1.test/api/static | grep -iE "cache|etag|x-backend|x-edge"
```
Expected: `cache-control: max-age=60` and an `etag` header.

### 5. Failure demonstration
1. Stop one backend (for example `Ctrl+C` on the server on Mac3 or Mac4).
2. Re-run the load-balancing loop:
   ```bash
   for i in {1..6}; do curl -s https://app.team1.test/api/status; echo; done
   ```
   Expected: every response comes from the surviving backend, with no errors.
3. Restart the stopped backend and run the loop again. Both backends should respond again.
