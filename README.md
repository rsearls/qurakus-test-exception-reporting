# qurakus-test-exception-reporting


All types of exceptions thrown during testing using quarkus are not getting
reported, (e.g. ClassNotFound, ServiceConfigurationError, RuntimeExceptions,
java.net.ConnectException). 
Almost all errors bubble up through io.quarkus.arc.Arc.initialize(Arc.java:26)
and are caught in  org.jboss.arquillian.core.impl.ObserverImpl  line 89,
however at no time in the call stack is the exception information being
printed to the console.  All the user sees is a test result of
   [INFO] Tests run: 0, Failures: 0, Errors: 0, Skipped: 0

In addition the true cause is hidden in layers of wrapping exceptions.  See
attached image (quarkus-cause-image.jpg) of a typical cause stack.  This image was generated by  
setting a breakpoint at ObserverImpl line 90 and commenting out exclude statement
in pom.xml at line 599 for test org/jboss/resteasy/test/client/InputStreamTest.java.
Test run with cmd 
    mvn clean test -DskipTests=true
    mvn test -Dmaven.surefire.debug


Execution env.
    jdk 11.0.2
    mvn 3.6.0
    quarkus 1.3.0.Alpha1



Before running the test install this archive in your local repo.
Here is the cmd to do it.

mvn install:install-file \
   -Dfile=___FIX_THIS___/lib/utils-arquillian-utils-4.5.0-SNAPSHOT.jar \
   -DgroupId=org.jboss.resteasy \
   -DartifactId=utils-arquillian-utils \
   -Dversion=4.5.0-SNAPSHOT \
   -Dpackaging=jar 



  