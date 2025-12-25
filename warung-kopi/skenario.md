# Warung Kopi  - Kubernetes Scenario

## Latar Belakang
Warung Kopi adalah aplikasi e-commerce untuk menjual kopi Indonesia. Sistem terdiri dari:
- **Frontend**: Website pelanggan (React)
- **Backend API**: REST API untuk order dan menu (Node.js)
- **Database**: PostgreSQL untuk data transaksi
- **Cache**: Redis untuk session dan cache menu
- **Worker**: Background job untuk notifikasi dan laporan

## Arsitektur
```
                    ┌─────────────┐
                    │   Ingress   │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────▼─────┐ ┌────▼────┐ ┌─────▼─────┐
        │ Frontend  │ │ Backend │ │  Metrics  │
        │ (3 pods)  │ │(2 pods) │ │ (DaemonSet│
        └───────────┘ └────┬────┘ └───────────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────▼─────┐ ┌────▼────┐ ┌─────▼─────┐
        │   Redis   │ │PostgreSQL│ │  Worker   │
        │  (cache)  │ │(database)│ │  (jobs)   │
        └───────────┘ └──────────┘ └───────────┘
```

## Tujuan Pembelajaran

### 1. Namespace & Resource Management
- Isolasi environment dengan Namespace
- Pembatasan resource dengan ResourceQuota dan LimitRange

### 2. Configuration Management
- ConfigMap untuk konfigurasi aplikasi
- Secret untuk kredensial database dan API key

### 3. Storage
- PersistentVolume dan PersistentVolumeClaim untuk database
- StorageClass untuk dynamic provisioning
- EmptyDir untuk shared volume antar container

### 4. Workloads
- Deployment untuk stateless apps (frontend, backend)
- StatefulSet untuk stateful apps (database)
- DaemonSet untuk monitoring di setiap node
- Job untuk migrasi database
- CronJob untuk backup harian

### 5. Networking
- Service: ClusterIP, NodePort, LoadBalancer
- Ingress untuk routing HTTP
- NetworkPolicy untuk keamanan antar pod

### 6. Security
- ServiceAccount untuk identitas pod
- Role dan RoleBinding untuk RBAC
- SecurityContext untuk hardening container

### 7. Scaling & Scheduling
- HorizontalPodAutoscaler untuk auto-scaling
- NodeSelector dan Affinity untuk penempatan pod
- Taints dan Tolerations

### 8. Health & Observability
- Liveness, Readiness, dan Startup Probe
- Resource requests dan limits

## Cara Menggunakan

```bash
# 1. Apply semua konfigurasi
kubectl apply -f konfig.yaml

# 2. Verifikasi
kubectl get all -n warung-kopi

# 3. Akses aplikasi
kubectl port-forward svc/frontend -n warung-kopi 8080:80

# 4. Cleanup
kubectl delete -f konfig.yaml
```

## Urutan Resource dalam konfig.yaml
1. Namespace
2. ResourceQuota & LimitRange
3. ConfigMap
4. Secret
5. StorageClass
6. PersistentVolume
7. PersistentVolumeClaim
8. ServiceAccount
9. Role & RoleBinding
10. NetworkPolicy
11. StatefulSet (PostgreSQL)
12. Deployment (Redis, Backend, Frontend)
13. DaemonSet (Metrics)
14. Job (DB Migration)
15. CronJob (Backup)
16. Service (ClusterIP, NodePort)
17. Ingress
18. HorizontalPodAutoscaler
