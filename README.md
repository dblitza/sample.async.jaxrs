Sample Async JAX-RS 
==============

This sample contains a few variations to illustrate how to use async request processing in JAX-RS 2.0 applications. It is organized around a generic `Item`, that is manipulated by an `ItemService` that happens to take a long time (it sleeps for a few seconds). Each variation copes with this slow service in a different way, to illustrate how JAX-RS Async requests work and how Concurrency Utilities and EJBs can be used to offload work to other threads while keeping the EE container happy.

* *Items*: [ItemsResource](/src/main/java/net/wasdev/jaxrs/async/ItemsResource.java) uses the `ItemService` to list, add, and fetch items, in the usual (synchronous) way.
* *Async Items*: [ItemsAsyncResource](/src/main/java/net/wasdev/jaxrs/async/ItemsAsyncResource.java) uses the `@Suspended` annotation with the `AsyncResponse` parameter, however, it is still essentially synchronous, as the work is not offloaded to a different thread.
* *Async EJB Items*: [ItemsEJBResource](/src/main/java/net/wasdev/jaxrs/async/ItemsEJBResource.java) defines a stateless EJB, which allows work offloading with the addition of the `@Asynchronous` annotation.
* *Async Items with Executor*: [ItemsExecutorResource](/src/main/java/net/wasdev/jaxrs/async/ItemsExecutorResource.java) looks up the `ManagedExecutorService` defined by EE7 Concurrency Utilities in JNDI. Work for the request is then submitted to the executor and resumed later.
* *Async Items with CDI-provided Executor*: [ItemsCDIExecutorResource](/src/main/java/net/wasdev/jaxrs/async/ItemsCDIExecutorResource.java) uses the `ManagedExecutorService` injected as an @Resource.

`AsyncResponse` objects can be configured to handle timeouts. There are two examples showing that:
* *Async with CDI-provided Executor and Timeout*: [ItemsCDIExecutorResourceTimeout](/src/main/java/net/wasdev/jaxrs/async/ItemsCDIExecutorResourceTimeout.java) sets a timeout, and registers a timeout handler, a connection callback, and a completion callback. Work is queued to a separate thread via a CDI-injected ManagedExecutorService.
* *Async EJB Items with Timeout*: [ItemsEJBResourceTimeout](/src/main/java/net/wasdev/jaxrs/async/ItemsEJBResourceTimeout.java) does the same as the previous example, but uses an asynchronous Stateless EJB instead.



## Maven
### Running in Eclipse with Maven

1. Clone this project and import into Eclipse as an 'Existing Maven Project'.
2. Right-click the project and select **Run As > Run on Server**.
3. You should see the following in the console: `Application sample.spring started in XX.XX seconds.`

### Running with Maven command-line

This project can be built with [Apache Maven]. The project uses [Liberty Maven Plug-in] to automatically download and install the Liberty Java EE 7 Full Platform runtime from [Maven Central]. Liberty Maven Plug-in is also used to create, configure, and run the application on the Liberty server. 

Use the following steps to run the application with Maven:

1. Execute the full Maven build. The Liberty Maven Plug-in will download and install the Liberty runtime and create the server.
    ```bash
    $ mvn clean install
    ```

2. To run the server with the Spring application execute:
    ```bash
    $ mvn liberty:run-server
    ```

## Gradle
### Running in Eclipse with Gradle
1. Go to Help > Eclipse Marketplace > Install Buildship Gradle Integration 2.0.
2. Clone this project and import into Eclipse as an 'Existing Gradle Project'.
3. Go to Window > Show View > Other > Gradle Executions & Gradle Tasks.
4. Go to Gradle Tasks view and run clean in build folder, then build in build folder, then libertyStart in liberty folder.
5. You should see the following in the console: `Application sample.spring started in XX.XX seconds.`

 
### Running with Gradle command-line

This project can be built with [Gradle]. The project uses [Liberty Gradle Plug-in] to automatically download and install Liberty with Java EE 7 Full Platform runtime from [Maven Central]. Liberty Gradle Plug-in is also used to create, configure, and run the application on the Liberty server. 

Use the following steps to run the application with Gradle:

1. Execute full Gradle build. This will cause Liberty Gradle Plug-in to download and install Liberty profile server.
    ```bash
    $ gradle clean build
    ```

2. To run the server with the Spring application execute:
    ```bash
    $ gradle libertyStart
    ```
        
3. To stop the server with the Spring application execute:
    ```bash
    $ gradle libertyStop
    ```
 

In your browser, enter the URL for the application: [http://localhost:9080/jaxrs-async/](http://localhost:9080/async-jaxrs/) (where port 9080 assumes the httpEndpoint provided in the sample server.xml has not been modified).
In your browser, you should see the phone book displayed.

# Notice

© Copyright IBM Corporation 2017.

# License

```text
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
````

[Liberty Maven Plug-in]: https://github.com/WASdev/ci.maven
[Liberty Gradle Plug-in]: https://github.com/WASdev/ci.gradle

[Apache Maven]: http://maven.apache.org
[Gradle]: https://gradle.org/

[Maven Central]: https://search.maven.org/

