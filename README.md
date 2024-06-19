# Cilium on Kind

Testing cilium CNI on kind k8s cluster

```sh
kind create cluster --config kind-config.yaml
```

## Download docker images
```sh
docker pull quay.io/cilium/cilium:v1.15.5
docker pull quay.io/cilium/operator-generic:v1.15.5
kind load docker-image quay.io/cilium/cilium:v1.15.5
kind load docker-image quay.io/cilium/operator-generic:v1.15.5
```

# Install Cilium via helm
```sh
helm upgrade --install --namespace kube-system \
  --repo https://helm.cilium.io cilium cilium \
  --values values.yaml
```

# Install ingress-nginx
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

# Install cilium cli (optional)

```sh
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
CLI_ARCH=amd64
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
```


# References
- https://docs.cilium.io/en/latest/installation/kind/
- https://docs.cilium.io/en/stable/network/egress-gateway/
- https://medium.com/@charled.breteche/kind-cluster-with-cilium-and-no-kube-proxy-c6f4d84b5a9d
- https://github.com/cilium/cilium/tree/v1.15.5/install/kubernetes/cilium