# vault-raft-snapshot-agent

![Version: 0.1.1](https://img.shields.io/badge/Version-0.1.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.4.1](https://img.shields.io/badge/AppVersion-v0.4.1-informational?style=flat-square)

vault-raft-snapshot-agent takes periodic snapshots of vault's raft database and writes it to a local file or an S3 bucket

**Homepage:** <https://github.com/Argelbargel/vault-raft-snapshot-agent>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| config | object | `{"addr":"http://127.0.0.1:8200","frequency":"1h","k8s_auth_path":"kubernetes","k8s_auth_role":"vault-raft-snapshot-agent","local_storage":{"enabled":true,"volume":{"emptyDir":{}}},"retain":72,"vault_auth_method":"k8s"}` | defines the contents of the configuration-file for vault-raft-snapshot-agent    except for `local_storage` the keys and values are the same as in the agents    [configuration file](https://github.com/Argelbargel/vault-raft-snapshot-agent) |
| config.local_storage.enabled | bool | `true` | enables/disables the local storage of snaphots    if disabled the corresponding volume and volume-mounts will not be created |
| config.local_storage.volume | object | `{"emptyDir":{}}` | defines the kind of volume used to store the snapshots locally    If you specify `persistentVolumeClaim` the chart can generate the    PVC for you. Just specify the claim as you would [normally do](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)    and add the property `create: true` and the relevant properties of your [PersistentVolumeClaimSpec]()    as key of `persistentVolumeClaim`. |
| deployment.image.pullPolicy | string | `"IfNotPresent"` | new releases of vault-raft-snapshot-agent always change the    `.Chart.AppVersion` of this chart thus must only be changed    if you use another repository than above |
| deployment.image.repo | string | `"ghcr.io/argelbargel/vault-raft-snapshot-agent"` | image that is deployed (change e.q. for private registry-proxy) |
| deployment.image.tag | string | `.Chart.AppVersion` | the image's tag |
| deployment.revisionHistoryLimit | int | `nil` | see [kubernetes docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#clean-up-policy)    You might want to change this to a small value to avoid cluttering up the    UI of a Continuous Delivery Tool like Argo-CD |
| deployment.strategy.type | string | `"Recreate"` | Update-strategy for the agent's pods    `Recreate` guarantees that no two snapshots get taken at the same time    `RollingUpdate` ensures that there's always one instance of the agent running |