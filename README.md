# How to not lose your mind:

Steps to connect the `Tailscale Client` to the `Headscale Server`:

1. Install the `Helm Chart` on the `k8s` cluster

   - Make sure the `Service` is created as a `NodePort`

2. Find the `Node`'s external IP via:

```cmd
kubectl get nodes -o yaml | grep address:
```

3. Create a namespace in the `Headscale` server:
```cmd
kubectl exec -it <pod name> -- headscale namespaces create <namespace name>
```

4. Generate an auth token in the `Headscale` server:
```cmd
kubectl exec -it <pod name> -- headscale --namespace <namespace name> preauthkeys create --reusable --expiration 24h
```

5. Connect from the client via the command:

```cmd
tailscale up --auth-key=<key> --hostname=<Node IP> --login-server=http://<Node IP>:<NodePort>
```
