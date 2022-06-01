# Package Operator 

## Setup

```sh
kind create cluster --name package-operator

# in the package-operator directory
kubectl create -f config/deploy

# start controller for package handling (blocking)
go run ./cmd/package-operator-manager -metrics-addr=0

# start controller for Handover ... handling (blocking)
go run ./cmd/coordination-operator-manager -metrics-addr=0
```

## Demo

### Part1 - General Concepts

0. Create a window with watches

```sh
kubectl get packagedeployment -w

watch -n1 kubectl get packageset --sort-by=.metadata.creationTimestamp
```

1. Show part1/nginx-v1.yaml
2. Create part1/nginx-v1.yaml
3. Delete pods - show status propagation

```sh
pygmentize part1/nginx-v1.yaml
kubectl create -f part1/nginx-v1.yaml

kubectl delete po --all
```

---

4. Show part1/nginx-v2.yaml
5. Upgrade to part1/nginx-v2.yaml
6. Show PackageSet status of unready
7. Edit cm nginx-v2 - add annotation

```sh
pygmentize part1/nginx-v2.yaml
kubectl apply -f part1/nginx-v2.yaml

kubectl get packageset -o yaml --sort-by=.metadata.creationTimestamp
kubectl edit cm nginx-v2
```

---

7. Upgrade to part1/nginx-v3-broken.yaml
8. Show pods

```sh
kubectl apply -f part1/nginx-v3-broken.yaml
kubectl get po
```

---

9. Upgrade to part1/nginx-v4-missing-dependencies.yaml
10. Show PackageSet and dependency Configuration

```sh
pygmentize part1/nginx-v4-missing-dependencies.yaml
kubectl apply -f part1/nginx-v4-missing-dependencies.yaml
kubectl get packageset -o yaml --sort-by=.metadata.creationTimestamp
```

---

10. Upgrade to part1/nginx-v5.yaml
11. Profit!
12. Show missing Succeeded condition on broken PackageSet iterations

```sh
kubectl apply -f part1/nginx-v5.yaml
```

---

13. Cleanup

```sh
kubectl delete packagedeployment nginx
```

### Part2 - Handover

0. Create watches

```sh
kubectl get clusterpackagedeployment -w
watch -n1 kubectl get clusterhandovers --sort-by=.metadata.creationTimestamp
watch -n1 kubectl get clusterpackageset --sort-by=.metadata.creationTimestamp
```

1. Create part2/packagedeployment-v1.yaml
2. Point out Handover stuff, CRDs, etc

```sh
kubectl create -f part2/packagedeployment-v1.yaml

kubectl get crd | grep nginx
kubectl create -f part2/nginx.yaml
kubectl get nginx
kubectl get nginx -o yaml

pygmentize part2/handover.yaml
```

3. Deploy workload part2/nginx.yaml
4. Watch work nginx-deployments and their version assignment
5. Upgrade to part2/packagedeployment-v2.yaml

```
kubectl apply -f part2/packagedeployment-v2.yaml
```
