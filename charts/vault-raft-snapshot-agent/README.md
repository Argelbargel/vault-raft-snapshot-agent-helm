# vault-raft-snapshot-agent

![Version: 0.1.4](https://img.shields.io/badge/Version-0.1.4-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.5.2](https://img.shields.io/badge/AppVersion-v0.5.2-informational?style=flat-square)

Vault Raft Snapshot Agent takes periodic snapshots of Vault's Raft database and writes it to a local file or an S3 bucket

**Homepage:** <https://github.com/Argelbargel/vault-raft-snapshot-agent>

## Source Code

* <https://github.com/Argelbargel/vault-raft-snapshot-agent-helm/>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| config | object | `{"addr":"http://127.0.0.1:8200","frequency":"1h","k8s_auth_path":"kubernetes","k8s_auth_role":"vault-raft-snapshot-agent","local_storage":{"enabled":true,"volume":{"emptyDir":{}}},"retain":72,"vault_auth_method":"k8s"}` | Defines the contents of the configuration-file for vault-raft-snapshot-agent.    Except for `local_storage` the keys and values are the same as in the agents    [configuration file](https://github.com/Argelbargel/vault-raft-snapshot-agent) |
| config.addr | string | `"http://127.0.0.1:8200"` | Url to the vault-API on the *leader* of your vault-cluster, e.g. `https?://vault-active.<vault-namespace>.svc.cluster.local:<vault-server service-port>` |
| config.local_storage.enabled | bool | `true` | Enables/disables the local storage of snaphots.    If disabled the corresponding volume and volume-mounts will not be created |
| config.local_storage.volume | object | `{"emptyDir":{}}` | Defines the kind of volume used to store the snapshots locally.    If you specify `persistentVolumeClaim` the chart can generate the    PVC for you. Just specify the claim as you would [normally do](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)    and add the property `create: true` and the relevant properties of your [PersistentVolumeClaimSpec]()    as key of `persistentVolumeClaim`. |
| deployment.image.pullPolicy | string | `"IfNotPresent"` | New releases of vault-raft-snapshot-agent always change the    `.Chart.AppVersion` of this chart thus must only be changed    if you use another repository than above |
| deployment.image.repo | string | `"ghcr.io/argelbargel/vault-raft-snapshot-agent"` | Image that is deployed (change e.q. for private registry-proxy) |
| deployment.image.tag | string | `.Chart.AppVersion` | the image's tag |
| deployment.revisionHistoryLimit | int | `nil` | see [kubernetes docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#clean-up-policy)    You might want to change this to a small value to avoid cluttering up the    UI of a Continuous Delivery Tool like Argo-CD |
| deployment.strategy.type | string | `"Recreate"` | Update-strategy for the agent's pods    `Recreate` guarantees that no two snapshots get taken at the same time    `RollingUpdate` ensures that there's always one instance of the agent running |
| fullnameOverride | string | release-name + chart-name truncated to 63 chars | overrides the generated full-name for generated resources |
| global.namespace | string | `.Release.Namespace` | allows to override the release's namespace |
| nameOverride | string | `.Chart.Name` | overrides the generated name for generated resources |

## License
- Source code is licensed under MIT

## Contributors
- Vault Raft Snapshot Agent was originally developed by [Lucretius on Github](https://github.com/Lucretius/vault_raft_snapshot_agent)
