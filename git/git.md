# Git

**Git** is a *version control system* (VCS), which helps you control the state of your project for different versions.
It is a *command line* tool. 

**Git** comes with **Git Bash**, which gives you some extra commands: `openssl`.

*See: https://git-scm.com/downloads*

## GUI

You can also have a **GUI** for *visualizing* your git repository.

*See: https://git-scm.com/downloads/guis*

## Gitea

*Self-hosted* and *open source* version of **GitHub**.

*See: https://about.gitea.com/*

## GitHub

A hosted version of **Git**, where you can save your *projects* in the cloud.

**Microsoft** acquired **GitHub** on *June 4, 2018,* for **$7.5 billion** in stock. **GitHub** continues to operate *independently* as a *community*, *platform*, and *business* within **Microsoft**.

### GitHub Actions (CI/CD)

Write the configuration of the **CI/CD** in `.github/workflows/config.yml` folder in the **repository**.
They are in **YAML** format *(`.yml` or `.yaml`).*

### Permissions

*See: https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/controlling-permissions-for-github_token*

### Free Usage Limits
**Public Repositories**: Unlimited usage of GitHub-hosted runners at no cost. 

**Private Repositories**:

**GitHub Free (Personal Accounts)**: *2,000* minutes/month and *500 MB* storage.

**GitHub Pro**: *3,000* minutes/month and *1 GB* storage.

**GitHub Team**: *3,000* minutes/month and *2 GB* storage.

**GitHub Enterprise Cloud**: *50,000* minutes/month and *50 GB* storage. 

These free minutes reset **monthly**. If you exceed them and have set a spending limit above *$0*, additional usage incurs charges.

### Usage Timeouts

- **Job Execution Time**: Each job can run for up to **6** hours.

- **Workflow Run Time**: Each workflow run is limited to **35** days.

- **Concurrent Jobs**: The number of concurrent jobs depends on your **GitHub** plan. For example, the **Free** plan allows up to **20** concurrent jobs, with a maximum of **5** concurrent macOS jobs.

### Overage Charges

If you surpass your free minutes and have a spending limit above $0, overage charges apply based on the operating system of the **GitHub-hosted runner**:

**Linux**: **$0.008** per minute.

**Windows**: **$0.016** per minute.

**macOS**: **$0.08** per minute. 

>*Note: that jobs running on Windows and macOS consume minutes at higher rates due to multipliers:*

**Linux**: *1x* multiplier.

**Windows**: *2x* multiplier.

**macOS**: *10x* multiplier. 

For example, a *10-minute* job on *macOS* would deduct *100* minutes from your quota.
