This project includes a sample Fedora dataset and a tool to upload the dataset into a Fedora repository for demostration.

To import the sample dataset:

1. Download the fcrepo-sample-dataset tool: git clone git@github.com:futures/fcrepo-sample-dataset.git
2. Change to the fcrepo-sample-dataset directory: cd fcrepo-sample-dataset
3. Build: mvn clean install
4. Run: mvn -Dfcrepo.url=&lt;repo.url&gt; exec:java

The whole dataset will be loaded into the Fedora repository <repo.url>, or view the dataset from http://localhost:8080/rest by default. 

To load your own dataset, convert it to jcr/xml format and place the data files under src/main/resources/data directory, then run "mvn -Dfcrepo.url=&lt;repo.url&gt; exec:java" to load it to the repository.

- - -
## WebAC resources

To load default WebAC protected resources.

1. Download the fcrepo-sample-dataset tool: git clone https://github.com/fcrepo4-labs/fcrepo-sample-dataset.git
2. Change to the fcrepo-sample-dataset directory: cd fcrepo-sample-dataset
3. Build: mvn clean install
4. Run mvn -Dfcrepo.url=&lt;repo.url&gt; -Dfcrepo.authUser=&lt;admin username&gt; -Dfcrepo.authPassword=&lt;admin password&gt; -Presourceimport exec:java

This will load resources into your Fedora respository that support [these scenarios](https://wiki.duraspace.org/display/FEDORA4x/WebAC+Authorization+Delegate#WebACAuthorizationDelegate-ExampleScenarios).

System properties have these default settings:
* fcrepo.url = http://localhost:8080/fcrepo/rest/
* fcrepo.authUser = NULL
* fcrepo.authPassword = NULL

### WebAC layout
WebAC protected resources are linked to ACLs using the ```http://www.w3.org/ns/auth/acl#accessControl``` predicate. The layout looks like this:

    +-----------+  acl:accessControl  +----------+
    | protected |   ---------->       |  ACL 1   |
    | resource  |                     +----------+
    +-----------+                          | ldp:contains
                                          /|\
                                        /  |  \
                                      /    |    \
                            +-------+  +-------+  +-------+
                            | auth1 |  | auth2 |  | auth3 |
                            +-------+  +-------+  +-------+

Each authorization can reference a protected resource or child of the linked protected resource, either by path or by rdf:type.

### WebAC groups and fcrepo-vagrant
For testing using the [fcrepo-vagrant](https://github.com/fcrepo4-labs/fcrepo4-vagrant) setup you can pass **Group** information using the _some-header_ Header.

ie. To get a resource using the **Registrars** group.

```curl -utestuser:password1 -H"some-header: Registrar" http://localhost:8080/fcrepo/rest/restricted```