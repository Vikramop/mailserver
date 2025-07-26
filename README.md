# 📧 Self-Hosted Secure Email Infrastructure

A production-grade, self-hosted email system built from scratch using Postfix, Dovecot, OpenDKIM, and Ansible automation. Designed to ensure **secure**, **scalable**, and **high-deliverability** email infrastructure for private or enterprise use — with complete ownership, rate limiting, and zero reliance on third-party inbox services.

---

## 🔧 Features

- ✅ Full Mail Stack: Postfix + Dovecot + OpenDKIM + Mailjet relay
- 🔐 Security: SPF, DKIM, DMARC, TLS 1.3, Fail2Ban, AppArmor, SASL auth
- 📬 Inbox Delivery: Gmail inbox-tested with DKIM-signed messages
- 📉 Rate Limiting: Daily/monthly caps per user (Postfwd)
- 🔁 Outbound Relay: Mailjet SMTP support (port 587)
- ⚙️ Automation: Fully scripted with Ansible roles
- 📈 Designed for scale, compliance, and long-term low-maintenance operation

---

## 🖥️ Architecture

```text
User Agent (Thunderbird, Webmail, Swaks)
            |
         [587/TLS]
            ↓
        ➤ Postfix (MTA)
            ↓
     ┌──────────────┐
     │  Postfwd2    │ ← Rate limiting + Policy
     └──────────────┘
            ↓
     ➤ Dovecot (IMAP/POP3, Auth)
            ↓
     ➤ OpenDKIM (signing)
            ↓
     ➤ Mailjet SMTP Relay (smarthost)
            ↓
        Internet (Inbox)
```

## 🌍 Domain + IP
Domain: 147258.info

VPS IP: 65.20.76.56

PTR: Reverse DNS properly set

Mail Server Hostname: 147258.info

## 🚀 Installation
### 🔁 1. Clone This Repository
```bash
git clone https://github.com/<your-username>/self-hosted-mailserver.git
cd self-hosted-mailserver
```
## 📦 2. Update Inventory & Variables
Edit inventory/hosts and group_vars/all.yml:
```bash
[mail]
147258.info ansible_host=65.20.76.56 ansible_user=root
```
Update your variables in:
```
domain_name: 147258.info
mail_relay_host: in.mailjet.com
mail_relay_port: 587
mail_relay_user: <your-mailjet-username>
mail_relay_pass: <your-mailjet-password>
vps_public_ip: 65.20.76.56
```
## ⚙️ Running the Playbooks
### 📥 Step-by-Step Role Execution
All roles are modular and tagged for selective execution:
```
# 1. Postfix Setup
ansible-playbook site.yml --tags postfix

# 2. Dovecot Setup
ansible-playbook site.yml --tags dovecot

# 3. OpenDKIM Setup
ansible-playbook site.yml --tags opendkim

# 4. Rate Limiting with Postfwd2
ansible-playbook site.yml --tags postfwd

# 5. All at once
ansible-playbook site.yml
```
## ✉️ Sending Test Mail
Once setup is complete, test local relay:
```
swaks --to your_email@gmail.com \
      --from test@147258.info \
      --server 127.0.0.1 \
      --port 587 \
      --auth LOGIN \
      --auth-user alice \
      --auth-password alice \
      --tls \
      --header "Subject: Test Email" \
      --body "Hello from your Postfix server!"
```
