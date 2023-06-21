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
  > $ hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \\
	-input input \
	-output lab3_python_output \
  -mapper /home/comp9313/Lab3/lab03/p1/mapper.py \
	-reducer /home/comp9313/Lab3/lab03/p1/reducer.py \
	-file /home/comp9313/Lab3/lab03/p1/mapper.py \
	-file /home/comp9313/Lab3/lab03/p1/reducer.py
* **Delete output directory if exist:"**
  > $ hadoop fs -rm -r <file_name>
* **Runninig 'mrjob' code:**
  > $ python3 mr_CoTermNSStripe.py -r hadoop hdfs://localhost:9000/user/comp9313/input/pg100.txt -o hdfs://localhost:9000/user/comp9313/output  
  
  -o indicates the output directory
