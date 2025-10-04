# mapreduce-project1
MIT805 - Part2
Below is my MapReduce code
TripCountMapper.java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class TripCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {

    private static final IntWritable one = new IntWritable(1);
    private Text tripDate = new Text();

    @Override
    protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {

        if (key.get() == 0 && value.toString().contains("pickup_datetime")) {
            return;
        }

        String[] fields = value.toString().split(",");

  
        if (fields.length > 5) {
            String pickupDatetime = fields[5];
            if (!pickupDatetime.isEmpty()) {
                String dateOnly = pickupDatetime.split(" ")[0]; // Extract date (e.g., 2025-01-01)
                tripDate.set(dateOnly);
                context.write(tripDate, one);
            }
        }
    }
}

TripCountReducer.java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

public class TripCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {

    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        int total = 0;
        for (IntWritable val : values) {
            total += val.get();
        }
        context.write(key, new IntWritable(total));
    }
}

TripCountDriver.java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class TripCountDriver {
    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "Trip Count Per Day");

        job.setJarByClass(TripCountDriver.class);
        job.setMapperClass(TripCountMapper.class);
        job.setReducerClass(TripCountReducer.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));

        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}

Compiling and running
mkdir tripcount_classes
javac -classpath `hadoop classpath` -d tripcount_classes TripCountMapper.java TripCountReducer.java TripCountDriver.java

Crearig a JAR
jar -cvf tripcount.jar -C tripcount_classes/ 

Run the job
hadoop jar tripcount.jar TripCountDriver /user/evezebekulu/input/fhvhv_tripdata_2025-Jan_to_Jul.csv /user/evezebekulu/output/tripcount
