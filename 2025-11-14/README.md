
## What Is a Container?

A Linux process with:

its own isolated filesystem

isolated network

isolated PID namespace

NOT a VM.

## Why Containers Are Ephemeral

Container = base image layers + temporary writable layer.

When the container stops → writable layer deleted.

## Container = Process on Host

Example:

docker run nginx


Creates:

containerd → shim → runc → nginx

nginx becomes the container’s PID 1.

## Isolation = Namespaces + Cgroups
### Namespaces provide:

PID isolation

Network isolation

Filesystem isolation

Hostname (UTS) isolation

IPC isolation

User isolation

### Cgroups provide:

CPU limits

RAM limits

I/O limits

## PID Namespace

Inside container:

PID 1 = main app


On host:

process appears as a normal Linux process

## Network Namespace

Container gets:

its own IP

its own routing table

virtual Ethernet pair (veth)

## Mount Namespace

Container has its own / (root filesystem) using overlay layers.

## Why Killing PID 1 Stops Container

Container’s lifetime = PID 1's lifetime.

If the main process exits → container stops.

## Why Containers Start Fast

No hypervisor

No kernel boot

Only one isolated process starts

Startup time: ~50ms.

## VM vs Container
VM

Has its own kernel

Heavyweight

Slow startup

Container

Shares host kernel

Isolated process

Lightweight

## Summary

A container is a process.

Uses namespaces for isolation.

Uses cgroups for resource limits.

Has its own fake filesystem.

Writable layer is temporary.

PID 1 controls container lifecycle.

Containers ≠ VMs.

![alt text](<WhatsApp Image 2025-11-18 at 21.49.44_e97e0c56.jpg>)