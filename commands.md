# **Hadoop system commands:**
> $ cd \$HADOOP_HOME   
> $ start-all.sh  
> $ stop-all.sh  
> $ jps  
> $ hdfs dfs -ls

# **Useful Hadoop Commands:**
* **Put files**
  > $ hdfs dfs -put pg100.txt input
* **Running python code with hadoop steaming:**
  > $ hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.1.jar -input input -output output -mapper mapper.py -reducer reducer.py -file mapper.py -file reducer.py
