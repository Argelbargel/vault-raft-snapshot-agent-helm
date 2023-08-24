![![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/vault-raft-snapshot-agent)](https://artifacthub.io/packages/search?repo=vault-raft-snapshot-agent)

# Vault Raft Snapshot Agent Helm-Charts

Helm-Charts for easy deployment of [Vault Raft Snapshot Agent](https://github.com/Argelbargel/vault-raft-snapshot-agent) alongside [Vault](https://github.com/hashicorp/vault-helm) in your kubernetes cluster.

## Charts

## [Vault Raft Snapshot Agent](./charts/vault-raft-snapshot-agent/) 

Installs Vault Raft Snapshot Agent standalone. To install the chart:

```
helm repo add vault-raft-snapshot-agent https://argelbargel.github.io/vault-raft-snapshot-agent-helm/
helm install vault-raft-snapshot-agent/vault-raft-snapshot-agent
```

See [chart-details](./charts/vault-raft-snapshot-agent/) for configuration.

