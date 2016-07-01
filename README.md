---
services: azure-resource-manager
platforms: java
author: alvadb
---

# Deploy an SSH Enabled VM with a Template in Node.js

This sample explains how to use Azure Resource Manager templates to deploy your resources to Azure
the Azure SDK for Java.

When deploying an application definition with a template, you can provide parameter values to customize how the
resources are created. You specify values for these parameters either inline or in a parameter file.

** On this page**

- [Running this sample](#run)
- [What is index.js doing?](#example)
  - [Deploy the template](#deploy)

<a id="run"></a>
## Running this sample

1. Set the environment variable `AZURE_AUTH_LOCATION` with the full path for an auth file. See [how to create an auth file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md).

2. Clone the repository.

```
git clone https://github.com/Azure-Samples/resources-java-deploy-using-arm-template.git
```

3. Run the sample

```
cd resources-java-deploy-using-arm-template
mvn clean compile exec:java
```

<a id="example"></a>
## What is DeployUsingARMTemplate.java doing?

The sample starts by setting up some variables.

```java
final String rgName = ResourceNamer.randomResourceName("rgRSAT", 24);
final String deploymentName = ResourceNamer.randomResourceName("dpRSAT", 24);
```

Then it logs in to the account using your authentication file.

```java
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure.configure()
        .withLogLevel(HttpLoggingInterceptor.Level.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

It creates a resource group into which it will deploy the template.

```java
azure.resourceGroups().define(rgName)
    .withRegion(Region.US_WEST)
    .create();
```

<a id="deploy"></a>
### Deploy the template

```java
azure.deployments().define(deploymentName)
    .withExistingResourceGroup(rgName)
    .withTemplate(templateJson)
    .withParameters("{}")
    .withMode(DeploymentMode.INCREMENTAL)
    .create();
```

## More information ##

[http://azure.com/java] (http://azure.com/java)

If you don't have a Microsoft Azure subscription you can get a FREE trial account [here](http://go.microsoft.com/fwlink/?LinkId=330212)

---

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.