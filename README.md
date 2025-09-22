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

   <img width="975" height="242" alt="image" src="https://github.com/user-attachments/assets/9812b81c-e440-4db3-85c7-8c0421cc1bfc" />

   du -sh /home/*   # Shows per-user directory usage
   ```
  <img width="506" height="138" alt="image" src="https://github.com/user-attachments/assets/67fd9355-057a-4139-964d-f08a3cf01591" />

3. **Identify resource-intensive processes:**
   ```bash
   ps aux --sort=-%cpu | head -n 10
   <img width="975" height="205" alt="image" src="https://github.com/user-attachments/assets/e878da86-c69e-4e28-98e7-dfeed8ce0de0" />

   ps aux --sort=-%mem | head -n 10
   <img width="975" height="190" alt="image" src="https://github.com/user-attachments/assets/63b3ba39-6142-4031-a589-d52a6b045b30" />

   ```

4. **Save outputs to logs for reporting:**
   ```bash
   mkdir -p /var/log/sysmon
   htop -b -n 1 > /var/log/sysmon/htop_$(date +%F).log
   df -h > /var/log/sysmon/disk_$(date +%F).log
   ps aux --sort=-%cpu | head -n 10 > /var/log/sysmon/topcpu_$(date +%F).log
   ```

✅ **Deliverables:**
- Logs in `/var/log/sysmon/`
- Screenshots of `htop`, `df`, and top processes

---

## **Task 2: User Management & Access Control** (10 Marks)

### Steps:
1. **Create user accounts:**
   ```bash
   sudo adduser Sarah
   sudo adduser Mike
   ```

2. **Set secure passwords:**
   ```bash
   sudo passwd Sarah
   sudo passwd Mike
   ```

3. **Create isolated workspaces:**
   ```bash
   mkdir -p /home/Sarah/workspace
   mkdir -p /home/Mike/workspace
   chown Sarah:Sarah /home/Sarah/workspace
   chown Mike:Mike /home/Mike/workspace
   chmod 700 /home/Sarah/workspace
   chmod 700 /home/Mike/workspace
   ```

4. **Enforce password policy:**
   Edit `/etc/login.defs`:
   ```
   PASS_MAX_DAYS   30
   PASS_MIN_DAYS   7
   PASS_MIN_LEN    10
   PASS_WARN_AGE   7
   ```
   Apply immediately:
   ```bash
   sudo chage -M 30 Sarah
   sudo chage -M 30 Mike
   ```

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
   cat /backups/apache_verify_YYYY-MM-DD.log
   cat /backups/nginx_verify_YYYY-MM-DD.log
   ```

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
