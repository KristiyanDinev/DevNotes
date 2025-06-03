# CRON

You **schedule** tasks to run at specific points in time.

You cannot use **seconds** and **years** in standard Linux **cron** *(via crontab)*, but you can use schedulers like **Quartz**.

## Characters:

- `*` *(Asterisk):* Represents **all** possible values for a field.

- `?` *(Question Mark):* Used in the ***day-of-month (DOM)*** and ***day-of-week (DOW)*** fields to **ignore** their time. You can **NOT** use `?` in both **DOM** and DOW at the **same** time. If you specify a value for **DOM**, then **DOW** must be `?`. If you specify a value for **DOW**, then **DOM** must be `?`.

- `-` *(Hyphen):* Specifies a **range** of values. Example: `5-10` in `0 5-7 * ? * * *`. Runs at **5**, **6** or **7 minutes** during the **hour**. Why the `0`? Because the **schedule** reads from **left** to **right** and, like that, we say *"Starts at ***second 0*** at ***minutes*** from ***5*** to ***7***, every ***hour***"*.

- `,` *(Comma):* Separates **multiple** values. Example: `0 5,6,7 * ? * * *` this will do the **same** thing as the example above.

- `/` *(Slash):* Specifies **increments** by adding **num1 + num2**. Example: `0 5/1 * ? * * *`. This will run from the **fifth** minute to the **59th** in that hour. Like **6**, **6**, **7**, **8** to **59** *included*.

## Quartz-style (library) CRON

It follows this **pattern**:

```sql
<second> <minute> <hour> <day-of-month> <month> <day-of-week> <year>
```

## Linux CRON

It follows this **pattern**:

```sql
<minute> <hour> <day-of-month> <month> <day-of-week> /path/to/script_or_command
```

### Install

**For Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install cron
```

**For RHEL/CentOS/Fedora:**
```bash
sudo yum install cronie
# or
sudo dnf install cronie
```

**For Arch Linux:**
```bash
sudo pacman -S cronie
```

### Enable CRON Service

```bash
sudo systemctl enable cron
sudo systemctl start cron
```

>*Note: On Red Hat-based systems, the service might be named `crond` instead of `cron`:*

```bash
sudo systemctl enable crond
sudo systemctl start crond
```