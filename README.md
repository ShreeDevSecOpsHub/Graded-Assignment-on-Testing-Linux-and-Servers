# 📑 Report: DevOps Development Environment Setup

## 🧑‍💻 Problem Context
As a Fresher DevOps Engineer assisting Rahul (Senior DevOps Engineer), I implemented system monitoring, secure user management, and automated backup configurations for two developers (Sarah and Mike).  
The goal was to ensure **system health visibility, secure access, and data recovery mechanisms**.

---

## **Task 1: System Monitoring Setup** (15 Marks)

### Steps:
1. **Install monitoring tools:**
   ```bash
   sudo apt update
   sudo apt install htop nmon -y
   ```
   - `htop`: Interactive CPU/memory/process monitoring.
   - `nmon`: Detailed performance data.
<img width="975" height="224" alt="image" src="https://github.com/user-attachments/assets/6574045d-f13a-4a70-80d0-5764162ef3a8" />

2. **Disk usage monitoring:**
   ```bash
   df -h   # Shows overall disk usage
```
<img width="975" height="242" alt="image" src="https://github.com/user-attachments/assets/9812b81c-e440-4db3-85c7-8c0421cc1bfc" />

```bash
   du -sh /home/*   # Shows per-user directory usage
   ```
<img width="506" height="138" alt="image" src="https://github.com/user-attachments/assets/67fd9355-057a-4139-964d-f08a3cf01591" />

3. **Identify resource-intensive processes:**
   ```bash
   ps aux --sort=-%cpu | head -n 10
   ```
<img width="975" height="205" alt="image" src="https://github.com/user-attachments/assets/e878da86-c69e-4e28-98e7-dfeed8ce0de0" />

```bash
   ps aux --sort=-%mem | head -n 10
```
<img width="975" height="190" alt="image" src="https://github.com/user-attachments/assets/63b3ba39-6142-4031-a589-d52a6b045b30" />

   ```

4. **Save outputs to logs for reporting:**
   ```bash
   mkdir -p /var/log/sysmon
   htop -b -n 1 > /var/log/sysmon/htop_$(date +%F).log
   df -h > /var/log/sysmon/disk_$(date +%F).log
   ps aux --sort=-%cpu | head -n 10 > /var/log/sysmon/topcpu_$(date +%F).log
   ```
<img width="571" height="59" alt="image" src="https://github.com/user-attachments/assets/eaa3252f-40ab-41a0-9f4d-6c691f513bee" />
<img width="879" height="53" alt="image" src="https://github.com/user-attachments/assets/3cca2525-34b1-4d21-be44-4d4aee19e764" />
<img width="809" height="33" alt="image" src="https://github.com/user-attachments/assets/3cb942fb-2af1-40b4-b7af-0fdf48f2c3bd" />
<img width="997" height="163" alt="image" src="https://github.com/user-attachments/assets/9ce76116-4385-422f-b7e3-7b0ff9ddc523" />

✅ **Deliverables:**
- Logs in `/var/log/sysmon/`
- Screenshots of `htop`, `df`, and top processes
<img width="975" height="490" alt="image" src="https://github.com/user-attachments/assets/b7242521-a9e0-4300-8469-d6306517830d" />
<img width="975" height="245" alt="image" src="https://github.com/user-attachments/assets/e224a7fc-3720-47d5-9e7a-6d80005a8e9f" />
<img width="975" height="793" alt="image" src="https://github.com/user-attachments/assets/ff8be75a-77ee-44a5-a4eb-5b392b00bcdb" />

---

## **Task 2: User Management & Access Control** (10 Marks)

### Steps:
1. **Create user accounts:**
   ```bash
   sudo adduser Sarah
   sudo adduser Mike
   ```
<img width="466" height="51" alt="image" src="https://github.com/user-attachments/assets/62e86891-bcf2-4ad1-b6ef-a654626ee1cd" />

2. **Set secure passwords:**
   ```bash
   sudo passwd Sarah
   sudo passwd Mike
   ```
<img width="791" height="285" alt="image" src="https://github.com/user-attachments/assets/74355f83-bb72-4c0a-92a5-248213d8b16d" />

3. **Create isolated workspaces:**
   ```bash
   mkdir -p /home/Sarah/workspace
   mkdir -p /home/Mike/workspace
   chown Sarah:Sarah /home/Sarah/workspace
   chown Mike:Mike /home/Mike/workspace
   chmod 700 /home/Sarah/workspace
   chmod 700 /home/Mike/workspace
   ```
<img width="679" height="53" alt="image" src="https://github.com/user-attachments/assets/9e72eff8-58fd-47fe-8913-ceea040fe0fc" />
<img width="791" height="56" alt="image" src="https://github.com/user-attachments/assets/df7b9ae0-36b9-4052-affb-273a4232b135" />
<img width="684" height="56" alt="image" src="https://github.com/user-attachments/assets/6173364a-19b3-4fd0-909e-efe86c137b76" />
<img width="765" height="190" alt="image" src="https://github.com/user-attachments/assets/949cfdeb-0aa2-4843-b404-20c34ac02e0b" />

**Isolation Verified** 
<img width="751" height="351" alt="image" src="https://github.com/user-attachments/assets/cb303ffb-357f-47a7-be52-74445c067b4a" />


4. **Enforce password policy:**
   Edit `/etc/login.defs`:
   ```
   PASS_MAX_DAYS   30
   PASS_MIN_DAYS   7
   PASS_MIN_LEN    10
   PASS_WARN_AGE   7
   ```
   <img width="975" height="260" alt="image" src="https://github.com/user-attachments/assets/ea6db75f-6d09-472c-8db5-373e4f4b4703" />

   Apply immediately:
   ```bash
   sudo chage -M 30 Sarah
   sudo chage -M 30 Mike
   ```
<img width="884" height="478" alt="image" src="https://github.com/user-attachments/assets/85a09edc-22ae-4314-a677-58776884bb50" />

✅ **Deliverables:**
- User accounts created
- Workspace isolation verified
- Password policies in place

---

## **Task 3: Backup Configuration for Web Servers** (20 Marks)

### Steps:
1. **Create backup directory:**
   ```bash
   sudo mkdir -p /backups
   sudo chmod 755 /backups
   ```
<img width="510" height="81" alt="image" src="https://github.com/user-attachments/assets/325e4554-a4db-4f60-a232-85ea52078995" />

2. **Backup scripts:**

   **Sarah (Apache):** `/usr/local/bin/apache_backup.sh`
   ```bash
   #!/bin/bash
   BACKUP_DIR="/backups"
   DATE=$(date +%F)
   FILE="$BACKUP_DIR/apache_backup_$DATE.tar.gz"

   tar -czf $FILE /etc/httpd/ /var/www/html/
   tar -tzf $FILE > $BACKUP_DIR/apache_verify_$DATE.log
   ```
   ```bash
   sudo chmod +x /usr/local/bin/apache_backup.sh
   ```

   **Mike (Nginx):** `/usr/local/bin/nginx_backup.sh`
   ```bash
   #!/bin/bash
   BACKUP_DIR="/backups"
   DATE=$(date +%F)
   FILE="$BACKUP_DIR/nginx_backup_$DATE.tar.gz"

   tar -czf $FILE /etc/nginx/ /usr/share/nginx/html/
   tar -tzf $FILE > $BACKUP_DIR/nginx_verify_$DATE.log
   ```
   ```bash
   sudo chmod +x /usr/local/bin/nginx_backup.sh
   ```
<img width="770" height="135" alt="image" src="https://github.com/user-attachments/assets/bafbfc25-8a9f-4e01-8689-40751a1bc170" />

3. **Schedule cron jobs (every Tuesday at 12:00 AM):**
   ```bash
   crontab -e
   ```
   Add entries:
   ```
   0 0 * * 2 /usr/local/bin/apache_backup.sh
   0 0 * * 2 /usr/local/bin/nginx_backup.sh
   ```

4. **Verify backup integrity:**
   ```bash
   ls /backups/
```
<img width="528" height="131" alt="image" src="https://github.com/user-attachments/assets/20c5f895-e507-4e23-8ab0-73399794a14e" />

```bash
cat /backups/apache_verify_YYYY-MM-DD.log
```
   <img width="940" height="826" alt="image" src="https://github.com/user-attachments/assets/bf6c4319-65ae-48a5-a28f-72f33e8d5355" />

```bash
   cat /backups/nginx_verify_YYYY-MM-DD.log
   ```
<img width="740" height="710" alt="image" src="https://github.com/user-attachments/assets/cbce3676-c40b-4ce6-8875-73e230d2dff2" />


✅ **Deliverables:**
- Backup files in `/backups/`
- Verification logs proving data integrity
- Cron job configuration

---

## 📊 Overall Challenges & Learnings (5 Marks)
- Ensuring **permissions isolation** without blocking user productivity.  
- Writing **robust backup scripts** with verification logs.  
- Learned how to use **htop, nmon, df, du, ps** effectively for real-time monitoring.  
- Implemented **password aging policies** to enforce security compliance.  
- Automated backups using **cron jobs**, ensuring disaster recovery readiness.

---

# ✅ Final Deliverables Checklist
- [ ] Logs from monitoring tools  
- [ ] Screenshots of user creation & directory permissions  
- [ ] Password policy verification (`chage -l Sarah`)  
- [ ] Backup tar files & verification logs in `/backups/`  
- [ ] Crontab entries proof  
