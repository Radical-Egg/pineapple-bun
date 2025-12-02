# haven-K8s

This is a Helm deployment for the private blogging platform [Haven](https://github.com/havenweb/haven/tree/master). I am not affliated with this application or developers. I just happen to use this application and could not find a helm deployment for it.

## Example install

```bash
helm repo add radical-egg https://radical-egg.github.io/pineapple-bun/
helm repo update
helm install haven \
    --namespace haven \
    radical-egg/haven-k8s \
    -f values.yaml
```

### Example values.yaml

```yaml
haven:
  replicas: 2
  env:
    HAVEN_USER_EMAIL: myemail@mydomain.com
    HAVEN_DB_PASSWORD: verysecretstring
    HAVEN_USER_PASS: verysecretpassword
  ingress:
    enabled: true
    className: traefik
    annotations:
      traefik.ingress.kubernetes.io/router.entrypoints: websecure
    hosts:
      - host: myapp.mydomain.com
        paths:
          - path: /
            pathType: Prefix
    tls:
      - secretName: myapp-tls-secret
        hosts:
          - myapp.mydomain.com
```


## Helm Values
| Key                | Type        | Default | Description                                                          |
| ------------------ | ----------- | ------- | -------------------------------------------------------------------- |
| `fullnameOverride` | string/bool | `false` | Overrides the full name used for all resources.                      |
| `nameOverride`     | string/bool | `false` | Overrides the base name of the chart.                                |

## Haven

### Image and Deployment
| Key                  | Type   | Default | Description                       |
| -------------------- | ------ | ------- | --------------------------------- |
| `haven.replicas`     | int    | `1`     | Number of web replicas.           |
| `haven.nodeSelector` | object | `{}`    | Node selector for Haven web pods. |
| `haven.image.repository` | string | `ghcr.io/havenweb/haven` | Haven application image repo. |
| `haven.image.tag`        | string | `54afa38`                | Haven image tag.              |
| `haven.imagePullPolicy`  | string | `IfNotPresent`           | Image pull policy.            |

### Networking
| Key                     | Type   | Default     | Description            |
| ----------------------- | ------ | ----------- | ---------------------- |
| `haven.networking.type` | string | `ClusterIP` | Service type.          |
| `haven.networking.port` | int    | `3000`      | Haven web server port. |

### Storage
| Key                                  | Type   | Default               | Description                             |
| ------------------------------------ | ------ | --------------------- | --------------------------------------- |
| `haven.storage.kind`                 | string | `hostvol`             | Storage mechanism (host volume).        |
| `haven.storage.hostvol.havenstorage` | string | `/data/haven/storage` | Host path for Haven persistent storage. |
| `haven.storage.tmpfs.dir`            | string | `/tmp/pids`           | Tmpfs mount for PID files.              |
| `haven.storage.tmpfs.sizeLimit`      | string | `32Mi`                | Tmpfs storage size.                     |

### Environment
| Key                           | Type   | Default                   | Description             |
| ----------------------------- | ------ | ------------------------- | ----------------------- |
| `haven.env.PIDFILE`           | string | `/tmp/pids/server.pid`    | Rails PID file path.    |
| `haven.env.RAILS_ENV`         | string | `production`              | Rails environment.      |
| `haven.env.HAVEN_DB_HOST`     | string | `haven-postgresql`        | DB hostname.            |
| `haven.env.HAVEN_DB_NAME`     | string | `haven`                   | DB name.                |
| `haven.env.HAVEN_DB_ROLE`     | string | `haven`                   | DB role/user.           |
| `haven.env.HAVEN_DB_PASSWORD` | string | `supersecretrandomstring` | DB password.            |
| `haven.env.HAVEN_USER_EMAIL`  | string | `changeme@havenweb.org`   | Initial admin email.    |
| `haven.env.HAVEN_USER_PASS`   | string | `ChangeMeN0W`             | Initial admin password. |

### Resources
| Key                               | Type   | Default | Description                 |
| --------------------------------- | ------ | ------- | --------------------------- |
| `haven.resources.requests.cpu`    | string | `100m`  | Minimum CPU reservation.    |
| `haven.resources.requests.memory` | string | `256Mi` | Minimum memory reservation. |
| `haven.resources.limits.cpu`      | string | `500m`  | Maximum CPU allowed.        |
| `haven.resources.limits.memory`   | string | `512Mi` | Maximum memory allowed.     |

### Ingress
| Key                         | Type        | Default | Description                      |
| --------------------------- | ----------- | ------- | -------------------------------- |
| `haven.ingress.enabled`     | bool        | `false` | Enables ingress for Haven.       |
| `haven.ingress.className`   | string/bool | `false` | IngressClass to use.             |
| `haven.ingress.annotations` | object      | `{}`    | Additional ingress annotations.  |
| `haven.ingress.hosts`       | list        | `[]`    | List of ingress hosts and paths. |
| `haven.ingress.tls`         | list        | `[]`    | TLS configuration for hosts.     |


## Postgresql

### Image
| Key                           | Type   | Default              | Description                        |
| ----------------------------- | ------ | -------------------- | ---------------------------------- |
| `postgresql.nodeSelector`     | object | `{}`                 | Node selector for PostgreSQL pods. |
| `postgresql.image.repository` | string | `docker.io/postgres` | PostgreSQL image repository.       |
| `postgresql.image.tag`        | string | `13.2-alpine`        | PostgreSQL image tag.              |
| `postgresql.imagePullPolicy`  | string | `IfNotPresent`       | Image pull policy.                 |

### Storage
| Key                                   | Type   | Default             | Description             |
| ------------------------------------- | ------ | ------------------- | ----------------------- |
| `postgresql.storage.accessModes`      | list   | `["ReadWriteOnce"]` | PVC access mode.        |
| `postgresql.storage.storageClassName` | string | `local-path`        | StorageClass for PVC.   |
| `postgresql.storage.size`             | string | `10Gi`              | Requested storage size. |

### Resources 
| Key                                    | Type   | Default | Description                 |
| -------------------------------------- | ------ | ------- | --------------------------- |
| `postgresql.resources.requests.cpu`    | string | `100m`  | Minimum CPU reservation.    |
| `postgresql.resources.requests.memory` | string | `256Mi` | Minimum memory reservation. |
| `postgresql.resources.limits.cpu`      | string | `500m`  | Maximum CPU allowed.        |
| `postgresql.resources.limits.memory`   | string | `512Mi` | Maximum memory allowed.     |

### Networking
| Key                          | Type   | Default     | Description              |
| ---------------------------- | ------ | ----------- | ------------------------ |
| `postgresql.networking.type` | string | `ClusterIP` | Service type.            |
| `postgresql.networking.port` | int    | `5432`      | PostgreSQL service port. |

### Environment
| Key                                        | Type   | Default | Description                |
| ------------------------------------------ | ------ | ------- | -------------------------- |
| `postgresql.env.POSGRES_USER`              | string | `haven` | PostgreSQL username.       |
| `postgresql.env.POSTGRES_HOST_AUTH_METHOD` | string | `trust` | Auth method (development). |
