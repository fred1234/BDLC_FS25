# MariaDB

```bash
sudo apt-get -y install mariadb-server
```

For `JupyterLab` support:

```bash
sudo apt install -y gcc
sudo apt install -y openssl
sudo apt install -y pkg-config
```

### Create a Test_User in MariaDB

We will create an own test_user in the `MariaDB` database:

The user root has no password:

```bash
sudo mysql -u root -p
Enter password: #just press enter
```

You should see a new sql prompt.

```text
mysql>
```

```sql
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'test_user' password expire never;
GRANT ALL ON *.* TO 'test_user'@'localhost';
exit;
```

We can set the password of `test_user` with the first login:

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
