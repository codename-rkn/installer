# Codename RKN Installer

Installation instructions for [Codename RKN](https://ecsypno.com/pages/codename-rkn):

* [Docker installation](#docker-installation) -- for Mac OSX, Linux and MS Windows. _(recommended)_
* [Automated installation](#automated-installation) -- For Linux.
* [Manual installation](#manual-installation) -- For Linux.
* [Dependencies for headless environments or WSL](#dependencies-for-headless-environments-or-wsl)

## Docker installation

Prerequisites:
* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/)

```bash
git clone https://github.com/codename-rkn/installer.git codename-rkn-installer
cd codename-rkn-installer

docker compose up -d --build # Start the services.
docker exec -it codename-rkn-app-1 bash # Connect to the container.

codename-rkn@a0f4d8c417bf:~$ ./install.sh # Install Codename RKN over the network.
[...] # Snip
codename-rkn@a0f4d8c417bf:~$ ls
install.sh  rkn-v1.4.3.1  rkn.install.log # TADA!
```
_In some Linux distributions, you may need to preface the `docker` commands with `sudo`._

You can now run Codename RKN by using the executables under the `rkn-v*/bin` directory.

1. For a CLI scan you can run `bin/rkn URL`.
2. You can use Codename RKN Pro by running `bin/rkn_pro`.

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

## Automated installation

To install run the following command in a terminal of your choice:

```bash
bash -c "$(curl -sSL https://get.ecsypno.com/rkn)"
```

You can now run Codename RKN by using the executables under the `bin/` directory.

* You can use Codename RKN Pro by running `bin/rkn_pro`
   * Setting up PostgreSQL is recommended for production workloads.

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

### PostgreSQL

For a more reliable Codename RKN Pro experience, it's best to configure it to use PostgreSQL.

Please exchange `secret` with a secure password in the role creation commands.

#### Role creation

```
$ sudo -u postgres psql
postgres=# CREATE USER "rkn-pro" WITH PASSWORD 'secret';
postgres=# ALTER USER "rkn-pro" superuser;
```

#### Connection

```
# Backup SQLite config.
mv ~/.rkn/pro/config/database.yml ~/.rkn/pro/config/database.yml.bak

# Set to use PostgreSQL config.
cp ~/.rkn/pro/config/database.postgres.yml ~/.rkn/pro/config/database.yml
```

Now edit `~/.rkn/pro/config/database.yml` to change the password from `secret`.

#### Setup

From the Codename RKN package directory:

    bin/rkn_pro_task db:create db:migrate db:seed

## Manual installation

1. Download the [latest package](https://github.com/codename-rkn/installer/releases).
2. Extract.
3. Run `bin/rkn_activate KEY` to activate your installation -- if you haven't already done so for a previous release.
   1. [Acquire a license activation key](https://ecsypno.com/)  -- if you haven't already done so for a previous release.

You can now run Codename RKN by using the executables under the `bin/` directory.

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

### Codename RKN Pro (WebUI)

You can run Codename RKN Pro by running `bin/rkn_pro`.

If you'd like to use the Codename RKN WebUI, its database will need to be prepared.

Out of the box, the WebUI comes configured with [SQLite](https://sqlite.org/index.html), however,
for better results and performance please switch to [PostgreSQL](https://www.postgresql.org/).

#### PostgreSQL

##### Configuration

Please exchange `secret` with a secure password in the role creation commands.

###### Role creation

```
$ sudo -u postgres psql
postgres=# CREATE USER "rkn-pro" WITH PASSWORD 'secret';
postgres=# ALTER USER "rkn-pro" superuser;
```

###### Connection

From the package root directory:

```
# Backup SQLite config.
mv .system/rkn-ui-pro/config/database.yml .system/rkn-ui-pro/config/database.yml.bak

# Set to use PostgreSQL config.
cp .system/rkn-ui-pro/config/database.postgres.yml .system/rkn-ui-pro/config/database.yml
```

Now edit `.system/rkn-ui-pro/config/database.yml` to change the password from `secret`.

#### Setup

If this is a fresh installation, you can setup a DB with:

    ./bin/rkn_pro_task db:create db:migrate db:seed

#### Update

If you'd like to update an existing installation you can do it with:

    ./bin/rkn_pro_task db:migrate

## Dependencies for headless environments or WSL

For minimal environments such as headless servers or the Windows Subsystem for Linux, please first run:

```
sudo apt-get update
sudo apt-get install libgconf-2-4 libatk1.0-0 libatk-bridge2.0-0 libgdk-pixbuf2.0-0 libgtk-3-0 libgbm-dev libnss3-dev libxss-dev libasound2
```

## License

Copyright 2023 [Ecsypno Single Member P.C.](https://ecsypno.com/).

All rights reserved.
