# Workshop 3 — Your system: EDGC

**Emergency Diesel Generator Controller** · Purdue Level 1 · Modbus TCP

This is your brief. Everyone in the class has a different system. Together the eight builds
are the plant from the course architecture diagram.

## Your allocation on the lab host

| | |
|---|---|
| Host | `plant.sanctumsec.com` |
| Role account | `edgc` |
| Your hostname | `edgc.plant.sanctumsec.com` |
| Port block | `5010`–`5019` |
| HMI port | `5010` — served over HTTPS at your hostname |
| Protocol services | `5011` onward — **bind to 127.0.0.1 only** |

### This is a role account, not a personal one

You log in as **`edgc`**, the system, not as yourself. Anyone whose public key is in
that account's `~/.ssh/authorized_keys` can operate this system. Right now that is only you.

Two consequences worth holding onto, because W4 is about exactly this:

- `authorized_keys` **is** an access control list. Granting access is adding a line;
  revoking it is removing one. That is the whole mechanism.
- A shared account costs attribution. The login record says `edgc`, not your name.
  sshd does log the key fingerprint on every login, so who did what is recoverable, but only
  if someone kept a map from fingerprints to people. Notice which half of that is a
  technical control and which half is a filing decision.

You get access once your public key is committed. See `infra/README.md`.

    ssh edgc@plant.sanctumsec.com

To see your own HMI from your laptop before TLS routing is confirmed, tunnel it:

    ssh -N -L 5010:127.0.0.1:5010 edgc@plant.sanctumsec.com

## Your links

Your system connects to these. The links come from the architecture diagram, not from me
inventing them. You need at least one working peer link.

| Peer system | Direction | Link | Address |
|---|---|---|---|
| DCS | bidirectional | serial / hardwired, Modbus | `dcs.plant.sanctumsec.com` |

Peers are named by **system**, not by person. If a peer is down, log the failed attempt and
capture it; a documented failure earns the same credit as a success.

## Bind rules

- **HMI** → your port `5010`, reachable publicly over HTTPS, **basic auth required**.
  An unauthenticated request must return 401.
- **Protocol services** → `127.0.0.1` only. Modbus, DNP3 and OPC-UA authenticate nothing
  and have nowhere to put a credential, so they do not go on a public interface. That gap is
  the finding of the week. W4 is where you design what sits around it.

## What goes in this directory

    workshops/week-03/
      lab/              your server, HMI, systemd units
      points.md         your point map
      protocol-analysis.md
      network-analysis.md
      capture.pcap
      session-log.md

## Events to the OT-SIEM

Your ingest token is in `~/lab/ASSIGNMENT.txt` on the host once your account exists.
The event API spec is linked from the Canvas assignment. At minimum emit `service.state`,
every `protocol.write`, sampled `protocol.read`, `auth.success` / `auth.failure` from your
HMI, and `process.alarm`.

**One requirement worth reading twice:** at least one event you emit must correspond to a
`detection_opportunity` you wrote in your own W2 adversary profile. Say which one in
`network-analysis.md`. In W2 you said what you would detect. This week you make it emit.

Watch the plant at `plant.sanctumsec.com`.
