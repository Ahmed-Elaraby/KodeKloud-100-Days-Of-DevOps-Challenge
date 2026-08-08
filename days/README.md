# Day 001: Linux User Setup with Non-interactive Shell

Today's task was to create a Linuxuser with a non-interactive shell
the kind of account you'd set up for a service or an automated process 
that should never be used for a real interactive login.



##  What I Used

- Linux user management commands
- SSH
- Basic system administration

## Steps I Followed

1. SSH'd into the app server:

    ```sh
    ssh user@app-server-ip
    ```

2. Created the user with a non-interactive shell:

    ```sh
    sudo useradd -m -s /usr/sbin/nologin user-name
    ```

    - `-s` sets the shell — I used `nologin` so the account can't be used to log in
    - `-m` creates a home directory for the user under `/home`

3. Verified it worked:

    ```sh
    cat /etc/passwd
    ```

    My new user showed up looking like this:
    ```
    ahmed:x:1003:1004::/home/ahmed:/usr/sbin/nologin
    ```

    Then I tried switching to it just to confirm the login is actually blocked:

    ```sh
    sudo su user-name
    ```

    Got back: `This account is currently not available.` — exactly what I expected.


## What I Took Away From This

- Non-interactive shells are how you stop an account from being used for a direct login
- Service/automation accounts should be set to `/usr/sbin/nologin` or `/bin/false`
- Don't just trust the command ran — verify with more than one method (`/etc/passwd`, `id`, trying to `su`)

## Notes to Self on Linux Users

### User Types

| Type | Description |
|------|-------------|
| Regular users | Normal human accounts, interactive login |
| System users | Created by the OS/packages for internal use |
| Service accounts | Created for apps/automation, usually non-interactive |

### Shell Types

| Shell | Behavior |
|-------|----------|
| `/bin/bash` | Interactive — normal login shell |
| `/usr/sbin/nologin` | Non-interactive — blocks login with a message |
| `/bin/false` | Non-interactive — blocks login silently (no message) |

### Where User Info Lives

| File | Contains |
|------|----------|
| `/etc/passwd` | User info (username, UID, GID, home dir, shell) |
| `/etc/shadow` | Encrypted passwords and password policy |

### `useradd` Flags I Care About Most

| Flag | Purpose |
|------|---------|
| `-m` | Create home directory |
| `-s` | Specify shell |
| `-d` | Custom home directory path |
| `-g` | Primary group |
| `-G` | Additional groups |
| `-e` | Account expiry date |


