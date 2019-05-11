# Hadoop

```html
http://hadoop.apache.org/
```

## Demo

### Sample

#### Prepare

* VM QuickStart

```html
https://www.cloudera.com/documentation/enterprise/5-13-x/topics/cloudera_quickstart_vm.html
```

* Docker QuickStart

```html
https://www.cloudera.com/documentation/enterprise/5-13-x/topics/quickstart_docker_container.html
```

```bash
docker pull cloudera/quickstart
docker run --name quickstart.cloudera --hostname=quickstart.cloudera --privileged=true -t -i -p 38888:8888 -p 37180:7180 -p 37190:7190 -p 37191:7191 -p 38020:8020 -p 38022:8022  cloudera/quickstart /usr/bin/docker-quickstart
/home/cloudera/cloudera-manager
```

* Start Cloudera Manager

``` bash
/home/cloudera/cloudera-manager --express/enterprise --force
```

* Leave Shell without interrupt docker

``` bash
Ctrl+P -> Ctrl+Q
````

#### create 2 folders test1 and test2

``` bash
$ hadoop fs -mkdir -p delete/test1
$ hadoop fs -mkdir -p delete/test2
$ hadoop fs -ls delete/
Found 2 items
drwxr-xr-x   - hdfs supergroup          0 2019-04-21 05:33 /delete/test1
drwxr-xr-x   - hdfs supergroup          0 2019-04-21 05:34 /delete/test2
```

#### remove test1

``` bash
hadoop fs -rm -r delete/test1
```

#### validate the trash for test1

``` bash
$ hadoop fs -ls .Trash/Current/user/hadoop/delete/
Found 1 items
drwxr-xr-x   - hdfs supergroup          0 2019-04-21 05:33 /user/hdfs/.Trash/Current/delete/test1
```

#### remove test2 skipTrash

``` bash
hadoop fs -rm -r -skipTrash delete/test2
```

#### validate the trash for test2

``` bash
$ hadoop fs -ls .Trash/Current/user/hadoop/delete/
Found 1 items
drwxr-xr-x   - hdfs supergroup          0 2019-04-21 05:33 /user/hdfs/.Trash/Current/delete/test1
```

#### Q&A

##### Permission denied

``` bash
[root@quickstart /]$ hadoop fs -mkdir -p /delete/test1
mkdir: Permission denied: user=root, access=WRITE, inode="/":hdfs:supergroup:drwxr-xr-x
[root@quickstart /]$ su - hdfs
```

### Practice

#### Cloudera CDH Installation

#### HDFS CLI