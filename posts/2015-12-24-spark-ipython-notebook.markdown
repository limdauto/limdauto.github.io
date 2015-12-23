---
title: Integrate Apache Spark with latest IPython Notebook (Jupyter 4.x)
tags: python, spark, ipython, jupyter, spark-redshift
---

As you may already know, [Apache Spark](http://spark.apache.org/) is possibly the most popular engine right now for large-scale data processing,
while [IPython Notebook](http://ipython.org/notebook.html) is a prominent front-end for, among other things, sharable exploratory data analysis.
However, getting them to work with each other requires some additional step, especially since latest IPython Notebook has been moved under the
[Jupyter](http://jupyter.org/) umbrella and doesn't support profile anymore.

### Table of Contents

- [I. Prerequisites](#prreq)
- [II. PySpark with IPython Shell](#with-ipython)
- [III. PySpark with Jupyter Notebook](#with-jupyter)
- [IV. Bonus: Spark Redshift](#spark-redshift)


### <a name="prereq"></a> I. Prerequisites

This setup has been tested with the following software:

- Apache Spark 1.5.x
- IPython 4.0.x (the interactive IPython shell & a kernel for Jupyter)
- Jupyter 4.0.x (a web notebook on top of IPython kernel)

```lang-bash
$ pyspark --version
$ ipython --version
$ jupyter --version
```

You will need to set an environment variable as follows:

- `SPARK_HOME`: this is where the spark executables reside. For example, if you are on OSX and install Spark via homebrew, add this
to your `.bashrc` or whatever `.rc` you use. This path differs between environment and installation, so if you don't know where it is,
Google is your friend.


```lang-bash
$ echo "export SPARK_HOME='/usr/local/Cellar/apache-spark/1.5.2/libexec/'" >> ~/.bashrc
```

### <a name="with-ipython"></a> II. PySpark with IPython Shell

The following is adapted from [Cloudera](http://blog.cloudera.com/blog/2014/08/how-to-use-ipython-notebook-with-apache-spark/).
I have removed some unnecessary steps if you just want to get up and running very quickly.

#### 1. Step 1: Create an ipython profile

```lang-bash
$ ipython profile create pyspark

# Possible outputs
# [ProfileCreate] Generating default config file: u'/Users/lim/.ipython/profile_spark/ipython_config.py'
# [ProfileCreate] Generating default config file: u'/Users/lim/.ipython/profile_spark/ipython_kernel_config.py'
```

#### 2. Step 2: Create a startup file for this profile

The goal is to have a startup file for this profile so that everytime you launch an IPython interactive shell session,
it loads the spark context for you.

```lang-bash
$ touch ~/.ipython/profile_spark/startup/00-sg4park-setup.py
```

A minimal working version of this file is

```lang-python
import os
import sys

spark_home = os.environ.get('SPARK_HOME', None)
sys.path.insert(0, os.path.join(spark_home, 'python'))
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.8.2.1-src.zip'))
execfile(os.path.join(spark_home, 'python/pyspark/shell.py'))
```

Verify that this works

```lang-bash
$ ipython --profile=spark
```

You should see a welcome screen similar to this with the SparkContext object pre-created

> <img src="../images/spark_welcome.png" width="600px">
> <figcaption><strong>Fig. 1</strong> | Spark welcome screen</figcaption>

### <a name="with-jupyter"></a> III. PySpark with Jupyter Notebook

After getting spark to work with IPython interactive shell, the next step is to get it to work with the Jupyter Notebook.
Unfortunately, since [the big split](http://blog.jupyter.org/2015/04/15/the-big-split/), Jupyter Notebook doesn't support IPython profile out of the box
anymore. To reuse the profile we created earlier, we are going to provide a modified [IPython kernel](http://ipython.readthedocs.org/en/stable/development/kernels.html)
for any spark-related notebook. The strategy is described [here](http://thepowerofdata.io/configuring-jupyteripython-notebook-to-work-with-pyspark-1-4-0/) but
it has some unnecessary boilerplates/outdated information, so here is an improved version:

#### 1. Preparing the kernel spec

IPython kernel specs reside in `~/.ipython/kernels`, so let's create a spec for spark:

```lang-bash
$ mkdir -p ~/.ipython/kernels/spark
$ touch ~/.ipython/kernels/spark/kernel.json
```

with the following content:

```lang-js
{
    "display_name": "PySpark (Spark 1.5.2)",
    "language": "python",
    "argv": [
        "/usr/bin/python2",
        "-m",
        "ipykernel",
        "--profile=spark"
        "-f",
        "{connection_file}"
    ]
}
```

Some notes: If you are using a virtual environment, change the python entry point to your virtualenvironment's,
e.g. mine is `~/.virtualenvs/machine-learning/bin/python`

#### 2. Profit

Now simply launch the notebook with

```lang-bash
$ jupyter notebook
# ipython notebook works too
```

When creating a new notebook, select the PySpark kernel and go wild :)

> <img src="../images/jupyter_spark_1.png" width="300px">
> <figcaption><strong>Fig. 2</strong> | Select PySpark kernel for a new Jupyter Notebook</figcaption>

> <img src="../images/jupyter_spark_2.png" width="600px">
> <figcaption><strong>Fig. 3</strong> | Example of spark interacting with matplotlib</figcaption>

### <a name="spark-redshift"></a> IV. Bonus: Spark-Redshift

[Amazon Redshift](https://aws.amazon.com/documentation/redshift/) is a popular choice for Data Warehousing and Analytics Database.
Now you can easily load data from your Redshfit cluster into Spark's native [DataFrame](http://spark.apache.org/docs/latest/sql-programming-guide.html)
using a spark package called [Spark-Redshift](https://github.com/databricks/spark-redshift). To hook it up with our jupyter notebook setup, add this to the kernel file

```lang-js
{
    ...
    "env": {
        "PYSPARK_SUBMIT_ARGS": "--jars </path/to/redshift/jdbc.jar> --packages com.databricks:spark-redshift_2.10:0.5.2,com.amazonaws:aws-java-sdk-pom:1.10.34,org.apache.hadoop:hadoop-awes:2.6.0 pyspark-shell"
    }
    ...
}
```

Please note that you need a JDBC drive for Redshfit, which can be downloaded [here](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html).
The last tricky thing to note is that the package uses Amazon S3 as the transportation medium, so you will need to configure the spark context object with your AWS
credentials. This could be done on top of your notebook with:

```lang-python
sc._jsc.hadoopConfiguration().set("fs.s3n.awsAccessKeyId", "YOUR_KEY_ID")
sc._jsc.hadoopConfiguration().set("fs.s3n.awsSecretAccessKey", "YOUR_SECRET_ACCESS_KEY")
```

<p class="typl8-drop-cap">
Whew! I admit it's a bit long but totally worth the trouble. Spark + IPython on top of Redshift is a very formidable combination for exploring your data at scale.
</p>
