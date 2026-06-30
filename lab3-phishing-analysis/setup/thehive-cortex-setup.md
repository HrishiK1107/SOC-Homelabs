# TheHive + Cortex Setup

## Overview
TheHive 5.7.3 and Cortex 3.1.8 installed on `soc-core` (Ubuntu Server 22.04),
alongside the existing Wazuh deployment from Lab 1/2. Both installed via
official `.deb` packages from StrangeBee's repository.

---

## Prerequisites

- Existing Wazuh manager + indexer running (occupies Elasticsearch on
  port 9200) — TheHive requires its own Elasticsearch instance, so port
  conflict must be resolved before install
- Java (TheHive and Cortex both require a JVM)
- Cassandra (TheHive's database backend) — fresh install, no prior instance

---

## Step 1 — Resolve the Elasticsearch Port Conflict

Since Wazuh's indexer already occupies port 9200, a separate Elasticsearch
instance for TheHive/Cortex was configured to listen on **port 9201**.

In the new Elasticsearch instance's `elasticsearch.yml`:
```yaml
http.port: 9201
```

This was the single most important config decision in this setup — skipping
it causes the second Elasticsearch instance to silently fail to bind, since
Wazuh's indexer is already holding 9200.

---

## Step 2 — Install Java and Cassandra

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

Cassandra installed fresh via the official Apache Cassandra `.deb`
repository (no prior instance existed on this host):

```bash
echo "deb https://debian.cassandra.apache.org 41x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt update
sudo apt install cassandra -y
sudo systemctl enable cassandra
sudo systemctl start cassandra
```

Verified Cassandra was running before proceeding:
```bash
nodetool status
```

---

## Step 3 — Install TheHive (.deb)

Added StrangeBee's official repository and installed via apt:

```bash
curl -fsSLg https://archive.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.7 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
sudo apt update
sudo apt install thehive -y
```

Configured `/etc/thehive/application.conf` to point at:
- The local Cassandra instance
- Elasticsearch on `localhost:9201` (not the default 9200, to avoid the
  Wazuh conflict noted in Step 1)

Started and enabled the service:
```bash
sudo systemctl enable thehive
sudo systemctl start thehive
```

Confirmed reachable at `http://192.168.234.141:9000`.

---

## Step 4 — Install Cortex (.deb)

Same repository pattern as TheHive:

```bash
sudo apt install cortex -y
```

Configured `/etc/cortex/application.conf` to also use Elasticsearch on
port 9201 (shared instance with TheHive, separate indices).

```bash
sudo systemctl enable cortex
sudo systemctl start cortex
```

Confirmed reachable at `http://192.168.234.141:9001`.

---

## Step 5 — Initial Account Setup

- Logged into TheHive's first-run setup, created the `admin@thehive.local`
  superadmin account
- Logged into Cortex's first-run setup, created the `admin` superadmin
  account

**Known issue:** the Cortex 3.1.8 `analyst` profile has a browser-login
bug in this version — login attempts fail silently. Workaround: all Cortex
operations (analyzer configuration, running jobs) performed via the admin
account directly rather than fixing the analyst login.

---

## Step 6 — Link TheHive to Cortex

In TheHive: **Organisation → Cortex** (or platform-level Cortex
configuration, depending on version) → added a new Cortex server:
Name: Cortex-Lab
URL:  http://127.0.0.1:9001
Key:  <Cortex API key, generated from Cortex admin account>

Confirmed the connector status showed **green** before proceeding to case
work.

---

## Step 7 — Organisation Structure (Lesson Learned)

Initial attempt at case creation failed silently — logging in as
`admin@thehive.local` only ever surfaced the platform-level
`/administration/organisations` panel, with no Cases/Alerts/Observables
navigation available.

**Root cause:** the `admin` organisation in TheHive is reserved for
platform administration (multi-tenancy management) and does not support
case management, regardless of user profile.

**Fix:** created a separate, normal organisation (`soc-lab`) and a new
**Normal**-type user inside it with the `org-admin` profile. Logging in as
this user surfaced the expected Cases/Alerts/Tasks/Observables workspace.

This is worth documenting explicitly since it isn't obvious from TheHive's
UI and cost real setup time — any future TheHive deployment should create
a dedicated working organisation immediately rather than using the default
`admin` org for casework.

---

## Final State

| Service | Port | Status |
|---------|------|--------|
| Wazuh Indexer (Elasticsearch) | 9200 | Running (Lab 1/2) |
| TheHive/Cortex Elasticsearch | 9201 | Running |
| Cassandra | 9042 (default) | Running |
| TheHive | 9000 | Running |
| Cortex | 9001 | Running |

Organisation: `soc-lab` (case management) — separate from `admin`
(platform administration only)

---

## References
- [TheHive Installation Guide](https://docs.strangebee.com/thehive/installation/)
- [Cortex Installation Guide](https://docs.strangebee.com/cortex/installation/)
- [Apache Cassandra Debian Install](https://cassandra.apache.org/doc/latest/cassandra/getting_started/installing.html)