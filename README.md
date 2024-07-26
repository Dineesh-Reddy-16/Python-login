 # (Server-01 ) Install MySQL on DataBase Server

```
yum update -y
dnf -y install @mysql
systemctl enable mysqld.service
systemctl start mysqld.service
systemctl status mysqld.service
mysql_secure_installation
```

# Create a DB and table
```
mysql -u root -p <your pass>
```

# Create a Database
```
create database <Database name>;
use <Database name>;
```

# Create a Table
```
CREATE TABLE accounts (
    id int NOT NULL AUTO_INCREMENT,
    username varchar(255) NOT NULL,
    fullname varchar(255) NOT NULL,
    password varchar(255) NOT NULL,
    email varchar(255),
    designation varchar(255) NOT NULL,
    about_yourself varchar(255) NOT NULL,
    PRIMARY KEY (id)
);
```

## Create a User and assign specific permissions
```
CREATE USER 'root'@'%' IDENTIFIED BY '<Password>';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
FLUSH PRIVILEGES;
```

# ( Server -02 ) Python Installation on RHEL 8.4
-----------------------------------------------
# pre-required softwares
```
yum install wget make git -y

sudo dnf install wget yum-utils make gcc openssl-devel bzip2-devel libffi-devel zlib-devel

wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz 

tar xzf Python-3.9.6.tgz 

cd Python-3.9.6 

sudo ./configure --with-system-ffi --with-computed-gotos --enable-loadable-sqlite-extensions

sudo make -j ${nproc} 
sudo make altinstall 
```

# Verify Python
```
python3.9 -V  
```

# Other Pre-requirements
```
yum -y install gcc gcc-c++ kernel-devel
yum -y install mysql-devel openssl-devel
```

# Get the code from GitHub
```
cd /opt
git clone https://github.com/lazy-anand/python-login-page.git
cd python-login-page
```

# Chnage the DB details in main.py file 
```
app.config['MYSQL_HOST'] = '<your DB IP>'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = '<your DB password>'
app.config['MYSQL_DB'] = '<Your DB>'
```

## Create a Vertual ENV
```
python3.9 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Run the Application in Baground mode
```
nohup ./run &
```

## Access the Application
```
Login Page : http://IP:5000/pythonlogin/
Registartion Page : http://IP:5000/pythonlogin/register
```
