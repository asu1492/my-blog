---
source: https://www.google.com/search?q=Hadoop&rlz=1C1JJTC_enIN977IN977&sourceid=chrome&ie=UTF-8
---
MapReduce 
1. Read Jeff Deans and Sanjay Ghemawat paper to understand Mapreduce 

What is Hadoop
* software framework for distributed storage and processing of big data using the MapReduce programming model.
* allows clustering multiple computers to analyze massive datasets in parallel more quickly.
* pioneered by Doug Cutting 
* based on paper published by Sanjay Ghemawat and Jeff Deans 

consists of four main modules

2. [[HDFS]]
3. [[YARN]]
4. [[Hadoop and MapReduce]]
5. Hadoop Common 

Hadoop Common 

6. Provides common Java libraries that can be used across all modules.

![[Computer Science/Data Engineering/_resources/Untitled_Note.resources/unknown_filename.png]]

Bigdata process is broken into 

* Map
* Reduce 

Mapreduce

* Java -- inefficient in representing text and numbers 
* mapreduce define own classes called writables
* Wrapping 
	* new Text(word); new LongWritable(word)
* Unwrapping 
	* value.toString()

* * *

Mapper 

* * *

public class StubMapper extends Mapper <object, text, text, longWritable>
{ 

@override 
public void map(Object key, Text value, Context context)
    throws IOException, InterruptedException
     {
         String\[\] words = value.toString().split("\[\\t\]+");  // input line is split by space or tabs into array of strings 
              for(String word: words
                 {
                  context.write(new Text(word), new longWritable(I));
                    }
        }
}

* object : Datatype of input key 
* text : datatype of input value 
* text : dataype of output key
* longWritable : datatype of output value 

![[Computer Science/Data Engineering/_resources/Untitled_Note.resources/image (1).png]]

![[Computer Science/Data Engineering/_resources/Untitled_Note.resources/image (2).png]]

* * *

Reducers 

* * *

public class StubReducer extends Reducer<Text, LongWritable, Text, LongWritable>
{
    @Override 
    public void reduce(Text key, Iterable<LongWritable> values, Context context)
          throws IOException, InterruptedExceptin {

                   long sum = 0;
                   for(LongWritable iw:values)
                 {
                        sum += iw.get();

                    }
                        context.write(key, new LongWritable(sum)); 

         }

}

* * *

Create a drive 

* * *

public class StubDiver 
{
    public static void main(String\[\] args) throws Exception 
          {
              Job job = Job.getInstance();
              job.setJarByClass(StubDriver.class);
              job.setMapperClass(stubMapper.class);
              job.setReducerClass(stubReducer.class);
              job.setOutputKeyClass(Text.class);
              job.setOutputValueClass(LongWritable.class);

           FileInputFormat.addInputPath(job, new Path("/data/mr/wordcount/input/big.txt"));  //both input and output points towards hdfs 
           FileOutputFormat.setOutputPath(job, new Path("javamrout"));
            boolean result = job.waitForCompletion(true);

         }

}

* * *

Hadoop Streaming 

* enable use of any binary as mapper or reducer 
* Why?
	* Java mapreduce is cumbersome
	* legacy code as mapper or reducer
* Here streaming does not means real time data processing, rather data is streamed through programs(any prog)
* read data from stdin and writes data as stdout 
* Mapper gives key<space>value per line 
* HS is written in java![[Computer Science/Data Engineering/_resources/Untitled_Note.resources/image.png]]
