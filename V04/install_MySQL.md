# MySQL

```bash
sudo apt-get -y install mariadb-server
```

For `JupyterLab` support:

```bash
#sudo apt install -y libmysqlclient-dev
#sudo apt install -y libsasl2-dev
sudo apt install -y gcc
sudo apt install -y openssl
sudo apt install -y pkg-config
```

### Create a Test_User in MySQL

We will create an own test_user in the `MySQL` database:

The user root has no password:

```bash
sudo mysql -u root -p
Enter password: #just press enter
```

You should see a new sql prompt.

```text
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.36-0ubuntu0.20.04.3 (Ubuntu)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

```sql
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'test_user' password expire never;
GRANT ALL ON *.* TO 'test_user'@'localhost';
exit;
```

We can set the password of test_user with the first login:

```bash
mysql -u test_user -ptest_user
```

### SQL with `JupyterLab`

We can run SQL directly in `JupyterLab`. Open a terminal in `Jupyterlab` and install:

```bash
pip install SQLAlchemy
pip install pandas
pip install PyMySQL
pip install ipython-sql
```
