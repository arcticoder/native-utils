# Native Utils
A simple library class which helps with loading dynamic libraries stored in the JAR archive. These libraries usualy contain implementation of some methods in native code (using JNI - Java Native Interface).

## Notes
 * The temporary file is stored into temp directory specified by java.io.tmpdir (by default it’s the operating system’s temporary directory). It should be automatically deleted when the application exits.
 * Although the code has some try-finally section (to be sure that streams are closed properly in case an exception is thrown), it does not catch exceptions. The exception has to be handled by the application. I belive this approach is cleaner and has some benefits.

## Usage
To load the dynamic library, just make sure it is packed inside the JAR archive and call method loadLibraryFromJar:

    import cz.adamh.NativeUtils;
     
    public class HelloJNI {  
        static {   
            try {    
                NativeUtils.loadLibraryFromJar("/resources/libHelloJNI.so");   
            } catch (IOException e) {
                // This is probably not the best way to handle exception :-)    
                e.printStackTrace();
            }    
        }  
     
        public native void hello();    
    }

## Building C library tests
```
$ cd src/test/c
$ gcc -c -Wall -Werror -fpic ctest.c
$ gcc -shared -o libctest.so ctest.o
$ gcc -L$(pwd) -Wall -o test main.c -lctest
```

## Running C library tests using C
```
$ cd src/test/c
$ export LD_LIBRARY_PATH=$(pwd):$LD_LIBRARY_PATH
$ ./test
```
## Running C library tests using Maven
```
$ sudo apt install -y maven
$ mvn package
```
## and/or
```
$ sudo apt install -y maven
$ mvn -Dtest=NativeUtilsTest#testLoadCLibraryLoad test
$ mvn -Dctest_int=NativeUtilsTest#testLoadCLibraryInt ctest_int
```

## More information
More information can be found in accompanying [blog post][1].

[1]: https://www.adamheinrich.com/blog/2012/12/how-to-load-native-jni-library-from-jar/
