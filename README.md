# puppetLearn
repo for play around with puppet


## CentOS7
Enable the official Puppet Labs collection repository with this command:

    $ sudo rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm

Install the puppetserver package:
    
    $ sudo yum -y install puppetserver

Puppet Server is now installed on your master server, but it is not running yet.

### Configure Memory Allocation (optional)

By default, Puppet Server is configured to use 2 GB of RAM. You should customize this setting based on how much free memory your master server has, and how many agent nodes it will manage.

First, open `/etc/sysconfig/puppetserver` in your favorite text editor. I'm using `vim`:

    $ sudo vi /etc/sysconfig/puppetserver

Then find the `JAVA_ARGS` line, and use the `-Xms` and `-Xmx` parameters to set the memory allocation. For example, if you want to use 3 GB of memory, the line should look like this:

    JAVA_ARGS="-Xms3g -Xmx3g"

my configuration the following one:

    JAVA_ARGS="-Xms512m -Xmx512m -XX:MaxPermSize=256m"

### Start Puppet Server

Now we're ready to start Puppet Server with this command:

    $ sudo systemctl start puppetserver

Next, enable Puppet Server so that it starts when your master server boots:

    $ sudo systemctl enable puppetserver


## Facter

create test facts:

    $ vim /opt/puppetlabs/facter/facts.d/test.py

    ```python
    #!usr/bin/env python
    data = {"key1" : "value1", "key2" : "value2"}

    for k in data:
        print "%s=%s" % (k,data[k])
    ```
run for validate py script:

    $ sudo python /opt/puppetlabs/facter/facts.d/test.py

expected output:

    key1=value1
    key2=vakue2

run `facts` via puppet:

    $ sudo /opt/puppetlabs/bin/puppet facts | grep key1

debug facts:

    $ sudo /opt/puppetlabs/bin/puppet facts --debug

fix the issue:

    $ sudo chmod +x /opt/puppetlabs/facter/facts.d/test.py
    $ sudo /opt/puppetlabs/bin/puppet facts | grep key1