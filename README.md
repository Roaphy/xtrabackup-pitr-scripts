# xtrabackup-pitr-scripts
A collection of scripts for managing a robust weekly full and hourly incremental backup strategy for MySQL using Percona XtraBackup.

## 🚀 Features

-   **Weekly Full Backups:** Automatically creates a full backup once a week.
-   **Hourly Incremental Backups:** Creates incremental backups every hour, chained to the most recent backup.
-   **Point-in-Time Recovery:** A powerful restore script that can recover the database to a specific minute.
-   **Automated Cleanup:** A retention script to automatically delete old backups.

## 📋 Prerequisites

-   A Red Hat-based Linux distribution (CentOS, RHEL, etc.)
-   Percona XtraBackup 8.0
-   MySQL Server version 8.0+

## 🛠️ Installation

The `install_xtrabackup.txt` file contains the commands to install Percona XtraBackup on a yum-based system.

```bash
# From install_xtrabackup.txt
yum install -y [https://repo.percona.com/yum/percona-release-latest.noarch.rpm](https://repo.percona.com/yum/percona-release-latest.noarch.rpm)
percona-release enable original
yum install percona-xtrabackup-80 -y
```

## ⚙️ Configuration

Before running, you must edit the variables at the top of each script to match your environment:

-   `BACKUP_DIR`: The main directory for storing backups.
-   `MYSQL_USER` & `MYSQL_PASS`: Credentials for the MySQL backup user.
-   `DATADIR`: The path to your MySQL data directory.

## 📜 Usage

### Backup Schedule

It is recommended to set up the following cron jobs:

1.  **Weekly Full Backup:** Run `weekly_full_backup.sh` once a week (e.g., Sunday at 2 AM).
    ```cron
    0 2 * * 0 /path/to/scripts/weekly_full_backup.sh
    ```

2.  **Hourly Incremental Backup:** Run `hourly_prepare_and_merge.sh` every hour.
    ```cron
    0 * * * * /path/to/scripts/hourly_prepare_and_merge.sh
    ```

3.  **Cleanup:** Run `backup_retention_cleanup.sh` daily.
    ```cron
    0 3 * * * /path/to/scripts/backup_retention_cleanup.sh
    ```

### Restore Process

To restore the database, run the interactive `restore_mysql_backup.sh` script. It will prompt you for a target time in `YYYYMMDDHHMM` format.

```bash
./restore_mysql_backup.sh
```

## ⚠️ License

This project is licensed under the MIT License. See the `LICENSE` file for details.
