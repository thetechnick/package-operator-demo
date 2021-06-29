# Demo - Part1

1. Create part1/nginx-v1.yaml
2. Delete pods - show status propagation

---

3. Upgrade to part1/nginx-v2.yaml
4. Show PackageSet status of unready
5. Edit cm nginx-v2 - add annotation

---

6. Upgrade to part1/nginx-v3-broken.yaml

---

7. Upgrade to part1/nginx-v4-missing-dependencies.yaml
8. Show PackageSet and dependency Configuration

---

9. Upgrade to part1/nginx-v5.yaml
10. Profit!

# Demo - Part2

1. Create part2/packagedeployment-v1.yaml
2. Point out Handover stuff, CRDs, etc

3. Deploy workload part2/nginx.yaml
4. Watch work nginx-deployments and their version assignment

5. Upgrade to part2/packagedeployment-v2.yaml
