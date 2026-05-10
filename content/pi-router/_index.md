---
title: "pi-router"
date: 2026-05-10
draft: false
showtoc: false
ShowRssButtonInSectionTermList: false
---

<style>
.post-content img {
  display: block;
  width: 100%;
  max-width: 591px;
  height: auto;
  margin: 1.5rem auto;
}
</style>

pi-router is an Ansible project that turns a Raspberry Pi 5 into a small, self-managing LTE gateway. The Pi takes its uplink from a USB LTE modem, hands out internet to anything plugged into its Ethernet port, and stays reachable for remote SSH through Twingate. A UPS HAT on top keeps it running through power cuts and brings it back on its own when mains power returns.

The whole thing is driven from a single playbook. Bootstrap a fresh Pi, drop in a `secrets.yml`, run the playbook, and the device comes up configured the same way every time.

## What it does

- Brings up a Waveshare A7670E LTE modem in RNDIS mode so it shows up as a USB Ethernet interface, with NetworkManager handling the connection. ModemManager is deliberately blocked — control happens through direct AT commands on `/dev/ttyUSB1`.
- Ships a `gsm` command — a small terminal UI that reads the modem in real time and shows RSSI, dBm, signal-quality label, operator, band, and a running best/worst tracker. I use it to find the best spot for the antenna: walk around with the Pi (or move just the antenna on its cable), watch the numbers climb, and bolt it where the readings settle highest.

![gsm TUI showing signal strength, level, best/worst tracker, network, status and band](/pi-router/gsm.png)

- Configures `eth0` as a LAN gateway on `192.168.10.1/24`, runs dnsmasq for DHCP, and uses nftables for NAT so any device behind the Pi reaches the internet through the LTE link.
- Installs the Twingate connector and pins it to the LTE interface, with a loopback alias so the Pi itself is reachable for SSH from anywhere without exposing a public port.
- Monitors a Waveshare UPS HAT B over I2C every 30 seconds. It tracks bus voltage and current, infers state of charge, and posts events to Slack when AC drops, when AC returns, and when the battery crosses 50/25/10 percent. A `battery` command prints the current state on demand.

![battery command output showing voltage, current, source AC and charging status](/pi-router/battery.png)

![Slack messages from the device: AC off at 12:00 with battery state, then AC on a minute later showing it has resumed charging](/pi-router/slack-ac.png)

- Halts the Pi cleanly at 25 percent battery, sets an RTC wake alarm, and lets a boot-check service decide on the next power-on whether to halt again or come up fully — so the device auto-recovers when mains power returns.
- Runs an Ookla speedtest on a schedule via Docker, logs every result to JSONL, and sends a daily summary to Slack with min/avg/max for download, upload, and ping. A `speed` command runs an ad-hoc test and prints just the download and upload figures.

![speed command output showing download and upload throughput with data used](/pi-router/speed.png)

![Two daily speedtest summaries posted to Slack on consecutive days, showing min/avg/max for download, upload and ping](/pi-router/slack-speedtest.png)

## How it's organised

The playbook is split into roles, each independently runnable with `--tags`:

- `slack_notify` — shared notification helper used by every other role.
- `waveshare_ups` — INA219 reader, systemd timer, shutdown logic, boot-check guard, and the `battery` CLI.
- `modem` — udev rules, RNDIS init script, NetworkManager keyfile, and the `gsm` TUI for live signal monitoring.
- `twingate_connector` — apt install, config, loopback alias service.
- `docker` — Docker CE from the official apt repository.
- `speedtest` — scheduled tests, JSONL logging, daily Slack report, and the `speed` CLI for ad-hoc runs.
- `router` — eth0 static IP, dnsmasq, nftables NAT, IPv4 forwarding. Run last, after Twingate SSH has been confirmed working — it changes how `eth0` behaves and you lose direct cabled SSH from a laptop.

## Why it exists

I needed a router for a garage that has no fixed-line internet and unreliable mains power. A Raspberry Pi with an LTE HAT and a battery HAT covers both problems for less than the cost of a commercial 4G router, and the result is a single repo I can re-deploy onto a new Pi in a few minutes if anything goes wrong.

## Source code

pi-router is open source. The full source — playbook, roles, templates, and the Python scripts for the UPS monitor and signal TUI — lives at [github.com/andrzejsiemion/pi-router](https://github.com/andrzejsiemion/pi-router).
