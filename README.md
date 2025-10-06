# 🧠 Install Hadoop in Ubuntu using Java 8

---

## 🚀 Step 1: Install Java JDK 8

First, install **Java JDK 8** on your Ubuntu system.

```bash
sudo apt update
sudo apt install openjdk-8-jdk -y
```

To verify installation:

```bash
java -version
```

Check installation path:

```bash
cd /usr/lib/jvm
```

🧩 **Note:** Hadoop 3.4.x is compatible with **Java 8**.

---

## ⚙️ Step 2: Configure Environment Variables

Edit your **.bashrc** file and add the following configuration:

```bash
nano ~/.bashrc
```

Append these lines at the end:

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin 
export HADOOP_HOME=/home/<username>/hadoop 
export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME 
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.4.1.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
export PDSH_RCMD_TYPE=ssh
```

Apply the changes:

```bash
source ~/.bashrc
```

---

## 🔐 Step 3: Enable SSH for Hadoop

Install **SSH (Secure Shell)** to allow Hadoop to communicate internally.

```bash
sudo apt-get install ssh -y
```

🔹 **SSH** is used for secure remote communication and encrypted data transfer between Hadoop nodes.

---

## 📦 Step 4: Download & Extract Hadoop

Visit [hadoop.apache.org](https://hadoop.apache.org/releases.html) and download the latest Hadoop tar file (e.g., `hadoop-3.4.1.tar.gz`).

Move it to your home directory:

```bash
mv ~/Downloads/hadoop-3.4.1.tar.gz ~/
cd ~
tar -zxvf hadoop-3.4.1.tar.gz
mv hadoop-3.4.1 hadoop
```

Then go to:

```bash
cd ~/hadoop/etc/hadoop
```

---

## 🛠️ Step 5: Configure `hadoop-env.sh`

Open the environment file:

```bash
sudo nano hadoop-env.sh
```

Set the Java path:

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

---

## ⚙️ Step 6: Configure Hadoop XML Files

### 🧩 `core-site.xml`

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
  <property>
    <name>hadoop.proxyuser.dataflair.groups</name>
    <value>*</value>
  </property>
  <property>
    <name>hadoop.proxyuser.dataflair.hosts</name>
    <value>*</value>
  </property>
  <property>
    <name>hadoop.proxyuser.server.hosts</name>
    <value>*</value>
  </property>
  <property>
    <name>hadoop.proxyuser.server.groups</name>
    <value>*</value>
  </property>
</configuration>
```

---

### 🧩 `hdfs-site.xml`

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```

---

### 🧩 `mapred-site.xml`

```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
  <property>
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
  </property>
</configuration>
```

---

### 🧩 `yarn-site.xml`

```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.env-whitelist</name>
    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
  </property>
</configuration>
```

---

## 🗝️ Step 7: Setup SSH Authentication

```bash
ssh localhost
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh/authorized_keys
```

Format Hadoop’s NameNode:

```bash
hadoop/bin/hdfs namenode -format
```

---

## ▶️ Step 8: Start Hadoop

Start all services:

```bash
start-all.sh
```

Verify running daemons:

```bash
jps
```

Example output:

```
8000 Jps
7617 NodeManager
7250 SecondaryNameNode
6979 DataNode
6821 NameNode
7465 ResourceManager
```

To stop Hadoop:

```bash
stop-all.sh
```

---

## 🧩 Architecture Overview

Below is a simplified architecture of a **single-node Hadoop setup**:

```
+--------------------------------------------+
|                Hadoop Node                 |
|--------------------------------------------|
|  NameNode       |  DataNode                |
|  ResourceManager|  NodeManager             |
|--------------------------------------------|
|     YARN Resource Scheduler (Java 8)       |
|     HDFS Storage Layer                     |
+--------------------------------------------+
```

---

### ✍️ Author

Neeraj Kumar Kannoujiya
