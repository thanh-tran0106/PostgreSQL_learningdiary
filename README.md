# PostgreSQL_learningdiary
The idea of Catshelter actually came to me while I was learning about PostgreSQL. I was just getting started with databases, and PostgreSQL seemed like a good place to start. As I was figuring things out, I decided to create a little practice project to help me learn—something simple but meaningful.

### Step 1: Install PostgreSQL

So how do I start practice with PostgreSQL, the first step was to install it. I was working on a Linux VM, so I opened the terminal and began with the usual update command:

```bash
sudo apt-get update
```

Then, I installed PostgreSQL with:

```bash
sudo apt-get install postgresql
```

It felt like a small victory when I saw the installation complete without any errors. But then came the next challenge—figuring out how to actually start using PostgreSQL.

### **Step 2: Logging into PostgreSQL**

With PostgreSQL installed, my next task was to log in and start interacting with it. But how? I spent some time reading through documentation and forum posts, trying to piece it all together.

Finally, I discovered the `psql` command, which is PostgreSQL’s interactive terminal. But to access it, I needed to log in as the `postgres` user, which PostgreSQL creates by default during installation. So, back to the terminal I went:

```bash
sudo -i -u postgres
```

This command switched me to the `postgres` user, and I was ready to dive into the PostgreSQL environment. To get into the PostgreSQL command line interface, I ran:

```bash
psql
```

However I encountered the following issue: [1]

![Screenshot 2024-08-25 at 14.07.44.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/87a467a1-ceb3-443e-afa2-a48d94047e83/Screenshot_2024-08-25_at_14.07.44.png)

Now I have to do troubleshooting

To troubleshoot, I started by checking the PostgreSQL service status:

```bash
postgres@robot4:~$ exit
logout
thanh@robot4:~$ sudo systemctl status postgresql.service 
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: en>
     Active: active (exited) since Sun 2024-08-25 11:07:03 UTC; 4min 43s ago
    Process: 3306 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 3306 (code=exited, status=0/SUCCESS)
        CPU: 717us
```

It looks like the `postgresql.service` is in an "exited" state, meaning it's not actively running the PostgreSQL server process, even though the service itself is technically "active."

After researching online, I discovered that the "exited" status typically means that the `postgresql.service` itself is not starting the PostgreSQL server. This could be due to a misconfiguration or because the actual PostgreSQL server process is managed by a different service.

So I tried to check the status of the PostgreSQL server directly with command:

```bash
sudo systemctl status postgresql@VERSION-main
# can Replace VERSION with your PostgreSQL version (e.g., 13, 14
```

However, I only saw the `postgresql.service` and not the instance-specific services like `postgresql@VERSION-main.service`: [2]

![Screenshot 2024-08-25 at 14.35.31.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/cfdb1dee-d852-4285-a39a-574cf7ae1712/Screenshot_2024-08-25_at_14.35.31.png)

So I conducted further research to troubleshoot this problem.

First, I needed to check which PostgreSQL versions were installed and Ensure that PostgreSQL server processes were running by using two commands below: [3]

```bash
dpkg -l | grep postgresql
ps aux | grep postgres
```

![Screenshot 2024-08-25 at 14.41.05.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/5d805f9d-04f5-41b8-a815-322d4d9876e2/Screenshot_2024-08-25_at_14.41.05.png)

Based on the output, it seemed that PostgreSQL 14 was installed, but the PostgreSQL server process was not running, and no PostgreSQL processes were listed. Therefore, the next step I needed to take was checking for an existing PostgreSQL cluster using the command: [4]

![Screenshot 2024-08-25 at 14.43.45.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/ddea01c5-289b-4d00-9085-744e27b310eb/Screenshot_2024-08-25_at_14.43.45.png)

It appeared that `pg_lsclusters` was not showing any output, which indicated that there were no PostgreSQL clusters currently configured or running on my system. So the next step I needed to initialize a New PostgreSQL Cluster: [5]

![Screenshot 2024-08-25 at 14.45.57.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/731f11d8-8fd9-4e1e-8ae9-67644d93b7b9/Screenshot_2024-08-25_at_14.45.57.png)

Next, I checked the status of the PostgreSQL cluster. [6]

![Screenshot 2024-08-25 at 14.48.34.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/8f31f184-7a59-4598-80c7-7b2174a1afec/Screenshot_2024-08-25_at_14.48.34.png)

It was running, which made me very happy. Now, I tried to log in as the `postgres` user again and access the PostgreSQL command line interface. [7]

![Screenshot 2024-08-25 at 14.49.59.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/fca46b8f-7c1c-44cc-8c9c-cde1a2b0a7af/Screenshot_2024-08-25_at_14.49.59.png)

### Step 3: Experiment PostgreSQL

It worked successfully; I was in. 

Then I started experimenting with a few basic commands to become familiar with PostgreSQL, such as \l and \c. [8]

![Screenshot 2024-08-25 at 15.01.22.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/7410a770-4be1-4942-b1a2-702ae2d4df0f/Screenshot_2024-08-25_at_15.01.22.png)

Once I was logged in, I started by creating a database for myself called "mycat" with the following command: [9]

```bash
create database mycat;
```

![Screenshot 2024-08-25 at 15.03.45.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/f7251d4d-461e-4b79-afdc-2d907d9a6dd0/Screenshot_2024-08-25_at_15.03.45.png)

Then I connected to the database and started practicing by creating a table to store information about different cats—things like their name, breed, and coloration. It was a basic table, but it helped me become familiar with how to structure data in PostgreSQL, and it’s also where I  using data type and SQL constraints to ensure the data's integrity.

Here's what my first table looked like: [10]

```sql
create table feline (
	felin_id SERIAL PRIMARY KEY,
	name VARCHAR(100),
	breed TEXT NOT NULL,
	coloration TEXT NOT NULL,
	age INTEGER NOT NULL,
	sex CHAR(1) NOT NULL,
	fav_toy TEXT NOT NULL );
```

![Screenshot 2024-08-25 at 15.21.32.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/9d71b7ff-523b-4bc9-b90a-d1ddba9f3322/b282b42e-0f8c-4561-9aef-7c0b5694c72a.png)

### What is data type ?

Data type refers to the type of value a variable can hold and the kind of operations that can be performed on it. Below are the data types used in the feline table and the rationale behind each choice:

- **feline_id**: `SERIAL` is used for the `feline_id` column to automatically generate unique identifiers for each row. As the primary key, it ensures that each record in the `feline` table has a unique identifier without manual input.
- **name** : VARCHAR(100)allows for variable-length strings up to 100 characters. It provides flexibility for storing cat names of different lengths while limiting the size to avoid excessively long names.
- **breed and coloration and fav_toy** : `TEXT` is used for the `breed`, `coloration`, and `fav_toy` columns because these fields can contain text of varying lengths, and `TEXT` is well-suited for longer descriptions.ght have longer names or descriptions
- **age**: INTERGER is suitable for storing whole numbers repesenting the age of the cat.
- **sex**: CHAR(1) is used to store a single character representing the cat's sex (e.g., 'M' for male or 'F' for female).

### What are PostgreSQL-constraints

**Constraints** are rules enforced by the database to ensure the accuracy and reliability of the data in a table. They limit the type of data that can be inserted into a table. If data violates a constraint, the action is aborted.

Constraints can be column level or table level. All of the constraints I used to create feline table are column constraints. 

- **PRIMARY Key** − Uniquely identifies each row/record in a database table.
- **NOT NULL Constraint** − Ensures that a column cannot have NULL value.

### Inserting Values into the `feline` Table

Then i try to insert values into feline table :

```sql
insert into feline (name, breed, coloration, age, sex, fav_toy) values
	('Tom', 'persian', 'grey','1', 'M', 'catnip'),
	('Luna', 'british short hair', 'white', '2', 'F', 'mouse'),
	('No', 'siamese', 'black','4', 'M', 'ball'),
	('To', 'ragdoll', 'spotted', '3', 'F', 'fish'),
	('Sam', 'maincoon', 'cream', '2','M', 'feather wand'),
	('Simba', 'sphynx', 'hairless', '1', 'F', 'ball'),
	('Nala', 'ragdoll', 'chocolate', '1', 'M', 'fish'),
	('Bella', 'ragdoll', 'blue', '3','F', 'ball'),
	('Osca', 'scottish', 'yellow', '5','M','lazer pointer'),
	('Whisker', 'bengal', 'spotted', '2', 'M', 'tunnel');
```

This is the feline table after inserting values: [11]

![Screenshot 2024-08-25 at 15.25.33.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/95f21519-b978-4265-badf-03aa101aedc1/Screenshot_2024-08-25_at_15.25.33.png)

### Creating the `medicalrecord` Table

After setting up that first table, I got a bit more ambitious. I decided to create another table to store medical records for the cats. This is where things started getting interesting, and I began diving into the concept of data relationships.

### Learning About Data Relationships

With the `feline` table in place, I moved on to creating the `medicalrecord` table. This table would store important information like record_id, checkup date , treatment, doctor_name.But I quickly realized that I needed a way to link each medical record to a specific cat in the `feline` table. That’s when I started learning about data relationships and foreign key constraint 

### What is a Foreign Key constraint?

A foreign key is a column (or a group of columns) in one table that creates a link between data in two tables. The foreign key in the second table references the primary key in the first table, establishing a relationship between the two tables. For example, when creating the `medicalrecord` table to store each cat's health history, I used a foreign key to link these medical records to the corresponding cats in the `feline` table.

The foreign key establishes a data relationship between the two tables. The table containing a foreign key is referred to as the referencing table or child table. Conversely, the table referenced by a foreign key is known as the referenced table or parent table.

In this scenario:

- The `feline` table is the **parent** table because it holds the primary key (feline_id).
- The `medicalrecord` table is the **child** table because it references the (feline_id) from the `feline` table.

This relationship is a one-to-many relationship. One cat in the 'feline’ table can have many corresponding records in the 'medicalrecord’ table, but each medical record belongs to only one cat.

**PostgreSQL foreign key constraint syntax**

```bash
FOREIGN KEY(fk_columns) 
   REFERENCES parent_table(parent_key_columns)
```

Here's how the `medicalrecord` table was created:

```sql
create table medicalrecord (
		      record_id serial primary key,
		      feline_id integer,
		      checkupdate DATE,
		      treatment varchar(100),
		      doctor_name varchar(50),
		      foreign key (feline_id) references feline (feline_id));
```

After creating the table, I realized there was a small error in the `feline` table column name, so I used the following command to rename the column:

```sql
alter table feline rename column id to feline_id;
```

I then recreated the `medicalrecord` table successfully: [12]

![Screenshot 2024-08-25 at 15.50.07.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/657551be-0855-49d2-9cdb-9abfdbb748b7/Screenshot_2024-08-25_at_15.50.07.png)

### Inserting Data from a CSV File

Then I inserted data into the table. However, while I originally used the command `INSERT INTO feline (name, breed, coloration, age, sex, fav_toy) VALUES` to insert data into the `feline` table, I discovered another method: importing data from a CSV file.

To do this, I needed to create a `data.csv` file. [13]

![Screenshot 2024-08-25 at 16.30.57.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/3b0695cf-1fef-497c-8a57-0ede05cc3f83/Screenshot_2024-08-25_at_16.30.57.png)

then I can load this CSV file into your table using the `COPY` command in psql:

```sql
COPY medicalrecord (record_id, feline_id, checkupdate, treatment, doctor_name)
FROM '/path/to/data.csv' 
DELIMITER ',' 
CSV HEADER;
```

- **`FROM '/path/to/data.csv'`**: Path to the CSV file.
- **`DELIMITER ','`**: Specifies the delimiter used in the CSV file (comma in this case).
- **`CSV HEADER`**: Indicates that the first row of the CSV file contains column headers.

However, I encountered errors because of file permissions [14]

![Screenshot 2024-08-25 at 16.36.45.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/e6d776ce-f344-4af9-8b44-3605cbcd4868/Screenshot_2024-08-25_at_16.36.45.png)

It mean use the `\COPY` command in `psql` instead of `COPY` For client-side file access

### Resolving File Permissions Issues

The issue was related to directory and file permissions. Here’s how I resolved it:

1. Check File Permissions of /home/thanh/data.csv.  [15]

![Screenshot 2024-08-25 at 16.41.17.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/c2243c28-b940-4217-a49a-932a392314e1/Screenshot_2024-08-25_at_16.41.17.png)

As you can see: 

The directory permissions `drwxr-x--- 6 thanh thanh 4096 Aug 25 13:12 /home/thanh` indicate that:

- **Owner** (`thanh`): Has read, write, and execute permissions.
- **Group** (`thanh`): Has read and execute permissions.
- **Others**: No permissions.

The issue likely arises because the directory does not have read and execute permissions for "others" (i.e., users not in the group or the owner). For PostgreSQL to access the file through `psql`, the `psql` client (running under your user) needs sufficient access permissions to traverse the directory path to the file.

⇒ Solution: Change the permissions of the `/home/thanh` directory to allow others to read and execute. This will allow users not in the group `thanh` to access the directory.

```bash
chmod 755 /home/thanh
```

New directory permissons: [16]

![Screenshot 2024-08-25 at 16.48.14.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/dd85a936-8fd8-4592-a701-e982ae7738fe/Screenshot_2024-08-25_at_16.48.14.png)

At the same time, file ownership of the data.csv file also had problem: [17]

![Screenshot 2024-08-25 at 16.42.43.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/fd85fcee-c3f9-4ba9-a4ad-d9baca48dea0/Screenshot_2024-08-25_at_16.42.43.png)

As you can see, the output `-rw-r--r-- 1 root root 394 Aug 25 13:05 data.csv` indicates that:

- **Permissions**: The file is readable by everyone (`rw-r--r--`).
- **Owner**: The file is owned by `root` not thanh
- **Group**: The group is `root`  not thanh

Since I run psql as a non-root user. I need to change file ownership to my username: my primary group by command:

```bash
sudo chown thanh:thanh /home/thanh/data.csv
```

New file ownership: [18]

![Screenshot 2024-08-25 at 16.52.54.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/f192923e-c559-4c74-9ce5-6e064504c71b/Screenshot_2024-08-25_at_16.52.54.png)

After fixing these issues, the `\COPY` command worked successfully, and the data was inserted into the `medicalrecord`table. [19]

![Screenshot 2024-08-25 at 16.54.53.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/082a4348-41da-4acd-a649-87746ab74535/Screenshot_2024-08-25_at_16.54.53.png)

## Configuring PostgreSQL: Making It Work for Me

Once PostgreSQL was installed, I realized that it needed some configuration to work the way I wanted. This is where I really started to understand how customizable PostgreSQL can be.

PostgreSQL on Linux comes with a couple of key configuration files such as `postgresql.conf` and `pg_hba.conf`. At first, these files were a bit intimidating, but after a little bit of trial and error, I started to get the hang of it.

### postgresql.conf

**`postgresql.conf`:** This file controls a lot of PostgreSQL's behavior, like how it handles memory, logging, and connections. I didn’t change much at first. In this file I configure.

```bash
#listen_addresses = 'localhost'         # what IP address(es) to listen on;
listen_addresses = '*'          # what IP address(es) to listen on;
```

By default, PostgreSQL might be configured to listen only on `localhost`, which means it would only accept connections from the local machine (VM1 in this case)

**Configuration Change**

- **`listen_addresses = '*'`**: This setting allows PostgreSQL to accept connections from any IP address. For my setup, this would allow VM2 to connect to the PostgreSQL database on VM1.

### **pg_hba.conf**

**`pg_hba.conf`:** This one was all about security—controlling who can connect to the database and how they can authenticate. 

**Configuration Change**

```bash
# local         DATABASE  USER  METHOD  [OPTIONS]
host            all     all     0.0.0.0/0       md5
host            all     all     0.0.0.0/0       trust
```

**`host all all 0.0.0.0/0 md5`**:

- **`host`**: This specifies that the entry applies to TCP/IP connections.
- **`all`**: This means the rule applies to all databases.
- **`all`**: This means the rule applies to all users.
- **`0.0.0.0/0`**: This CIDR notation means the rule applies to all IP addresses (essentially, any client can connect).
- **`md5`**: This indicates that MD5 password authentication is required for connections. While MD5 is not the most secure hashing algorithm, it provides a basic level of password protection, ensuring that unauthorized users cannot easily gain access to the database.

**`host all all 0.0.0.0/0 trust`**:

- **`host`**: Again, this applies to TCP/IP connections.
- **`all`**: Applies to all databases.
- **`all`**: Applies to all users.
- **`0.0.0.0/0`**: Applies to all IP addresses.
- **`trust`**: This means no authentication is required; anyone can connect from any IP address without providing a password.By using `trust`, we reduce the friction of repeatedly entering passwords, thus streamlining the development process.

### Why I chose to configure like that?

My project is on development phase so I chose a configuration that emphasizes flexibility and ease of access. This configuration will be change in production to make it more security. 

After edit configuration file of postgresql I had to restart server to apply the change by using command:

```bash
sudo systemctl restart postgresql
```

## Backing Up and Restoring database

As I delved deeper into PostgreSQL, I quickly discovered that managing data is not just about creating tables and relationships. One crucial aspect I learned about was backing up and restoring databases.

### Why Backup?

Backing up my database wasn’t something I thought about initially. However, when I began planning to deploy my project on AWS, I quickly saw how important backups are. Moving to the cloud introduced new risks and potential problems. It became clear that having a strong backup plan was essential to protect my data from unexpected issues or mistakes during deployment.

### Backup a PostgreSQL Database

When I began to think seriously about data protection, I realized that backing up my entire PostgreSQL setup was crucial, especially as I prepared to deploy my project on AWS. To do this, I used `pg_dumpall`, a tool that creates a backup of all databases in a PostgreSQL instance.

`pg_dumpall` is handy for creating a comprehensive backup of all the databases and roles in your PostgreSQL server. This is especially useful if you have multiple databases and want to ensure that every piece of your setup is backed up.

Here’s the command I used to back up all my databases:

```bash
sudo pg_dumpall -U postgres -h localhost -p 5432 -f backup_file.sql
```

However, I encountered a password authentication issue when run this command: [20]

![Screenshot 2024-08-26 at 11.41.08.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/ef1f44d2-8663-4759-8a68-876bc65f9e78/Screenshot_2024-08-26_at_11.41.08.png)

But I found out how to solve that by changing the PostgreSQL user password since passwords in PostgreSQL are stored in a hashed format, making it impossible to extract the original password.

I used the SQL command below to change the password.

```bash
ALTER USER postgres PASSWORD 'newpassword';
```

And it worked beautifully when I tried the pg_dumpall command again. Now the backup_file.sql is successfully created. [21]

![Screenshot 2024-08-26 at 11.41.43.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3eeea792-6a15-4818-99ea-5b65509b1256/b4863123-375c-43c9-89c1-b12be4a83c80/Screenshot_2024-08-26_at_11.41.43.png)

**Restoring from a Backup**

After backup database completely, I restored it in ec2  on AWS by using ‘psql’ command below.

```bash
psql -U postgres -f backup_file.sql
```

This command restored all the databases and roles from the backup file, putting everything back to how it was when the backup was made.

### Conclusion

---

After diving into PostgreSQL, I successfully installed and configured it on my Linux VM. I navigated through some initial issues with service status and learned how to initialize a new PostgreSQL cluster. Creating a simple database and setting up tables for tracking cats and their medical records was both challenging and rewarding. I experimented with data insertion methods, tackled file permissions issues, and adjusted PostgreSQL configurations to fit my needs. By the end, I had a fully functional database and a solid grasp of PostgreSQL fundamentals. If you're starting on a similar journey, just remember: persistence and curiosity are key!
