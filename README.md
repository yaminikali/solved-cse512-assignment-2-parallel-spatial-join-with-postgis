Download Link: https://assignmentchef.com/product/solved-cse512-assignment-2-parallel-spatial-join-with-postgis
<br>
The required task is to understand and implement two tasks: i) Parallel spatial joining technique on-top of an open-source relational database management system (i.e., PostgreSQL) using PostGIS spatial extension and ii) Spatial joining with a map reduce program on top of Apache Spark using the Apache Sedona spatial extension.

<strong>Input Data:</strong> Input datasets consist of two spatial datasets: a point dataset and a rectangle dataset. The point dataset (points.csv) consists of latitudes and longitudes of various points while the rectangle dataset (rectangles.csv) consists of latitudes and longitudes of two diagonal points of rectangles.

Each row in points.csv file has the format longitude,latitude while the same for the rectangles.csv file is longitude1,latitude1,longitude2,latitude2.

Our goal is to perform spatial join between points dataset and rectangles dataset and return the number of points inside each rectangle (including points on rectangle boundary).




<strong>**Required Tasks**</strong>

<h1>Part A (Parallel Spatial Join with PostGIS)</h1>

We load the points and rectangles datasets into two PostgreSQL tables: <em>points</em> and <em>rectangles</em> respectively. The <em>points</em> table has a <em>geometry</em> type column <em>geom</em> which denotes <em>Point</em> geometry. The <em>rectangles</em> table has a <em>geometry</em> type column <em>geom</em> which denotes <em>Envelop</em> geometry (rectangle). You are given a Python file <em>Assignment2_Interface.py</em> with an incomplete function <em>parallelJoin</em>. You need to complete this function. It has following attributes: <em>pointsTable</em>, <em>rectsTable</em>, <em>outputTable</em>, <em>outputPath</em>, and <em>openConnection</em>. The highlevel objective is to find the number of points inside each rectangle save it to the <em>outputTable</em> and <em>outputPath</em>. Perform the following tasks:

<ul>

 <li>Create four partitions/fragments of both <em>pointsTable </em>and<em> rectsTable</em>. You should consider space partitioning such that all points or rectangles of a partition should lie within the corresponding fragment. Fragments doesn’t need to satisfy disjointness property.</li>

 <li>Run four parallel threads. Each thread should perform a spatial join between a fragment of <em>pointsTable </em>and a fragment of<em> rectsTable</em>. The purpose of the join is to find the number of points (<em>pointsTable</em>.<em>geom</em>) inside each rectangle or Envelop (<em>geom</em>) within the corresponding fragment. You must make use of ST_Contains method supported by PostGIS.</li>

 <li>Sort the output of each fragment in the ascending order of counts of points inside the parallel threads.</li>

 <li>Merge the outputs of four parallel joins into <em>outputTable</em> in the ascending order of counts of points. You can design the structure of the <em>outputTable</em> as you wish, but it should have a column named points<em>_count</em> containing counts of points.</li>

 <li>Write the counts of points into the <em>outputPath</em> in the ascending order. You don’t need to write rectangle coordinates.</li>

</ul>

<strong>Set up Instructions for Part A:</strong> You should have PostgreSQL &gt;=13 installed on your device. On top of it, you should install PostGIS extension.

<strong>Naming Conventions to be Followed: </strong>You should not change the following naming conventions:

<ul>

 <li>Database Name: dds_assignment2</li>

 <li>Postgres Username: postgres</li>

 <li>Postgres Password: 12345</li>

</ul>







<h1>Part B (Spatial Join with Map Reduce on Apache Sedona)</h1>

Similar to Part A, you need to perform a spatial join between the points dataset and rectangles dataset in order to count the number of points located within each rectangle. This time, you will perform the task with the help of map and reduce functionalities supported by Spark.

<strong>Task: </strong>You are given a Apache Sedona project named Map-Reduce-Apache-Sedona. Find the Scala file <em>SparkMapReduce.scala</em>. Complete the incomplete function <em>runMapReduce</em>(). Instead of using group by operation, you must make use of Spark <em>map</em> and <em>reducebyKey</em> operations to complete the task. Covert the resultant RDD back to a DataFrame before returning the result. The returned DataFrame should contain the number of points within each rectangle in an ascending order of count values. You don’t need to return rectangles.

<strong>Setting Up Hadoop and Apache Spark Test Environment: </strong>

The following setup instructions are specific to Ubuntu operating system:

<ul>

 <li>Install Java version &gt;= 1.8 and set up the JAVA_HOME environment variable. If you don’t know how to set up the JAVA_HOME environment variable, follow the link: <a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">https://askubuntu.com/questions/175514/how</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">to</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">set</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">java</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">home</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">for</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">–</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">java</a><a href="https://askubuntu.com/questions/175514/how-to-set-java-home-for-java">.</a> To check whether set up is done, run the command <em>echo $JAVA_HOME</em> on your command line. You should see the path.</li>

 <li>Download Hadoop-2.7.7 from the link: <u>https://archive.apache.org/dist/hadoop/common/hadoop2.7.7/hadoop-2.7.7.tar.gz</u> and extract it to your desired location. Now, you need to set up the HADOOP_HOME environment variable. Run the command <em>sudo nano ~/.bashrc</em> on the command line. Copy the statement <em>export HADOOP_HOME=path to the folder hadoop-2.7.7</em> in the opened file. Save and close the file. Run the command <em>source ~/.bashrc</em> and check the correctness of the setup with <em>echo $HADOOP_HOME</em></li>

 <li>Download Spark-2.4.7 from the link: <a href="https://drive.google.com/file/d/1XlRcJNoGTaf-0YvVv8pwZzzjrCqG-IRA/view?usp=sharing">https://drive.google.com/file/d/1XlRcJNoGTaf</a><a href="https://drive.google.com/file/d/1XlRcJNoGTaf-0YvVv8pwZzzjrCqG-IRA/view?usp=sharing">0YvVv8pwZzzjrCqG</a><a href="https://drive.google.com/file/d/1XlRcJNoGTaf-0YvVv8pwZzzjrCqG-IRA/view?usp=sharing">–</a><a href="https://drive.google.com/file/d/1XlRcJNoGTaf-0YvVv8pwZzzjrCqG-IRA/view?usp=sharing">IRA/view?usp=sharing</a> and extract it to your desired location. Now, you need to set up the SPARK_HOME environment variable. Run the command <em>sudo nano ~/.bashrc</em> on the command line. Copy the statement <em>export SPARK_HOME=path to the folder spark-2.4.7-bin-hadoop2.7</em> in the opened file. Save and close the file. Run the command <em>source ~/.bashrc</em> and check the correctness of the setup with <em>echo $SPARK_HOME</em> If you browse the path to the SPARK_HOME, you should see <em>spark-submit</em> under the <em>bin </em>folder which is required for your testing.</li>

</ul>