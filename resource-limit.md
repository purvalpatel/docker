docker-compose.yaml
```
version: '3.8'

services:
  ss95224v_service:
    image: gezp/ubuntu-desktop:22.04-cu12.4.1
    environment:
      - USER=xxxxxx
      - PASSWORD=Ixcdsfsd
      - GID=1000
      - UID=1000
      - DOCKER_ALLOW_IPV6_ON_IPV4_INTERFACE=1
    container_name: sxxxxx_machine
    hostname: xxxxxx_hostname
    cap_add:
      - SYS_ADMIN
      - NET_ADMIN
      - MKNOD
      - SYS_PTRACE
      - AUDIT_WRITE
      - SYS_RESOURCE
      - SYS_NICE
    security_opt:
      - apparmor=unconfined
      - seccomp=unconfined


    # ── CPU limits ─────────────────────────────────────────────
    cpus: "100"           # max 8 logical CPU cores
 
    deploy:
      resources:
        limits:
          cpus: "100"
          memory: 150G
        reservations:
          cpus: "80"
          memory: 100G
          devices:
            - driver: nvidia
              capabilities: [gpu]
              device_ids: ["3"]

    ports:
      - "28572:22"    # SSH Port

    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
      - sxxxx:/home/  # Volume for Home
      - sxxxx_etc:/etc/  # Volume for etc
      - sxxxx_var:/var/  # Volume for var
      - sxxxx_usr:/usr/  # Volume for usr

    networks:
      - shared-network-dind

    # Uncomment the line below if you want the service to restart automatically
    # restart: unless-stopped

    healthcheck:
      test: ["CMD-SHELL", "nvidia-smi > /tmp/nvidia-smi-health.log 2>&1 || exit 1"]
      interval: 300s
      timeout: 50s
      retries: 5
      start_period: 100s

    logging:
      driver: "json-file"
      options:
        max-size: "100m"
        max-file: "5"

volumes:
  xxxx:
  sxxxx_var:
  sxxxx_etc:
  sxxxx_usr:

networks:
  shared-network-dind:
    external: true
    name: shared-network-dind

```

### Verify the limits are assigned to container or not ?
```
docker inspect xxxxxx_machine | grep -E "NanoCpus|Memory\""
docker stats xxxxxx_machine --no-stream
```
CPUs are showing in nonoCPUs.
