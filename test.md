# Running SQL Server 2017 on Linux Containers

## Install Docker

1. To log on, click **Administrator**, type the password **Pa$$w0rdLinux**, and then click **Sign In**.
2. At the bottom-left of the desktop, click **Show Applications**, and then click **Terminal**.
3. To update the package index, type the following command, and then press Enter:

    ```-nocode
    sudo apt-get update
    ```

4. At the **Password** prompt, type **Pa$$w0rdLinux**, and then press Enter.
5. To add install packages to allow apt to use a repository over HTTPS, type the following command, and then press Enter:

    ```-nocode
    sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    ```

6. To add the Docker GPG key, type the following command, and then press Enter:

    ```-nocode
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

7. To add install packages to allow apt to use a repository over HTTPS, type the following command, and then press Enter:

    ```-nocode
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

8. To update the package index, type the following command, and then press Enter:

    ```-nocode
    sudo apt-get update
    ```

9.  To install Docker, type the following command, and then press Enter:

    ```-nocode
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```

10. If the status of Docker is not **Active**, type the following command, and then press Enter:

    ```-nocode
    sudo systemctl start docker
    ```

11. To automatically start Docker when the system boots up, type the following command, and then press Enter:

    ```-nocode
    sudo systemctl enable docker
    ```

## Create and Query a SQL Server Container

1. In the terminal, to pull the SQL Server container image, type the following command, and then press Enter:

    ```-nocode
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

2. In the terminal, to run the SQL Server container image, type the following command, and then press Enter:

    ```-nocode
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Pa$$w0rdSQL' -p 1500:1433 --name sqltestcontainer -d mcr.microsoft.com/mssql/server:2017-latest
    ```

3. At the **Password** prompt, type **Pa$$w0rdLinux**, and then press Enter.
4. When the image has been downloaded and the container started, to check that SQL Server is running in the new container, type the following command, and then press Enter:

    ```-nocode
    sudo docker ps
    ```

5. To install software dependencies for Azure Data Studio, type the following command and press Enter:

    ```-nocode
    sudo apt-get install gconf2-common libxss1 libgconf-2-4 libunwind8
    ```

6. Type **Y** if asked for confirmation.
7. To install Azure Data Studio, type the following command, and then press Enter:

    ```-nocode
    sudo dpkg -i ./Downloads/azuredatastudio-linux-1.4.5.deb
    ```

8. At the **Password** prompt, type **Pa$$w0rdLinux**, and then press Enter.
9. Type **y** to any confirmation questions.
10. To start Azure Data Studio, type the following command, and then press Enter:

    ```-nocode
    azuredatastudio
    ```

11. If asked if you would like to enable preview features, click **Yes**.
12. If asked if you would like to allow Microsoft to collect usage data, close the dialog box.
13. In the **Connection** pane, in the **Server** box, type **localhost, 1500**.
14. In the **Authentication type** drop-down list, click **SQL Login**.
15. In the **User name** box, type **sa**.
16. In the **Password** box, type **Pa$$w0rdSQL**, and then click **Connect**.
17. In the **Tasks** section, click **New Query**.
18. In the script window, type the following code:

    ```-nocode
    SELECT @@VERSION
    GO
    ```

19. In the top-left of the script window, click **Run**.
20. Switch to the terminal window.
21. To start the Bash shell within the new container, type the following command, and then press Enter:

    ```-nocode
    sudo docker exec -it sqltestcontainer bash
    ```

22. To start the sqlcmd tool, type the following command, and then press Enter:

    ```-nocode
    /opt/mssql-tools/bin/sqlcmd -U SA -P 'Pa$$w0rdSQL'
    ```

23. To query the database server, type the following commands, and then press Enter:

    ```-nocode
    SELECT @@version
    GO
    ```

24. To exit the sqlcmd tool, type **exit**, and then press Enter.
25. To exit the Bash shell on the container, type **exit**, and then press Enter.

## Investigate and Clean Up Containers

1. To list active container instances, in the terminal, type the following command, and then press Enter:

    ```-nocode
    sudo docker ps
    ```

2. To list all container images, type the following command, and then press Enter:

    ```-nocode
    sudo docker image ls
    ```

3. To stop the SQL Server container, type the following command, and then press Enter:

    ```-nocode
    sudo docker stop sqltestcontainer
    ```

4. To check that the container is no longer running, type the following command, and then press Enter:

    ```-nocode
    sudo docker ps -a
    ```

5. To delete the container, type the following command, and then press Enter:

    ```-nocode
    sudo docker rm sqltestcontainer
    ```

6. To check that the container no longer exists, type the following command, and then press Enter:

    ```-nocode
    sudo docker container ls
    ```
