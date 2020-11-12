

#### 1. 하둡 WordCount프로그램의 동작 원리 설명

- 하둡 WordCount프로그램의 동작 원리를 아래 예제를 통해 설명

 <img src="..\..\img\image-20201112135108462.png" alt="image-20201112135108462" style="zoom:80%;" />

- **Input 단계**에서 단어를 세기위한 데이터를 입력한다.
- **Splitting 단계**에서 입력데이터를 하둡 분산 파일 시스템에서 정한 64MB 블록 사이즈로 나눠서 저장후 블록 단위로 처리를 한다.
  - 예제에서는 Input Data의 한줄이 처리되는 하나의 블록이라고 가정한다.(**Input Split**)

- **Mapping 단계**에서 Mapper는 자기가 담당하는 Input Split을 읽어서 사용자가 정의한 Map함수를 적용한다.
  - 각 단어를 Key로 정하고(**Intermediate key**), Value는 Input Split에 포함된 각 단어의 수가 들어간다.
  - Map함수 적용 결과로 Intermediate Key-Value의 집합이 생성된다.
    - 뒤 늦게 Map 함수가 적용되는 것이 있으면 전체적인 결과에 혼란이 생길 수 있기 때문에 **모든 Mapper들이 Intermediate Key-Value를 생산하기 전까지는 Reduce단계로 진행하지 않는다.**
- **Shuffling 단계**에서 Intermediate Key를 공유하는 Value들은 하나로 묶은 후, 이 Value들은 리스트 형태로 넘겨준다.
- **Reducing 단계**에서 리스트 형태로 받아온 Value들을 Aggregation 연산을 수행한다.
  - 같은 Key를 갖는 Value들은 항상 같은 Reducer가 담당한다.
- 같은 Key를 갖는 Value들이 넘어오면 **Final Result가 생성**된다.



#### 2. 기존 WordCount 프로그램에 추가하거나 개선하고자 하는 각 기능

1. **Mapper와  Reducer에게 부가적인 Parameter전달**
   - 값에 따라서 원하는 작업을 컨트롤이 가능해진다.
2. 성능 개선을 위한 **Combiner** 적용
   - 클러스터에서 가용하는 Bandwidth에 제한이 있을 경우 map과 reducer사이에 전송량을 줄이기 위해 Combiner를 적용한다.
   - Combiner는 Mapper가 실행되는 노드에서 같이 실행된다.

3. 아웃풋 형식 지정



#### 3. 대상 입력 데이터 설명

- 입력 데이터는 임의로 Google의 [*MapReduce: Simplified Data Processing on Large Clusters*]논문의 Abstract부분을 사용하였다.

- 입력 데이터 내용

  ```
   MapReduce is a programming model and an associated implementation for processing and generating large data sets. Users specify a map function that processes a key/value pair to generate a set of intermediate key/value pairs, and a reduce function that merges all intermediate values associated with the same intermediate key. Many real world tasks are expressible in this model, as shown in the paper.
  
   Programs written in this functional style are automatically parallelized and executed on a large cluster of commodity machines. The run-time system takes care of the details of partitioning the input data, scheduling the program’s execution across a set of machines, handling machine failures, and managing the required inter-machine communication. This allows programmers without any experience with parallel and distributed systems to easily utilize the resources of a large distributed system.
  
   Our implementation of MapReduce runs on a large cluster of commodity machines and is highly scalable: a typical MapReduce computation processes many terabytes of data on thousands of machines. Programmers find the system easy to use: hundreds of MapReduce programs have been implemented and upwards of one thousand MapReduce jobs are executed on Google’s clusters every day.
  ```



#### 4. 각 기능에 대한 구현 결과물

1. 특정 횟수 이상 나온 단어만 결과 출력

   ```java
   public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {
       private IntWritable value = new IntWritable(0);
       @Override
       protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
           int sum = 0;
           Configuration conf = context.getConfiguration();
           String param = conf.get("MinimumNum");
           for (IntWritable value : values)
               sum += value.get();
           value.set(sum);
           if(sum >= Integer.parseInt(param))		// 추가된 부분
               context.write(key, value);
       }
   }
   ```



2.  단어를 대문자로

   ```java
   public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {
       private final static IntWritable one = new IntWritable(1);
       private Text word = new Text();
       @Override
       protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
           String line = value.toString();
           StringTokenizer tokenizer = new StringTokenizer(line, " \t\n\r\f,.:;?![]'\"");
   
           while (tokenizer.hasMoreTokens()) {
               word.set(tokenizer.nextToken().toUpperCase());	// 추가된 부분
               context.write(word, one);
           }
       }
   }
   ```



3. 성능 개선을 위한 Combiner 적용

   ```java
   public static void main(String[] args) throws Exception {
           Configuration conf = new Configuration();
           conf.set("MinimumNum",args[2]);
           Job job = new Job(conf, "wordcount");
           job.setJarByClass(WordCount.class);
           job.setOutputKeyClass(Text.class);
           job.setOutputValueClass(IntWritable.class);
           job.setMapperClass(Map.class);
           job.setCombinerClass(Reduce.class);		// 추가된 부분
           job.setReducerClass(Reduce.class);
           job.setInputFormatClass(TextInputFormat.class);
           job.setOutputFormatClass(TextOutputFormat.class);
           job.setNumReduceTasks(1);
   
           FileInputFormat .setInputPaths(job, new Path(args[0]));
           FileOutputFormat.setOutputPath(job, new Path(args[1]));
   
           boolean success = job.waitForCompletion(true);
           System.out.println(success);
       }
   ```

- 전체 코드

  ```java
  package mju.myhadoop.wordcount;
  
  import java.io.IOException;
  import java.util.StringTokenizer;
  
  import org.apache.hadoop.conf.Configuration;
  import org.apache.hadoop.fs.Path;
  import org.apache.hadoop.io.IntWritable;
  import org.apache.hadoop.io.LongWritable;
  import org.apache.hadoop.io.Text;
  import org.apache.hadoop.mapreduce.Job;
  import org.apache.hadoop.mapreduce.Mapper;
  import org.apache.hadoop.mapreduce.Reducer;
  import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
  import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
  import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
  import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
  
  public class WordCount {
      public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {
          private final static IntWritable one = new IntWritable(1);
          private Text word = new Text();
          @Override
          protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
              String line = value.toString();
              StringTokenizer tokenizer = new StringTokenizer(line, " \t\n\r\f,.:;?![]'\"");
             
              while (tokenizer.hasMoreTokens()) {
                  word.set(tokenizer.nextToken().toUpperCase());
                  context.write(word, one);
              }
          }
      }
  
      public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {
          private IntWritable value = new IntWritable(0);
          @Override
          protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
              int sum = 0;
              Configuration conf = context.getConfiguration();
              String param = conf.get("MinimumNum");
              for (IntWritable value : values)
                  sum += value.get();
              value.set(sum);
              if(sum >= Integer.parseInt(param))
              	context.write(key, value);
          }
      }
  
      public static void main(String[] args) throws Exception {
          Configuration conf = new Configuration();
          conf.set("MinimumNum",args[2]);
          Job job = new Job(conf, "wordcount");
          job.setJarByClass(WordCount.class);
          job.setOutputKeyClass(Text.class);
          job.setOutputValueClass(IntWritable.class);
          job.setMapperClass(Map.class);
          job.setCombinerClass(Reduce.class);
          job.setReducerClass(Reduce.class);
          job.setInputFormatClass(TextInputFormat.class);
          job.setOutputFormatClass(TextOutputFormat.class);
          job.setNumReduceTasks(1);
  
          FileInputFormat .setInputPaths(job, new Path(args[0]));
          FileOutputFormat.setOutputPath(job, new Path(args[1]));
  
          boolean success = job.waitForCompletion(true);
          System.out.println(success);
      }
  }
  ```

  

#### **5. 각 기능에 대한 실행 결과물**

1. Mapper와  Reducer에게 부가적인 Parameter전달(MinimumNum)

   - 특정 횟수 이상 나온 단어만 결과 출력

     `# ./hadoop/bin/hadoop jar mywordcount102.jar mju.myhadoop.wordcount.WordCount input myoutput102 7`

    <img src="..\..\img\image-20201112221115659.png" alt="image-20201112221115659" style="zoom:80%;" />

2. Mapper와  Reducer에게 부가적인 Parameter전달(Upper)

   `# ./hadoop/bin/hadoop jar mywordcount101.jar mju.myhadoop.wordcount.WordCount input myoutput101`

    <img src="..\..\img\image-20201112223105423.png" alt="image-20201112223105423" style="zoom:67%;" />

3. 성능 개선을 위한 Combiner 적용

   - Mapper에서 Reducer로 넘어갈때 전송되는 크기가 해당 기능 구현 전 보다 감소되었다.
   - 기능 구현 전

    <img src="..\..\img\image-20201112200606923.png" alt="image-20201112200606923" style="zoom:67%;" />

   - 기능 구현 후

    <img src="..\..\img\image-20201112200638921.png" alt="image-20201112200638921" style="zoom: 67%;" />



#### 6. 결과 분석

- 하둡에서 기본적으로 제공하는 WordCount는 별도의 인자를 받지도 않았다.

-  Combiner기능이 없어 Mapper에서 Reducer로 넘어갈때 전송되는 크기가 컸다.

- 기본적으로 제공하는 WordCount 수행 결과

    <img src="..\..\img\image-20201112224112224.png" alt="image-20201112224112224" style="zoom:80%;" />

- 기능을 모두 구현한 후, 실행 결과

  `./hadoop/bin/hadoop jar mywordcount100.jar mju.myhadoop.wordcount.WordCount input myoutput100 5`

   <img src="..\..\img\image-20201112224223185.png" alt="image-20201112224223185" />