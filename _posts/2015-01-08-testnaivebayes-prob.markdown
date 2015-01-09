---
layout: post
title: "修改mahout naivebayes test driver遇到问题"
date: 2015-01-08 23:18
comments: true
categories: mahout
---
昨天开始修改mahout testnaivebayesdriver的输出。

看了才知道，原来test job输出的是测试集的正确label值以及每条记录经过
模型判断后得到的score向量。

要得到经过模型分类的label，需要找到score向量最大值的index，然后通过index
映射到label上。因此testjob有两个重要的输入，一个是train阶段得到的model，
一个是train阶段得到的labelIndex. labelIndex文件存储整数(index)与label的映射.

我的需求是test阶段的输出文件为模型分类label及对应的向量，也就是一个textfile，
key是text，value也是text。

因此只需要把mapreduce的输出文件格式改为textfile，key和value都为text，write key
的时候，write模型真实分类的label，write value的时候，将vector dump一下，转为String
格式就ok了。仔细看看代码就能搞定。

但改完放到集群上面跑的时候却遇到了问题，报错如下：

{% highlight java%}
Error: java.io.EOFException
	at java.io.DataInputStream.readFully(DataInputStream.java:197)
	at java.io.DataInputStream.readUTF(DataInputStream.java:609)
	at java.io.DataInputStream.readUTF(DataInputStream.java:564)
	at org.apache.mahout.math.VectorWritable.readFields(VectorWritable.java:130)
	at org.apache.mahout.math.VectorWritable.readFields(VectorWritable.java:89)
	at org.apache.mahout.math.VectorWritable.readVector(VectorWritable.java:226)
	at org.apache.mahout.classifier.naivebayes.NaiveBayesModel.materialize(NaiveBayesModel.java:116)
	at mahout.classifier.naivebayes.TestNaiveBayesMapper.setup(TestNaiveBayesMapper.java:42)
	at org.apache.hadoop.mapreduce.Mapper.run(Mapper.java:142)
	at org.apache.hadoop.mapred.MapTask.runNewMapper(MapTask.java:764)
	at org.apache.hadoop.mapred.MapTask.run(MapTask.java:340)
	at org.apache.hadoop.mapred.YarnChild$2.run(YarnChild.java:168)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1548)
	at org.apache.hadoop.mapred.YarnChild.main(YarnChild.java:163)
{% endhighlight %}

仔细比对了代码，与原代码没有区别，只是在输出处做了修改。于是直接跑cdh5.3-mahout0.9的testnb job，以seq和mr模式跑都没问题呀。也是醉了。我测试阶段用到的模型是train阶段输出的，train阶段
用的也是cdh。应该没问题呀？仔细看异常，是在namedvector解析处遇到了问题，但我从输入数据开始
都没有用到过namedvector。

最后想到可能是，我是基于mahout-1.0-snapshot开发的，新代码可能在某些函数上做了修改，虽然代码
看起来一样，但内部实现已经改变了。于是我转为直接用mahout-1.0生成model，然后重新在集群上测试
我的代码，ok了。

总结：在开源lib上做开发的时候，测试与开发保持相同版本很重要，否则会带来很多意想不到的问题。就
像做app开发一下，一定要做真机测试，不同机型很可能就会有不同问题。