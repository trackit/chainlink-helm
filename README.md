# Chainlink Terraform

## Chainlink Helm Chart

### Required Values

The required values are mostly the Chainlink environment variables described in their [documentation](https://docs.chain.link/docs/configuration-variables/).

| Name                         | Chainlink Environment Variable | Type   | Default Value |
|------------------------------|--------------------------------|--------|---------------|
| `explorer_access_key`        | EXPLORER_ACCESS_KEY            | string | ""            |
| `explorer_secret`            | EXPLORER_URL                   | string | ""            |
| `root`                       | ROOT                           | string | "/chainlink"  |
| `log_level`                  | LOG_LEVEL                      | string | "debug"       |
| `eth_chain_id`               | ETH_CHAIN_ID                   | string | "4"           |
| `chainlink_tls_port`         | CHAINLINK_TLS_PORT             | string | "0"           |
| `secure_cookies`             | SECURE_COOKIES                 | string | "false"       |
| `allow_origins`              | ALLOW_ORIGINS                  | string | "*"           |
| `eth_url`                    | ETH_URL                        | string | ""            |
| `database_locking_mode`      | DATABASE_LOCKING_MODE          | string | "lease"       |
| `p2p_bootstrap_peers`        | P2P_BOOTSTRAP_PEERS            | string | ""            |
| `feature_offchain_reporting` | FEATURE_OFFCHAIN_REPORTING     | string | "true"        |
| `chainlink_dev`              | CHAINLINK_DEV                  | string | "true"        |
| `p2p`                        | see below                      | object | see below     |

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

## Chainlink Adapters Helm Chart

### Supported Adapters

- nomics
- coingecko
- coinmarketcap
- bea
- cryptocompare
- jpegd
- tiingo

### Required Values

The required values are mostly the Chainlink Adapters environment variables described [here](https://github.com/DexTrac-Devlin/Chainlink-EA-Manager).

| Name                                      | Chainlink Environment Variable          | Type   | Default Value |
|-------------------------------------------|-----------------------------------------|--------|---------------|
| `ea_port`                                 | EA_PORT                                 | string | see below     |
| `api_key`                                 | API_KEY                                 | string | ""            |
| `timeout`                                 | TIMEOUT                                 | string | 30000         |
| `cache_enabled`                           | CACHE_ENABLED                           | string | "false"       |
| `rate_limit_enabled`                      | RATE_LIMIT_ENABLED                      | string | "true"        |
| `warmup_enabled`                          | WARMUP_ENABLED                          | string | "true"        |
| `rate_limit_api_provider`                 | RATE_LIMIT_API_PROVIDER                 | string | see below     |
| `rate_limit_api_tier`                     | RATE_LIMIT_API_TIER                     | string | see below     |
| `request_coalescing_enabled`              | REQUEST_COALESCING_ENABLED              | string | "true"        |
| `request_coalescing_interval`             | REQUEST_COALESCING_INTERVAL             | string | "100"         |
| `request_coalescing_interval_max`         | REQUEST_COALESCING_INTERVAL_MAX         | string | "1000"        |
| `request_coalescing_interval_coefficient` | REQUEST_COALESCING_INTERVAL_COEFFICIENT | string | "2"           |
| `request_coalescing_entropy_max`          | REQUEST_COALESCING_ENTROPY_MAX          | string | "0"           |
| `warmup_unhealthy_treshold`               | WARMUP_UNHEALTHY_THRESHOLD              | string | "3"           |
| `warmup_subscription_ttl`                 | WARMUP_SUBSCRIPTION_TTL                 | string | "360000"      |
| `log_level`                               | LOG_LEVEL                               | string | "info"        |
| `debug`                                   | DEBUG                                   | string | "false"       |
| `api_verbose`                             | API_VERBOSE                             | string | "false"       |
| `experimental_metrics_enabled`            | EXPERIMENTAL_METRICS_ENABLED            | string | "true"        |
| `metrics_name`                            | METRICS_NAME                            | string | see below     |
| `retry`                                   | RETRY                                   | string | "1"           |

All the above values are created inside the adapter object, e.g. with nomics:

```yaml
nomics:
  config:
    ea_port: 1111
    api_key: ""
    timeout: 30000
    cache_enabled: false
    rate_limit_enabled: true
    warmup_enabled: true
    rate_limit_api_provider: 'nomics'
    rate_limit_api_tier: "https://api.nomics.com/v1"
    request_coalescing_enabled: true
    request_coalescing_interval: 100
    request_coalescing_interval_max: 1000
    request_coalescing_interval_coefficient: 2
    request_coalescing_entropy_max: 0
    warmup_unhealthy_treshold: 3
    warmup_subscription_ttl: 3600000
    log_level: "info"
    debug: false
    api_verbose: false
    experimental_metrics_enabled: true
    metrics_name: nomics
    retry: 1
```

The following keys depend on the provider, you probably won't have to change their values:
- `ea_port`, e.g.: `1111`
- `rate_limit_api_provider`, e.g.: `nomics`
- `rate_limit_api_tier`, e.g.: `https://api.nomics.com/v1`
- `metrics_name`, e.g.: `nomics`
