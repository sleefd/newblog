---
layout: post
title: "使用Gson解析json字符串"
date: 2015-09-09 20:58
comments: true
categories: learn
---
Gson是google开发的json序列化与反序列化开源lib,个人觉得相比于其他json解析库比如jackson与Java内置的JSONObject都好用。
正好工作中有用到，因此仔细看了Gson官方文档，并总结如下。

gson object是无状态的，一个gson object可以重复使用，gson提供了
一些有用的工厂方法来进行序列化与反序列化。

#### simple object 序列化与反序列化

{% highlight java%}
Gson gson = new Gson();
gson.toJson(object); //primitive,class，connection都支持
gson.fromJson(json. Object.class);
{% endhighlight %}

#### 泛型反序列化

{% highlight java%}
List<Student> students = Lists.newArrayList(student1, student2);
gson.toJson(object); //get a json array with json object

Type studentListType = new TypeToken<List<Student>>(){}.getType();  //store generical type
List<Student> studentsList = gson.fromJson(json, studentListType);
gson.fromJson(object, type); 
{% endhighlight %}

#### 解析复杂复合json对象

parse json字符串<code> {"name":"slee","columns":[{"name":"a","type":"int"},{"name":"b","type":"int"}]}</code>
得到columns的所有name值.

{% highlight java%}
/** three methods to parse above json string */
//use fromJson and toJson
Gson gson = new Gson();
HashMap map = gson.fromJson(json, HashMap.class);
String jsonArray = gson.toJson(map.get("columns"));
ArrayList list = gson.fromJson(jsonArray, ArrayList.class);
for(Object obj: list){ //no need to defined specified class, use object  ok
	map = gson.fromJson(obj.toString(), HashMap.class);
	System.out.println(map.get("name"));
}


//use gson streaming api, hard to use, need to be very careful


//use gson tree api
JsonParser parser = new JsonParser();
JsonElement  element = parser.parse(json);
JsonArray array = element.getAsJsonObject().getAsJsonArray("columns");
for (JsonElement jsonElement : array) {
System.out.println(jsonElement.getAsJsonObject().get("name").getAsString());
}

{% endhighlight %}

参考：[Gson introduction](http://www.studytrails.com/java/json/java-google-json-introduction.jsp)
