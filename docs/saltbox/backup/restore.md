# Restore

!!! info
    Just like the initial install, these instructions are assuming you are running as `root` until told otherwise below.

## Dependencies

Start by installing dependencies.

=== "curl"

    ```shell
    curl -sL https://install.saltbox.dev | sudo -H bash; cd /srv/git/saltbox
    ```

=== "wget"

    ```shell
    wget -qO- https://install.saltbox.dev | sudo -H bash; cd /srv/git/saltbox
    ```

=== "curl (verbose)"

    ```shell
    curl -sL https://install.saltbox.dev | sudo -H bash -s -- -v; cd /srv/git/saltbox
    ```

=== "wget (verbose)"

    ```shell
    wget -qO- https://install.saltbox.dev | sudo -H bash -s -- -v; cd /srv/git/saltbox
    ```

Then retrieve the configuration files from a backup.

## Using Restore Service

=== "curl"

    ```{ .sh .annotate }
    curl -sL https://restore.saltbox.dev | bash -s 'USERNAME' 'PASSWORD' # (1)!
    ```

    1. Use the username and password defined for the service when last backup was executed.

        Must wrap the username and password in quotes.

=== "wget"

    ```{ .sh .annotate }
    wget -qO- https://restore.saltbox.dev | bash -s 'USERNAME' 'PASSWORD' # (1)!
    ```

    1. Use the username and password defined for the service when last backup was executed.

        Must wrap the username and password in quotes.

Then run `preinstall` which will setup the user account and a few other dependencies for the restore.

```shell
sb install preinstall
```

!!! info
    From this point you'll want to make sure you run commands as the user specified in the accounts.yml

!!! info
    If you are using a service account to authenticate the rclone remote that holds the backup, you will need to put that SA JSON file in place manually so that the restore process can authenticate the remote to download the rest of the backup.

 <details>
  <summary>What's this about service accounts?</summary>
  <br />

  Open `rclone.conf` in a text editor and look through the remotes defined in there.

  If the look like this:

  ```text
  [SOME REMOTE]
  type = drive
  scope = drive
  service_account_file = /opt/sa/all/1500.json
  team_drive = OZZY
  root_folder_id =
  ```

  You will need to make sure that service account file [`/opt/sa/all/1500.json`] is available on the new saltbox machine at that same path in order to authenticate against google and download the backup files you're about to restore.
  </details>

Start the restore process.

```shell
sb install restore
```

Once succesfully completed you can now follow the installation guide from this [step](../../saltbox/install/install.md#step-5-saltbox).

## Without Restore Service

Retrieve the following configuration files from your backup manually and place them in `/srv/git/saltbox`:

* accounts.yml
* settings.yml
* adv_settings.yml
* backup_config.yml
* providers.yml
* hetzner_nfs.yml
* rclone.conf
* localhost.yml

Then run `preinstall` which will setup the user account and a few other dependencies for the restore.

```shell
sb install preinstall
```

!!! info
    From this point you'll want to make sure you run commands as the user specified in the accounts.yml

!!! info
    If you are using a service account to authenticate the rclone remote that holds the backup, you will need to put that SA JSON file in place manually so that the restore process can authenticate the remote to download the rest of the backup.

Start the restore process.

```shell
sb install restore
```

Once successfully completed you can now continue:

If you are migrating from one server to another, return to the [migration guide](migrate.md)

If you are restoring to the same server, you can now follow the installation guide from this [step](../../saltbox/install/install.md#step-5-saltbox).
