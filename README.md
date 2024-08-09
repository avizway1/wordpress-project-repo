# Create a wordpress website using EC2 and RDS

1. Launch an ec2 instance (Launch Amazon Linux 2)
2. Launch rds cluster 
3. Configure ec2 instance to connect to rds


**Install mysql client in ec2 instance to connect to RDS cluster**

```bash
sudo yum install -y mysql
```

**Export mysql endpoint as MySQSL_HOST Variable**

```bash
export MYSQL_HOST=<your-RDS-endpoint>
```

Example : export MYSQL_HOST=yt-wordpress.cfpgnjehw330.ap-south-1.rds.amazonaws.com


**Now Connect to mysql to create required user users that we are going to use**

*Connect to RDS using below command (Replace the host-info and user info)*

```bash
mysql -h yt-wordpress.cfpgnjehw330.ap-south-1.rds.amazonaws.com -P 3306 -u admin -p
```

Create required Database, user and provide permisisons to newsly created user on schema we created.

```bash
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser' IDENTIFIED BY 'Avinash1234';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser;
FLUSH PRIVILEGES;
Exit
```

**Now Install and configure apache**

```bash
sudo yum install -y httpd
```
Start Apache Service

```bash
sudo service httpd start
```

**Download a wordpress template and unzip the wordpress**

```bash
wget https://wordpress.org/latest.tar.gz
```

```bash
tar -xzf latest.tar.gz
```

```bash
ls
```

When you enter ls it should deisplay these files and directory "latest.tar.gz" and "wordpress"

```bash
cd wordpress
```

**create a wp-config file from sample file already provided**

```bash
cp wp-config-sample.php wp-config.php
```

**edit the wp-config.php file to point to database**

```bash
vim wp-config.php
```

***Replace the Database_name_here , "username_here", "password_here" and "rds-endpoint-name" with valid info***

```bash
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'rds-endpoint-name' );
```


Now go to below link and it provides some information to update the wp-config file. It looks like below shared one.

```bash
https://api.wordpress.org/secret-key/1.1/salt/
```

***Acquire the valid information and update wp-config file accordingly***

```bash
define('AUTH_KEY',         '+7CA?k*Ju&8eCfg=/aFKo0tO5Tn73Cg 9|Ed73k|Gw(3^');
define('SECURE_AUTH_KEY',  ':H$M&FvbE6t:EwH5ik/D!@]@%Dv3!-Q^hNH3*O+-$L6c*|');
define('LOGGED_IN_KEY',    'g9?;b_A BNW[; $9N^E2^jt$LkF 8_^baTmjhp<eE5GUd');
define('NONCE_KEY',        'G;Wf@|;jzQh>R812&-x^cPoq`tOOu>q)#JVa Y%No%.JpZ[');
define('AUTH_SALT',        'up^dE)4&x/?]1[thjghhjjhz6Vhiohr(dVMh+d5=R<.l_#l');
define('SECURE_AUTH_SALT', '@%ka=9?}BQ[m#29D+@jkgjkhjkhjkhkjhkjdTZ`MT{|fypE~');
define('LOGGED_IN_SALT',   'o!UX5|LW4eijhjkbhkjhkjkjbnjjb/1JSPS?e`YW*nrWb|FG ');
define('NONCE_SALT',       '+t}kH4DA`jhbjkbjkbjkbjkbjkbt8(iWX(]e?&tV;k:>|)IoE');
```

**Now install dependencies**

```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```

**come back to home directory and copy all content to /var/www/html, Then restart the service.**

```bash
cd /home/ec2-user
```

```bash
sudo cp -r wordpress/* /var/www/html/
```
```bash
sudo service httpd restart
```

**Get your instance public IP and Paste it in browser, It should give you wordpress initial configuration page.** 
