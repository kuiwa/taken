---
layout: post
title: "testng (dependsOnMethod)"
date: 2016-08-23
comments: true
categories: 
---

<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />  
<style type="text/css">
#customers
  {
  font-family:"Trebuchet MS", Arial, Helvetica, sans-serif;
  width:100%;
  border-collapse:collapse;
  text-align:left;
  }
  
#customers td, #customers th 
  {
  font-size:1em;
  border:1px solid #98bf21;
  padding:3px 7px 2px 7px;
  text-align:left;
  }

#customers th 
  {
  font-size:1.1em;
  text-align:left;
  padding-top:5px;
  padding-bottom:4px;
  background-color:#A7C942;
  color:#ffffff;
  }

#customers tr.alt td 
  {
  color:#000000;
  background-color:#EAF2D3;
  }
</style>
</head>

Java files as below:

MyTest.java

```java
package testng.groups;

import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

import testng.helper.Logger;

public class MyTest{

    @Test(groups = {"group3", "group2"}, dependsOnGroups = {"group4"})
    @Parameters({"group1A"})
    public void testGroup1A(String group1A) {
        Logger.print("This is testGroup1A.");
        Logger.print(group1A);
    }    
}
```

MyTestB.java

```

package testng.groups;

import org.testng.annotations.Test;

import testng.helper.Logger;

public class MyTestB{

    String[] name = this.getClass().getName().toString().split("\\.");

    @Test(groups = {"group4"})
    public void testB2() {
        String methodName = Thread.currentThread().getStackTrace()[1].getMethodName();
        String[] name = this.getClass().getName().toString().split("\\.");
        Logger.print("This is " + name[name.length - 1] + "." +  methodName);
        Logger.print("test logger");
    }
}

```

Logger.java
used for log printing with time mark.  

```
package testng.helper;

import java.util.Date;
import java.text.SimpleDateFormat;
import org.testng.annotations.Test;

public class Logger {

    final private static SimpleDateFormat date_format = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss:SS");
    
    public static void print(String message) {
        Date date= new Date();
        System.out.print(date_format.format(date));
        System.out.print("  ");
        System.out.println(message);
    }
}

```

xml file:
testGroups.xml

```
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="TestNgSuite" parallel="false">
    
    <test name="Test Groups">
        <groups>
            <run>
                <include name="group2" />
            </run>
        </groups>
        <classes>
            <class name="testng.groups.MyTest" />
            <class name="testng.groups.MyTestB" />
        </classes>
    </test>
</suite>
```

Results:

[TestNG] Running:
  C:\git\testng\mytest\src\main\resources\groups\testGroups.xml

2016-08-12 10:56:17:818  This is MyTestB.testB1  
2016-08-12 10:56:17:818  This is MyTestB.testB2  
2016-08-12 10:56:17:818  test logger  
2016-08-12 10:56:17:828  This is testGroup1A.  
2016-08-12 10:56:17:828  group1A

===============================================  
TestNgSuite  
Total tests run: 3, Failures: 0, Skips: 0  
===============================================

