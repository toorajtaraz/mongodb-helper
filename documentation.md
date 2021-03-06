
# Table of Contents

1.  [MongoDB](#orgc03a2af)
    1.  [MySQL to Mongo Lang!](#orgb56c9e5)
        1.  [Table/Collection creation](#orge494f13)
        2.  [Insertion](#org783aaad)
        3.  [Queries](#org67b792e)
    2.  [How to install?](#org98233c2)
        1.  [Headless windows server](#orgf21d809)
        2.  [Centos 8](#org55abb5f)
        3.  [Ubuntu 20.04](#org7ae79eb)
        4.  [Starting mongo&rsquo;s service (Ubuntu and Centos)](#org7452201)
    3.  [How to automate this process?](#orgb1ebbeb)
    4.  [How to setup our database?](#org168a261)
        1.  [Creating a new user with admin access](#org8212198)
        2.  [Creating a new database](#orgcbf985a)
        3.  [Connecting to our database](#orgdbae089)



<a id="orgc03a2af"></a>

# MongoDB


<a id="orgb56c9e5"></a>

## MySQL to Mongo Lang!


<a id="orge494f13"></a>

### Table/Collection creation

    db.people.insertOne( {
        user_id: "abc123",
        age: 55,
        status: "A"
     } )

    CREATE TABLE people (
        id MEDIUMINT NOT NULL
            AUTO_INCREMENT,
        user_id Varchar(30),
        age Number,
        status char(1),
        PRIMARY KEY (id)
    )


<a id="org783aaad"></a>

### Insertion

    db.people.insertOne({
        user_id: "bcd001",
        age: 45, status: "A"
    })

    INSERT INTO people(user_id,
                      age,
                      status
                      )
    VALUES ("bcd001",
            45,
            "A"
           )


<a id="org67b792e"></a>

### Queries

    db.people.find({
        status: "A"
    }).sort({
        user_id: -1
    })

    SELECT *
    FROM people
    WHERE status = "A"
    ORDER BY user_id DESC


<a id="org98233c2"></a>

## How to install?


<a id="orgf21d809"></a>

### Headless windows server

You can find whatever you are going to need in the link below, You also can use notes provided after the link!

-   <https://community.chocolatey.org/packages/mongodb>

Using this line of code you can install chocolatey package manager on your server:

    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

You just need to run this command on your server, please have in mind that prior to this command you need to install chocolatey package manager on your server:
,#+BEGIN\_SRC shell
choco install mongodb &#x2013;pre
\#+END\_SRC

You can provide other parameters such as:

1.  /dataPath: - where MongoDB stores its database files - defaults to &ldquo;$env:ProgramData\MongoDB\data\db&rdquo;
2.  /logPath: - where MongoDB stores its logs - defaults to &ldquo;$env:ProgramData\MongoDBlog&rdquo;


<a id="org55abb5f"></a>

### Centos 8

You can find a rather verbose documentation on this website:

-   <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/>

But these notes are less verbose and easier to follow if you are a experienced system admin.

1.  Configure package manager

    Before anything you need to add mongo&rsquo;s repository so that yum can install mongodb:
    Use your favorite command-line text editor to create a file in this path:
    
    -   /etc/yum.repos.d/mongodb-org-5.0.repo
    
    And paste this yaml file:
    
        [mongodb-org-5.0]
        name=MongoDB Repository
        baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/5.0/x86_64/
        gpgcheck=1
        enabled=1
        gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc

2.  Install the package

    Now you just need to run the command below and you are good to go:
    
        sudo yum install -y mongodb-org
        #Or for installing a specific version
        sudo yum install -y mongodb-org-5.0.2 mongodb-org-database-5.0.2 mongodb-org-server-5.0.2 mongodb-org-shell-5.0.2 mongodb-org-mongos-5.0.2 mongodb-org-tools-5.0.2


<a id="org7ae79eb"></a>

### Ubuntu 20.04

You can find a detailed blog post by digital ocean in the link below:

-   <https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04>

Or even mongo&rsquo;s documentation:

-   <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/>

1.  Configure package manager

    We need to add mongo&rsquo;s repository to apt&rsquo;s configuration, as apt requires packages to be signed to prevent man-in-the-middle attack we need to import mongo&rsquo;s keys, for doing so we need to type the command below in our terminal:
    
        curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
    
    OK
    Now we can add mongo&rsquo;s repository:
    
        echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
    
    After updating apt&rsquo;s db we can safely install mongodb package on our server:
    
        sudo apt update
        sudo apt install mongodb-org


<a id="org7452201"></a>

### Starting mongo&rsquo;s service (Ubuntu and Centos)

As far as I know mongo can be handled by any INIT system such as systemd, runit, openrc, init.d. But as we all know, systemd this &ldquo;GOOD DEVIL&rdquo; is the most popular and every distro is using it; that&rsquo;s why I will only explain starting mongodb service using systemd:

    #TO START
    sudo systemctl start mongod
    #TO START AND ENABLE AUTOSTART
    sudo systemctl enable --now mongod
    #TO STOP
    sudo systemctl stop mongod
    #TO RESTART
    sudo systemctl restart mongod
    #TO DISABLE AUTOSTART
    sudo systemctl disable mongod


<a id="orgb1ebbeb"></a>

## How to automate this process?

1.  Ansible : <https://galaxy.ansible.com/community/mongodb>
2.  Puppet : <https://forge.puppet.com/modules/puppet/mongodb>
3.  Chef : <https://supermarket.chef.io/cookbooks/mongodb>


<a id="org168a261"></a>

## How to setup our database?

If we successfully install mongodb on our server, we will have access to mongo cli tool, using that we can do all kind of stuff with our database :).
Before moving on issue this command to get mongodb&rsquo;s interactive command line:

    $ mongo
    MongoDB shell version v4.4.10
    connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
    Implicit session: session { "id" : UUID("66ed5c8d-e780-47d9-822e-c747a35870cd") }
    MongoDB server version: 4.4.10
    >


<a id="org8212198"></a>

### Creating a new user with admin access

1.  Creating an admin user for any database:

    use admin
    db.createUser({
        user: "mongoadmin",
        pwd: "mongoadmin",
        roles: [
            "userAdminAnyDatabase",
            "dbAdminAnyDatabase",
            "readWriteAnyDatabase"
        ]
    })

1.  Creating user for different databases with different roles:

    use admin
    db.createUser({
        user:"replSetManager",
        pwd:"password",
        roles:[
            {
                role:"clusterManager",
                db:"admin"
            },
            {
                role:"dbOwner",
                db:"adminsblog"
            },
            {
                role:"readWrite",
                db:"departmentblog"
            },
            {
                role:"read",
                db:"otherblog"
            }
        ]
    })

1.  Enabling authorization (OPTIONAL)

    #open /etc/mongod.conf with your favorite text editor and add this section:
    security:
      authorization: enabled


<a id="orgcbf985a"></a>

### Creating a new database

There are to steps to it:

1.  Telling mongodb name of the database
2.  Insert something to this database

    use myBrandNewDB
    db.dummy.insert({
        name: "Ada Lovelace",
        age: 205
    })


<a id="orgdbae089"></a>

### Connecting to our database

We can use mongouri for that matter, the overall format of it is:

-   mongodb://[user]:[pass]@[server address]:[port]/[database name]

