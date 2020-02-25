## Centにjava導入

yum install -y java-1.8.0-openjdk
yum install -y java-1.8.0-openjdk-devel
echo "export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64" >> ~/.bashrc
echo "export PATH=$PAT:$JAVA_HOME/bin" >> ~/.bashrc
echo "export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar" >> ~/.bashrc