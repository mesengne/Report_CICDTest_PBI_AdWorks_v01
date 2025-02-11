
### Option 4 - CI/CD for ISVs in Fabric (managing multiple customers/solutions)

![image](https://github.com/user-attachments/assets/826d4fc7-1894-4f7b-a7d0-c0b6aaa18006)


This option is different from the others. It's most relevant for Independent Software Vendors (ISV) who build SaaS applications for their customers on top of Fabric. ISVs usually have a separate workspace for each customer and can have as many as several hundred or thousands of workspaces. When the structure of the analytics provided to each customer is similar and out-of-the-box, we recommend having a centralized development and testing process that splits off to each customer only in the *Prod* stage.

This option is based on [option #2](#option-2---git--based-deployments-using-build-environments). Once the PR to *main* is approved and merged:

1. A *build* pipeline is triggered to spin up a new *Build environment* and run unit tests for *dev* stage. When tests are complete, a *release* pipeline is triggered. This pipeline can upload the content to a *Build environment*, run scripts to change some of the configuration, adjust the configuration to *dev* stage, and then use Fabric’s [Update item definition](/rest/api/fabric/core/items/update-item) APIs to upload the files into the Workspace.
1. After this process is complete, including ingesting data and approval from release managers, the next *build* and *release* pipelines for *test* stage can kick off. This process is similar to that described in the first step. For *test* stage, other automated or manual tests might be required after the deployment, to validate the changes are ready to be release to *Prod* stage in high-quality.
1. Once all tests pass and the approval process is complete, the deployment to *Prod* customers can start. Each customer has its own release with its own parameters, so that its specific configuration and data connection can take place in the relevant customer’s workspace. The configuration change can happen through scripts in a *build* environment, or using APIs post deployment. All releases can happen in parallel as they aren't related nor dependent of each other.

#### When should you consider using option #4?

* You're an ISV building applications on top of Fabric.
* You're using different workspaces for each customer to manage the multi-tenancy of your application
* For more separation, or for specific tests for different customers, you might want to have multi-tenancy in earlier stages of *dev* or *test*. In that case, consider that with multi-tenancy the number of workspaces required grows significantly.
