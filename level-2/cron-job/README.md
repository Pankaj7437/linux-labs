# Day 1 – Configure Cron Job on App Servers

## Objective
Install the `cronie` package and configure a cron job for the root user on all application servers.

## Commands

Install cronie

```bash
sudo yum install -y cronie
```

Start and enable cron service

```bash
sudo systemctl start crond
sudo systemctl enable crond
```

Add cron job

```bash
sudo crontab -e
```

Cron entry

```
*/5 * * * * echo hello > /tmp/cron_text
```

## Result
The cron job runs every 5 minutes and writes the text **hello** to `/tmp/cron_text`.
