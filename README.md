# Maven Core Extensions Example for Safe Parallel Builds

If you are using multiple threads and building projects in parallel with Maven, then the only safe way to do this is using the [Takari Local Repository implementation][1]. The Takari Local Repository implementation is thread and process safe by employing a form of advisory locking that makes sure an artifact with a particular Maven coordinate is handled properly. 

Maven's dependency resolution mechanism works on a per-project basis where just prior to building a project its dependencies are downloaded. If you have two projects building simultaneously that require the same dependency and both download that dependency at the same time, if you're not using a mechanism to synchonrize that dependency being downloaded you run the risk of corrupting your local Maven repository.

This example uses the new Core Extension Mechanism in Maven 3.3.1 to enable the [Takari Smart Builder][2], the [Takari Local Repository][1], and the [Takari OkHttp Aether Connector][3] to give you more efficient parallel builds with the required local repository safeguards and efficient artifact downloading.

To enable all these extensions just copy the `.mvn` directory into your project and give it a try! The `.mvn/extensions.xml` file looks like the following:

```
<?xml version="1.0" encoding="UTF-8"?>
<extensions>
  <extension>
    <groupId>io.takari.maven</groupId>
    <artifactId>takari-smart-builder</artifactId>
    <version>0.6.1</version>
  </extension>
  <extension>
    <groupId>io.takari.aether</groupId>
    <artifactId>takari-concurrent-localrepo</artifactId>
    <version>0.0.7</version>
  </extension>
  <extension>
    <groupId>io.takari.aether</groupId>
    <artifactId>aether-connector-okhttp</artifactId>
    <version>1.0.1-alpha</version>
  </extension>
</extensions>

```

And the `.mvn/maven.config looks like the following:

```
--T 8
--builder smart
```

Where you want to enable multiple threads and tell Maven to use the [Takari Smart Builder][2].


[1]: https://github.com/takari/takari-local-repository
[2]: https://github.com/takari/takari-smart-builder
[3]: https://github.com/takari/aether-connector-okhttp
