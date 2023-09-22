# An Ansible Playbook To Install Gitea

> Tested with Gitea version `1.20.x`

This playbook is only meant to be used for a very simple installation of Gitea. There is not a lot of error handling and alike, so productive use is not recommended.  
You can however easily deploy Gitea on a **linux host running systemd** and have a look at Gitea.

This playbook

- installs `git` if needed
- downloads the **Gitea binary**
- creates a **git user**
- creates the Gitea **configuration file**
- creates directories for runtime data
- creates a **systemd service** for Gitea
- creates an initial user called "*gitea*" with a password of "*gitea*"

The playbook also asks you which hosts you want to set up. When asked, simply type in the name(s) of your host(s).

```
ansible-playbook -i /path/to/my/inventory gitea.yml
```

```
Which hosts would you like to set up?: name-of-host-1, name-of-host-2, ...
```

You can of course get rid of the `vars_prompt` yourself.

---

## Variables

`gitea_version`  
Default: "1.20.4"  
Here you can set the specific version of Gitea you want to install. Setting the variable to `latest` will install the latest release.

`gitea_user`  
Default: "git"  
The name of the user within your system to be used for SSH connections for example. Will be created if needed.

`gitea_user_home`  
Default: "/home/{{ gitea\_user }}"  
The home directory of the `gitea_user`.

`gitea_port`  
Default: "80"  
The port to be used by Gitea.

`gitea_disable_registration`  
Default: true  
Whether to disable open registration to the Gitea instance.

---

By default this playbook sets up Gitea using **SQLITE**. If you prefer, you can use a MySQL database instead. You'd have to [set it up](https://docs.gitea.com/installation/database-prep) first!  
For the connection to work you have to set the following variable according to your existing database.

`gitea_connection_mysql`  
Default: undefined

```
gitea_connection_mysql:
  host: STRING -> IP address of your database host
  port: STRING -> Port used by your database
  database: STRING -> The name used for your Gitea database
  user: STRING -> The name of database user to use
  password: STRING -> The password for the database user
  ssl: BOOL -> Whether to use an SSL encrypted connection; you'd have to set this up yourself
```

Other backends like PostgreSQL might follow.
