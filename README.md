# Diving  into the future of Infrastructure
Pod-101
# Container Definition:
Uses the "kuard-amd64:blue" image.
Exposes port 8080 for HTTP traffic.
# Resource Requests and Limits:
Requests 64 megabytes of memory and 250 milliCPU.
Limits the container to 128 megabytes of memory and 500 milliCPU.
# Health Checks:
Readiness probe checks the "/ready" endpoint, starting 5 seconds after the container starts and running every 10 seconds.
Liveness probe checks the "/healthz" endpoint, starting 10 seconds after the container starts and running every 20 seconds.
# Persistent Volume Usage:
Defines a Persistent Volume Claim (PVC) named "kuard-data" requesting 1Gi of storage.
Mounts the PVC to the path "/data" inside the container.
