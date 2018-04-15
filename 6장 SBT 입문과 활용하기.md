# 6장 SBT 입문과 활용하기

## 리스트 6-1
```
package example

object Hello extends Greeting with App {
    println(greeting)
}

trait Greeting {
    lazy val greeting: String = "hello"
}
```

## 리스트 6-2
```
package example

object Hello {

    def main(args: Array[String]){
        println()
        println("scala =================")
        println("Welcome to Scala world!")
        println("scala =================")
        println()
    }
}
```

## 리스트 6-3
```
name := "sbt-app"
version := "0.0.1-SNAPSHOT"
organization := "com.tuyano"
```

## 리스트 6-4
```
addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "5.2.4")
```

## 리스트 6-5
```
addSbtPlugin("com.github.mpeltonen" % "sbt-idea" % "1.6.0")
```

## 리스트 6-6
```
name := "sbt-app"
version := "0.0.1-SNAPSHOT"
organization := "com.tuyano"
```

## 리스트 6-7
```
lazy val root = (project in file(".")).
    settings(
        name := "sbt-app",
        version := "0.0.1-SNAPSHOT",
        organization := "com.tuyano"
    )
```

## 리스트 6-8
```
lazy val root = (project in file(".")).
    settings(
        name := "SBT-APP"
    )
```

## 리스트 6-9
```
val mytask = taskKey[Unit]("sample task.")

lazy val root = (project in file(".")).
    settings(
        name := "SBT-APP",

        mytask := {
            println()
            println("mytask=================")
            println("Hello! this is mytask!")
            println("project name is\"" + name.value + "\".")
            println("mytask=================")
            println()
        }
    )
```

## 리스트 6-10――App.java
```
package com.tuyano.sbt;

public class App {

    public static void main(String[] args){
        System.out.println("This is java application!");
    }
}
```

## 리스트 6-11――AppTest.java
```
package com.tuyano.sbt;

import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class AppTest {

    @Test
    public void testApp() {
        assertTrue(true);
    }
}
```

## 리스트 6-12――build.sbt
```
lazy val root = (project in file(".")).
    settings(
        name := "SBT-APP",
        libraryDependencies ++= Seq(
            "junit" % "junit" % "4.12"  % "test"
        )
    )
```

## 리스트 6-13
```
libraryDependencies ++= Seq(
    "junit" % "junit" % "4.12"  % "test",
     "com.novocode" % "junit-interface" % "0.11" % "test"
)
```

## 리스트 6-14
```
import sbt._

object Dependencies {
    lazy val junit = "junit" % "junit" % "4.12"  % "test"
    lazy val junitInterface = "com.novocode" % "junit-interface" % "0.11" % "test"
}
```

## 리스트 6-15
```
import Dependencies._

lazy val root = (project in file(".")).
    settings(
        name := "SBT-APP",
        libraryDependencies ++= Seq(
            junit,
            junitInterface
        )
    )
```

## 리스트 6-16――index.jsp
```
<html>
    <head>
        <title>index.jsp</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <%= new java.util.Date().toString() %>
    </body>
</html>
```

## 리스트 6-17――MyServlet.java
```
package com.tuyano.web;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/serv")
public class MyServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request,
            HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.println("<html> <head></head>");
        out.println("<body><h1>Hello!</h1>");
        out.println("<p>this is sample servlet.</p>");
        out.println("</body></html>");
    }
}
```

## 리스트 6-18――SampleServlet.scala
```
package com.tuyano.web

import java.util.Date
import javax.servlet.annotation.WebServlet
import javax.servlet.http.{
    HttpServlet, HttpServletRequest,
    HttpServletResponse}

@WebServlet(Array("/sample"))
class SampleServlet extends HttpServlet {

    override def doGet(request: HttpServletRequest,
            response: HttpServletResponse) = {

        val s = response.getOutputStream
        s.print("<html><head></head><body>")
        s.print("<h1>Scala Servlet</h1>")
        s.print("<p>" + new Date().toString + "</p>")
        s.print("</body></html>")
        s.flush()
    }
}
```

## 리스트 6-19――web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
            http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         id="serv" version="3.0">

    <servlet>
        <servlet-name>sample</servlet-name>
        <servlet-class>com.tuyano.web.SampleServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>sample</servlet-name>
        <url-pattern>/scala/sample</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>myserv</servlet-name>
        <servlet-class>com.tuyano.web.MyServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>myserv</servlet-name>
        <url-pattern>/java/serv</url-pattern>
    </servlet-mapping>

</web-app>
```

## 리스트 6-20――build.sbt
```
enablePlugins(JettyPlugin)

lazy val root = (project in file(".")).
    settings(
        name := "SBT-WEB-APP",
        libraryDependencies ++= Seq(
            "javax.servlet" % "javax.servlet-api" % "3.1.0" % "provided"
        ),
        containerPort := 8080
    )
```

## 리스트 6-21――build.properties
```
sbt.version=1.1.1
```

## 리스트 6-22――plugins.sbt
```
addSbtPlugin("com.earldouglas" % "xsbt-web-plugin" % "4.0.1")
```

## 리스트 6-23――App.java
```
package com.tuyano.spring;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAutoConfiguration
@ComponentScan
public class App implements CommandLineRunner {

    @Override
    public void run(String... args) {
        System.out.println("run!");
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(App.class, args);
    }
}
```

## 리스트 6-24――HeloController.java
```
package com.tuyano.spring;

import java.util.Date;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @RequestMapping("/")
    public String index(){
        return "Welcome to Spring Boot Application!! (" +
        new Date().toString() + ")";
    }
}
```

## 리스트 6-25――build.sbt
```
enablePlugins(JettyPlugin)

lazy val root = (project in file(".")).
    settings(
        name := "SBT-SPRING-APP",

        libraryDependencies ++= Seq(
            "org.springframework.boot" % "spring-boot-starter-web" %
                "1.5.10.RELEASE" exclude("org.springframework.boot", "spring-boot-starter-tomcat"),
            "org.springframework.boot" % "spring-boot-starter-jetty" %
                "1.5.10.RELEASE" % "provided",
            "org.springframework.boot" % "spring-boot-starter-test" %
                "1.5.10.RELEASE" % "test",
            "javax.servlet" % "javax.servlet-api" % "3.1.0" % "provided"
        ),
        containerPort := 8080
    )
```

## 리스트 6-26――HomeController.java
```
package controllers;

import play.mvc.*;
import views.html.*;

public class HomeController extends Controller {

    public Result index() {
        return ok(index.render("This is sample PLAY application!"));
    }

}
```

## 리스트 6-27――index.scala.html
```
@(message: String)

<!DOCTYPE html>
<html>
<head></head>
<body>
    <h1>Index</h1>
    <p>@message</p>
</body>
</html>
```

## 리스트 6-28――routes
```
GET / controllers.HomeController.index
```

## 리스트 6-29――build.sbt
```
name := "SBT-PLAY-APP"

enablePlugins(PlayJava)

lazy val root = (project in file(".")).
    settings(
        scalaVersion := "2.11.7",
        libraryDependencies ++= Seq(
            guice
        )
    )
```

## 리스트 6-30――build.properties
```
sbt.version=1.1.1
```

## 리스트 6-31――plugins.sbt
```
addSbtPlugin("com.typesafe.play" % "sbt-plugin" % "2.6.11")
```

## 리스트 6-32
```
name := """play-java"""

version := "1.0-SNAPSHOT"

lazy val root = (project in file(".")).enablePlugins(PlayJava)

scalaVersion := "2.11.7"

libraryDependencies ++= Seq(
    javaJdbc,
    cache,
    javaWs
)

fork in run := true
```