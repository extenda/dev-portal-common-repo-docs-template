# Runbook for identity-and-access-management

## General

[README](./internal/README.md)

## Architecture

[Architecture](./internal/README.md)

## Business Continuity and Disaster Recovery Plan

[SRE](./internal/SRE.md)

## Services

All IAM services support `Correlation-Id` & `Tenant-Id` in logs.

### iam-ui

IAM UI

[Github repository](https://github.com/extenda/hiiretail-engineering-iam-ui-new)

### iam-api

IAM Rest API

[Github repository](https://github.com/extenda/hiiretail-engineering-iam-api)

[Logs production](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-api/iam-api/logs?project=engineering-prod-5e3f)

[Logs staging](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-api/iam-api/logs?project=engineering-staging-8ec1)

### iam-sync-worker

[Github repository](https://github.com/extenda/hiiretail-engineering-iam-sync-worker)

[Logs production](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-sync-worker/iam-sync-worker/logs?project=engineering-prod-5e3f)

[Logs staging](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-sync-worker/iam-sync-worker/logs?project=engineering-staging-8ec1)

### iam-das-sync-worker

[Github repository](https://github.com/extenda/hiiretail-engineering-iam-das-sync-worker)

[Logs production](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-das-sync-worker/iam-das-sync-worker/logs?project=engineering-prod-5e3f)

[Logs staging](https://console.cloud.google.com/anthos/run/detail/cluster/europe-west1/k8s-cluster/iam-das-sync-worker/iam-das-sync-worker/logs?project=engineering-staging-8ec1)

### sync-functions

We have a couple of functions that live in mono-repo

[Github repository](https://github.com/extenda/hiiretail-engineering-iam-sync-function)

Locations: 
- [prod shoppers](https://console.cloud.google.com/functions/list?env=gen1&project=hiidentity-shoppers)
- [prod staff](https://console.cloud.google.com/functions/list?env=gen1&project=hiidentity-staff)
- [staging shoppers](https://console.cloud.google.com/functions/list?env=gen1&project=hiidentity-shoppers-staging)
- [staging staff](https://console.cloud.google.com/functions/list?env=gen1&project=hiidentity-staff-staging)

## Dashboard

[IAM Monitoring dashboard](https://console.cloud.google.com/monitoring/dashboards/builder/010207f1-2548-440f-b630-6ad90b8462cc?project=engineering-prod-5e3f&dashboardBuilderState=%257B%2522editModeEnabled%2522:false%257D&timeDomain=1h)

## Service Level Objectives

[SLO](./internal/SRE.md#SLO)

## Alerts

### Production

- [[P1] iam - failed cloud function execution](https://console.cloud.google.com/monitoring/alerting/policies/4120286768099657257?project=engineering-prod-5e3f)
- [[P1] iam - error logs above 5 last min](https://console.cloud.google.com/monitoring/alerting/policies/8499721844428083102?project=engineering-prod-5e3f)
- [[P1] iam - cloud run 5xx](https://console.cloud.google.com/monitoring/alerting/policies/10229589768744303761?project=engineering-prod-5e3f)
- [[P1] iam - oldest unacked message is older than 30 m](https://console.cloud.google.com/monitoring/alerting/policies/16840498693342649951?project=engineering-prod-5e3f)
- [[P1] iam - pubsub message in dlq](https://console.cloud.google.com/monitoring/alerting/policies/15077983622807156309?project=engineering-prod-5e3f)
- [[P2] iam - pubsub push latency above 3 s](https://console.cloud.google.com/monitoring/alerting/policies/6744172967171065197?project=engineering-prod-5e3f)
- [[P2] iam - failed cloud scheduler jobs](https://console.cloud.google.com/monitoring/alerting/policies/621752483381358264?project=engineering-prod-5e3f)
- [[P2] iam - upload styra system dataset latency more than 6 min ](https://console.cloud.google.com/monitoring/alerting/policies/10235727237491441717?project=engineering-prod-5e3f)
- [[P2] iam - cloud run (anthos) 5xx](https://console.cloud.google.com/monitoring/alerting/policies/3079864150620300493?project=engineering-prod-5e3f)
- [[P2] iam - styra das unusual amount of denied decisions](https://console.cloud.google.com/monitoring/alerting/policies/16420261040249396952?project=engineering-prod-5e3f)
- [[P2] iam.hydra-auth - production uptime failure](https://console.cloud.google.com/monitoring/alerting/policies/12284037483711202557?project=engineering-prod-5e3f)
- [[P2] iam.iam-api - production uptime failure](https://console.cloud.google.com/monitoring/alerting/policies/9271639656293007233?project=engineering-prod-5e3f)
- [[P2] iam.iam-ui - production uptime failure](https://console.cloud.google.com/monitoring/alerting/policies/14258000284687716004?project=engineering-prod-5e3f)

## Staging

- [[staging] iam - failed cloud scheduler jobs](https://console.cloud.google.com/monitoring/alerting/policies/6506883447855915797?project=engineering-staging-8ec1)
- [[staging] iam - cloud run (anthos) 5xx](https://console.cloud.google.com/monitoring/alerting/policies/14594656430068746810?project=engineering-staging-8ec1)
- [[staging] iam - pubsub message in dlq](https://console.cloud.google.com/monitoring/alerting/policies/11240766820744596911?project=engineering-staging-8ec1)
- [[staging] iam - oldest unacked message is older than 30 m](https://console.cloud.google.com/monitoring/alerting/policies/15113978084708498908?project=engineering-staging-8ec1)
- [[staging] iam - pubsub push latency above 3 s](https://console.cloud.google.com/monitoring/alerting/policies/16746321163288586612?project=engineering-staging-8ec1)

## Health Checks

- [Uptime iam api](https://console.cloud.google.com/monitoring/uptime/iam-api-production?project=engineering-prod-5e3f)
- [Uptime iam ui](https://console.cloud.google.com/monitoring/uptime/iam-ui-production?project=engineering-prod-5e3f)

## How do I..?

### Helpers sctips

As a development team, we created a couple of helper scripts,
that simplify some routine processes.

> Note: Scripts are not intended to be used by anyone other then dev team.

- [scripts](../../scripts)

## Known Issues

- Anonymous users to not get 'tenant id' claim in the token right after login.
- Uploads of Styra datasets might take some time. we are okay with under 5 minutes.
Everything above is a problem, should be reported to styra

## Contact & Escalation Matrix

In case of emergency, primary contacts are:

| #   | Name               | Role       | E-Mail                                   | Phone         |
| --- | ------------------ | ---------- | ---------------------------------------- | ------------- |
| 1   | Lise Lindalen      | Tribe Lead | lise.lindalen@extendaretail.com          | +4795143157   |
| 2   | Maksym Dovhopoliy  | IAM Lead   | maksym.dovhopoliy@extendaretail.com      | +380505442287 |

In case of less urgent questions, or support, in addition to the ones above, you can contact

| #   | Name               | Role       | E-Mail                                   |
| --- | ------------------ | ---------- | ---------------------------------------- |
| 0   | #hiiretail-iam     | Slack      |                                          |
| 1   | Yaroslav Krutiak   | Dev        | yaroslav.krutiak.ext@extendaretail.com   |
| 2   | Artemii Dovhopolyi | Dev        | artemii.dovhopolyi.ext@extendaretail.com |

