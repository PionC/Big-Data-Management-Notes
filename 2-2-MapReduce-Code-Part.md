# **MapReduce Code Part**
This note is only for python, not Java.
## **Secondary Sort**
### **Code**
> **SORT _VALUES = True**  
> *# Set the SORT_VALUE flag as True to tell the program that we are going to sort.*  
> **JOBCONF = {**  
**'mapreduce.map.output.key.field.separator': '-',**  # *Using the '-' symbol for seperating the assemble key*  
**'partitioner': 'org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner',** *# Using the above key field for partition*    
**'mapreduce.partition.keypartitioner.options':'-k1,1',** *# set '-k1,1' as the actual key, in order to ensure the data contain same actual key are paritioned to the same reducer. (E.g. ((a,b),1) and ((a,c),1) are partitioned to the same reducer)*  
**'mapreduce.job.output.key.comparator.class': 'org.apache.hadoop.mapreduce.lib.partition.KeyFieldBasedComparator'** *# Using the above key field for secondary sort.*  
**'mapreduce.partition.keycomparator.options':'-k1,1 -k2,2n'** *# In each partition, sort according to '-k1,1' firstly, then sort accoring to '-k2,2n' * 
**}**

* Assuming the assemble key is **Key = String(field1-field2-field3)**, then,  
  * **'-k1,1'** represents **field1**,  
  * **'-k2,2'** represents **field2**,  
  * **'-k1,2'** represents **field1-field2**,  
  * **'-k2,3'** represents **field2-field3**,  
  * **'-k1,3'** represents **field1-field2-field3**.
* '-k1,1 -k2,2nr'
  * '-n' will convert the string to Numbers firstly, then sort according to the value of the Numbers;
  * '-r' will sort inversely.
* If we are going to excute third-ary sort, we should edit the format of KEY, let's say  **Key = String(field1-field2-field3)**,  
  then, **'mapreduce.partition.keycomparator.options':'-k1,1 -k2,2n -k3,3'**

***
## **Grouping Comparator**
### **Example**
![Grouping Comparator Example](/img3/grouping-comparator-eg.png)
### **Method**
When we are going to use Grouping Comparator, we should apply **reducer_init(), reducer(), reducer_final()** methods, to initialise a list for store all values for an actual key.   
* The implementation of Methods follows the sequence of: 
  * **reducer_init()**
  * **reducer()**
  * **reducer()**
  * **...**
  * **reducer()**
  * **reducer_final()**
### **Code**
>**def reducer_init(self):**  *# Set and initialise global variables: CurKey, PreKey, value_list*  
>&emsp;**self.PreKey = ' '**  
>&emsp;**self.CurKey = ' '**  
>&emsp;**self.value_list = []**  
>
>**def reducer(self, key, values):**  
>&emsp;**k = key.split('-')**  
>&emsp;**self.CurKey = k[0]**  
>&emsp;**cur_value = k[1]**  
>&emsp;**if self.PreKey != self.CurKey and self.PreKey != ' ':**  
>&emsp;&emsp;**yield self.PreKey, self.value_list**  
>&emsp;&emsp;**self.value_list = []** *# Initialise/Empty the value_list*  
>&emsp;**self.value_list.append(cur_value)**  
>&emsp;**self.PreKey = self.CurKey**  
>
>**def reducer_final(sel):**  
>&emsp;**yield self.CurKey, self.value_list**  