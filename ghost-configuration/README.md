# Installation and Deployment of Ghost

**Step 1: Connect to Your AWS Instance:**

  - Open your terminal.

  - Use the SSH command to connect to your AWS instance using the SSH credentials. Replace `your-aws-instance-ip` and `your-ssh-key.pem` with the actual IP address and SSH key file path:

    ```bash
    ssh -i /path/to/your-ssh-key.pem ubuntu@your-aws-instance-ip
    ```

**Step 2: Update and Upgrade Your System:**

  - Once connected to your AWS instance, it's a good practice to update and upgrade your system's packages:

    ```bash
    sudo apt update -y
    sudo apt upgrade -y
    ```

**Step 3: Apply the new hostname:**

  - To apply the new hostname without rebooting, you can use the hostnamectl command:

    ```bash
    sudo hostnamectl set-hostname new-hostname
    ```
    Please make sure to replace "new-hostname" with your desired hostname when following these steps.

**Step 4: Install NGINX:**

  - Ghost uses NGINX as a reverse proxy. To install Nginx, use following command::

    ```bash
    sudo apt install nginx -y
    ```

**Step 5: Install MySQL:**

  - Next, you’ll need to install MySQL to be used as the production database.

    ```bash
    sudo apt install mariadb-server -y
    ```

  - Enable the MariaDB service.

    ```bash
    sudo systemctl enable --now mariadb
    ```

  - To create a "myblog" database with a user "myblog" that has full privileges on the database.

    ```bash
    mysql
    CREATE DATABASE myblog;
    GRANT ALL ON myblog.* TO 'myblog'@'localhost' IDENTIFIED BY '123Admin';
    FLUSH PRIVILEGES;
    EXIT;
    ````
    Here's what each of these commands does:

    1. mysql: This command starts the MySQL shell, allowing you to interact with the MySQL server.
    
    2. CREATE DATABASE myblog;: This command creates a new database named "myblog."
    
    3. GRANT ALL ON myblog.* TO 'myblog'@'localhost' IDENTIFIED BY '123Admin';: This command grants all privileges on the "myblog" database to a user named "myblog" when connecting from the localhost with the password "123Admin." Make sure to use single quotes around the username, hostname ('localhost' in this case), and password.
    
    4. FLUSH PRIVILEGES;: This command tells the MySQL server to reload the user privileges and apply the changes you made with the GRANT command.
    
    5. EXIT;: This command exits the MySQL shell and returns you to the command line.
   
**Step 6: Add User for Ghost administration:**

  ```bash
  sudo adduser ghostcms
  ```

  This command creates a new user named "ghostcms." When you run this command, you will be prompted to set a password for the new user and provide additional information like their full name, phone number, and other details. You can choose to provide this information or leave it empty.

  ```bash
  sudo usermod -aG sudo ghostcms
  ```
    
  After creating the "ghostcms" user, this command adds the user to the "sudo" group. Users in the "sudo" group have administrative privileges and can run commands with superuser (root) privileges using the sudo command. This is a common practice to grant administrative access to users while maintaining security.

**Step 7: Install Node.js and npm:**

  Ghost requires Node.js and npm to run. You can install them using the following commands:

  - Download and import the Nodesource GPG key
     ```bash
     sudo apt- update
     sudo apt install -y ca-certificates curl gnupg
     sudo mkdir -p /etc/apt/keyrings
     curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
    ```
  - Create deb repository
     ```bash
     NODE_MAJOR=18 # Use a supported version
     echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
     ```
  - Run update and install
    ```bash
    sudo apt update
    sudo apt install nodejs -y
    ```

**Step 8: Install Ghost-CLI:**

  - Ghost has a command-line tool called Ghost-CLI for managing Ghost installations. Here's a breakdown of each command:

    1. `sudo npm i -g ghost-cli`: This command installs the Ghost Command Line Interface (CLI) globally on your system. The Ghost CLI is a tool that helps you install, configure, and manage Ghost instances.

    2. `sudo npm i -g ghost-cli@latest`: This command specifically installs the latest version of the Ghost CLI. Adding `@latest` ensures you get the most up-to-date version.

    3. `sudo mkdir -p /var/www/ghostcms`: This command creates a directory named "ghostcms" in the "/var/www" directory. This is a common location for hosting web applications.

    4. `cd /var/www/ghostcms`: This command changes the current working directory to "/var/www/ghostcms." You'll typically install your Ghost CMS in this directory.

    5. `sudo chown ghostcms:ghostcms /var/www/ghostcms`: This command changes the ownership of the "/var/www/ghostcms" directory and all its contents to the user and group "ghostcms." This is often done to ensure that the Ghost CMS process has the necessary permissions to read and write files in this directory.

    6. `sudo chmod 775 /var/www/ghostcms`: This command sets the permissions on the "/var/www/ghostcms" directory to 775. This means that the owner and group have read, write, and execute permissions, while others only have read and execute permissions. These permissions are often set to allow the Ghost CMS process to write logs, manage files, and execute scripts within the directory.


**Step 9: Create a Directory for Your Ghost Site:**
  - `sudo su - ghostcms`: This command switches the user to "ghostcms" and opens a new shell session as the "ghostcms" user. You'll be prompted to enter the user's password to authenticate.

  - `cd /var/www/ghostcms`: After switching to the "ghostcms" user, this command changes the current working directory to "/var/www/ghostcms." This is where you previously set up the directory for your Ghost installation.

  - `mkdir blog.example.com`: This command creates a directory named "blog.example.com" within the "/var/www/ghostcms" directory. You're likely planning to install your Ghost blog in this subdirectory.


**Step 10: Navigate to Your Ghost Directory:**
  
  - `cd blog.example.com`: After creating the "blog.example.com" directory, this command changes the current working directory to "/var/www/ghostcms/blog.example.com," where you intend to install Ghost.

**Step 11: Install Ghost**

  - Use Ghost-CLI to install Ghost. You'll be prompted to provide your domain and MySQL database information during the installation:

    ```bash
    ghost install
    ```
    
    **Follow the prompts to configure your site. i.e:**
     
    ? Enter your blog URL: **https://blog.example.com**

    ? Enter your MySQL hostname:  **localhost**

    ? Enter your MySQL username: **myblog**

    ? Enter your MySQL password: **[hidden] your_password**

    ? Enter your Ghost database name: **myblog**
    

**If you want more details, you can check out these web page:**

- [How to install Ghost on Ubuntu](https://ghost.org/docs/install/ubuntu/ "How to install Ghost on Ubuntu")