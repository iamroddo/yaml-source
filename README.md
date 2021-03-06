# Concourse webhook
Generate a string of characters that can be used to authenticate the webhook eg `asldfjhsadlkjh`
- GitHub
  - Go to repository `settings/hooks`
    - Secret: `Empty`
    - Payload URL:#
      - https://`ConcourseServer`/api/v1/teams/`TeamName`/pipelines/`PipielineName`/resources/`ResourceName`/check/webhook?webhook_token=`GeneratedString` (example - `https://concourse.netinfra.c.eu-de-1.cloud.sap/api/v1/teams/playground/pipelines/webhook/resources/yaml-source.git/check/webhook?webhook_token=asldfjhsadlkjh`)
- Concourse
  - Add `webhook_token:`& `check_every:`under the resource name
    - webhook_token - use the generated string of characters above
    - check_every - set a long period eg 600h
  - Add `trigger: true` where the resource is used

```YAML
---
resources:
  - name: yaml-source.git
    type: git
    check_every: 600h
    webhook_token: asldfjhsadlkjh
    source:
      uri: https://github.wdf.sap.corp/D069683/yaml-source.git
      branch: master
      skip_ssl_verification: true

jobs:
  - name: lint-yaml
    plan:
      - get: yaml-source.git
        trigger: true
```

# YAML linting via Concourse

Yamllink status badge from https://concourse.netinfra.c.eu-de-1.cloud.sap/api/v1/teams/playground/pipelines/webhook/badge
![yamllint status](https://concourse.netinfra.c.eu-de-1.cloud.sap/api/v1/teams/playground/pipelines/webhook/badge)
