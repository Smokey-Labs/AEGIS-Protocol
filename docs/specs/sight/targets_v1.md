# AEGIS Target Types v1

Targets are strings. Many targets use a prefix convention.

## Filesystem
- file:/absolute/path
- dir:/absolute/path
- glob:/absolute/path/pattern*

Examples:
- file:/etc/aegis/kernel.json
- dir:/etc/systemd/system
- glob:/etc/aegis/contracts/*.yml

## Systemd / Journald
- unit:<name>.service
- unit:<name>.timer
- journal:unit:<name>.service
- journal:unit:<name>.timer
- journal:boot
- boot:current
- boot:previous

Examples:
- unit:ollama.service
- journal:unit:ollama.service
- journal:boot

## Processes
- pid:<number>
- proc:<name>
- cgroup:/system.slice/<unit>.service

Examples:
- pid:4254
- proc:ollama
- cgroup:/system.slice/ollama.service

## Network
- listen
- port:<tcp|udp>:<number>
- iface:<name>
- route
- dns
- host:<ip-or-name>:<port>

Examples:
- listen
- port:tcp:11434
- iface:enp3s0
- host:192.168.0.121:11434

## Storage
- mounts
- fs:<mountpoint>
- disk:<device>
- smart:<device>

Examples:
- mounts
- fs:/
- disk:/dev/sda
- smart:/dev/sda

## Packages / Versions
- pkg:apt
- pkg:<name>
- kernel
- os:release

Examples:
- pkg:apt
- pkg:ollama
- kernel

## Docker (if present)
- docker:ps
- docker:container:<name>
- docker:image:<ref>
- docker:logs:<name>

Examples:
- docker:ps
- docker:container:open-webui
- docker:logs:open-webui

## AEGIS Internals
- aegis:ledger
- aegis:host:<name>
- aegis:event:<id>
- aegis:contracts
- aegis:kernel

Examples:
- aegis:ledger
- aegis:kernel


Targets make operator intent unambiguous and machine-verifiable.

Examples:
- `file:/etc/aegis/kernel.json`
- `unit:ollama.service`
- `journal:unit:ollama.service`
- `port:tcp:11434`
- `docker:container:open-webui`

---

## Enforcement Rules

Human-readable enforcement logic lives in:

