Java-Runtime-Compiler
=====================

This takes a String, compiles it and loads it returning you a class from what you built.  
By default it uses the current ClassLoader.  It supports nested classes, but otherwise builds one class at a time.

## On maven central

You can include in your project with

    <dependency>
        <groupId>net.openhft</groupId>
        <artifactId>compiler</artifactId>
        <version>2.2.1</version>
    </dependency>
    
You can find the latest version here [maven search for net.openhft:compiler](http://search.maven.org/#browse%7C842970587)

## Simple example

You need a CachedCompiler and access to your JDK's tools.jar.

     // dynamically you can call
     String className = "mypackage.MyClass";
     String javaCode = "import mypackage;\n" +
                      "public class MyClass implements Runnable {\n" +
                      "    public void run() {\n" +
                      "        System.out.println("\"Hello World\");\n" +
                      "    }\n" +
                      "}\n";
     Class aClass = CompilerUtils.CACHED_COMPILER.loadFromJava(className, javaCode);
     Runnable runner = (Runnable) aClass.newInstance();
     runner.run();
     
I suggest making you class implement a KnownInterface of your choice as this will allow you to call/manipulate instances of you generated class.

Another more hacky way is to use this to override a class, provided it hasn't been loaded already.  
This means you can redefine an existing class and provided the methods and fields used match,
you have compiler redefine a class and code already compiled to use the class will still work.

## Using the CachedCompiler.

In this example, you can configure the compiler to write the files to a speicifc directory when you are in debug mode.
       
    private static final CachedCompiler JCC = CompilerUtils.DEBUGGING ?
                                                       new CachedCompiler(new File(parent, "src/test/java"), new File(parent, "target/compiled")) :
                                                       CompilerUtils.CACHED_COMPILER;
     
By selecting the src directory to match where your IDE looks for those files, it will allow your debugger to set into code you have generated at runtime.

Note: you may need to delete these files if you want to regenerate them.