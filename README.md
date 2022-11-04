# Chainlink Terraform

## Chainlink Helm Chart

### Required Values

The required values are mostly the Chainlink environment variables described in their [documentation](https://docs.chain.link/docs/configuration-variables/).

| Name                         | Chainlink Environment Variable | Type   | Required | Default Value |
|------------------------------|--------------------------------|--------|----------|---------------|
| `explorer_access_key`        | EXPLORER_ACCESS_KEY            | string | True     | ""            |
| `explorer_secret`            | EXPLORER_URL                   | string | True     | ""            |
| `root`                       | ROOT                           | string | True     | "/chainlink"  |
| `log_level`                  | LOG_LEVEL                      | string | True     | "debug"       |
| `eth_chain_id`               | ETH_CHAIN_ID                   | string | True     | "4"           |
| `chainlink_tls_port`         | CHAINLINK_TLS_PORT             | string | True     | "0"           |
| `secure_cookies`             | SECURE_COOKIES                 | string | True     | "false"       |
| `allow_origins`              | ALLOW_ORIGINS                  | string | True     | "*"           |
| `eth_url`                    | ETH_URL                        | string | True     | ""            |
| `database_locking_mode`      | DATABASE_LOCKING_MODE          | string | True     | "lease"       |
| `p2p_bootstrap_peers`        | P2P_BOOTSTRAP_PEERS            | string | True     | ""            |
| `feature_offchain_reporting` | FEATURE_OFFCHAIN_REPORTING     | string | True     | "true"        |
| `chainlink_dev`              | CHAINLINK_DEV                  | string | True     | "true"        |
| `p2p`                        | see below                      | object | True     | see below     |

The p2p key is an object with the following default value:
```yaml
  p2p:
    enabled: true
    port: 10000
    elastic_ip_alloc_id: ''
    public_ip: ''
```

`p2p.enabled` isn't a Chainlink environment variable, but it allows you to choose whether you need the following **P2P** environment variables enabled or not.
If `p2p.enabled` is true the following environment variables will be set:
- **P2P_LISTEN_PORT** (`p2p.port`)
- **P2P_ANNOUNCE_PORT** (`p2p.port`)
- **P2P_ANNOUNCE_IP** (`p2p.public_ip`)

`p2p.elastic_ip_alloc_id` will always be used for the AWS Load Balancer EIP allocations (see [Chainlink Service](./chainlink/templates/service.yaml)).
