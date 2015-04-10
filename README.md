# Maven Core Extensions Example for Safe Parallel Builds

If you a parallel build with Maven, using the `-T` option, then the only safe way to do this is using the Takari Local Repository implementation that is thread and process safe. Maven's dependency resolution mechanism works on a per-project basis where just prior to building a project its dependencies are downloaded. If you have two projects building simultaneously that require the same dependency and both download that dependency at the same time, if you're not using a mechanism to synchonrize that dependency being downloaded you run the risk of corrupting your local Maven repository.

This example uses the new Core Extension Mechanism in Maven 3.3.1 to enable the Takari Smart Builder and the Takari Local Repository implementation to give you more efficient parallel builds with the required local repository safeguards.

To enable the the Smart Builder and thread/process-safe local repository implementation just copy the `.mvn` directory into your project and give it a try!
