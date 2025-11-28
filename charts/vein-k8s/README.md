# vein-k8s

Kubernetes deployment for Vein dedicated server. This chart uses [ghcr.io/radical-egg/vein-dedicated-server](https://github.com/Radical-Egg/vein-dedicated-server) docker image.


## Usage

```bash
helm repo add radical-egg https://radical-egg.github.io/pineapple-bun/
helm repo update
helm install vein radical-egg/vein-k8s \
	--set VEIN_SERVER_NAME="Eggs Strange World" \
    --set VEIN_SERVER_DESCRIPTION="nollie 360 flips" \
	--set VEIN_SERVER_PASSWORD="secretpass" \
```

## Container Defaults

| Variable                                          | Default                                   |
| --------                                          | -------                                   |
| image.game.repoistory                             | ghcr.io/radical-egg/vein-dedicated-server |
| image.game.tag                                    | latest                                    |
| image.backups.repoistory                          | ghcr.io/radical-egg/vein-dedicated-backup |
| image.backups.tag                                 | latest                                    |
| resources.game.requests                           | { memory: 6Gi }                           |
| resources.game.limits                             | { memory: 12Gi }                          |
| resources.backups.requests                        | { memory: 32Mi }                          |
| resources.backups.limits                          | { memory: 500Mi }                         |

## Storage Defaults

| Variable                                          | Default                               |
| --------                                          | -------                               |
| storage.kind                                      | hostvol                               |
| storage.hostvol.gamefiles                         | /data/vein-gamefiles                  |
| storage.hostvol.backups                           | /data/vein-backupfiles                |
| storage.pvc.gamefiles.size                        | 10Gi                                  |
| storage.pvc.gamefiles.storageClassName            | false                                 |
| storage.pvc.backups.size                          | 1Gi                                   |
| storage.pvc.backups.storageClassName              | false                                 |

## Networking Defaults

| Variable                                          | Default                               |
| --------                                          | -------                               |
| networking.type                                   | LoadBalancer                          |
| networking.nodePort                               | false                                 |

## Environment Variables

The environment variables mirror that of the docker image. See [this repo](ghcr.io/radical-egg/vein-dedicated-server) for descriptions if needed.

| Variable                                          | Default                               |
| --------                                          | -------                               |
| VEIN_GAME_PORT                                    | 7777                                  |
| VEIN_QUERY_PORT                                   | 27015                                 |
| PUID                                              | 1000                                  |
| PGID                                              | 1000                                  |
| VEIN_SERVER_NAME                                  | "Vein Dedicated Server K8s"           |
| VEIN_SERVER_INSTALL_DIR                           | /home/vein/server                     |
| VEIN_SERVER_PASSWORD                              | changeme                              |
| VEIN_SERVER_DESCRIPTION                           | "Vein Dedicated server in K8s"        |
| VEIN_SERVER_AUTO_UPDATE                           | true                                  |
| VEIN_SERVER_PUBLIC                                | true                                  |
| VEIN_SERVER_HEARTBEAT_INTERVAL                    | "5.0"                                 |
| VEIN_SERVER_MAX_PLAYERS                           | 16                                    |
| VEIN_SERVER_ADMIN_STEAM_IDS                       | false                                 |
| VEIN_SERVER_SUPER_ADMIN_STEAM_IDS                 | false                                 |
| VEIN_SERVER_WHITELISTED_PLAYERS                   | false                                 |
| VEIN_SERVER_VAC_ENABLED                           | 0                                     |
| VEIN_EXTRA_ARGS                                   | ""                                    |
| VEIN_SERVER_BACKUP_SRC_DIR                        | /data                                 |
| VEIN_SERVER_BACKUP_DIR                            | /backup                               |
| VEIN_SERVER_BACKUP_RETENTION                      | 5                                     |
| VEIN_SERVER_BACKUP_INTERVAL_SECONDS               | 3600                                  |
