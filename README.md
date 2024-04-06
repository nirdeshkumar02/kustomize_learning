## Kustomize

Kustomize is a Kubernetes configuration transformation tool that enables you to customize untemplated YAML files, leaving the original files untouched. Kustomize can also generate resources such as ConfigMaps and Secrets from other representations.

Example - Using Kustomize; We can deploy different configuration without changing base configuration to different env (Dev, Stage or Prod).

<img title="Kustomize with GitOps" alt="Deploying different configuration to each Env server using Kustomize" src="/img.png">

## Benefits of Using Kustomize

Kustomize for kubernetes offers several advantages for managing Kubernetes configurations:

**Separation of Concerns:** Kustomize promotes a clear separation between base configurations and overlays. This separation makes it easier to maintain a common set of configurations while accommodating environment-specific changes.

**Reusability:** You can create reusable bases and overlays, reducing duplication and ensuring consistency across multiple applications or environments.

**Environment-Specific Customization:** Kustomize simplifies the process of customizing configurations for different environments, such as development, staging, and production.

**Patch Management:** Kustomize allows you to apply patches to resources, enabling fine-grained customization without modifying the original manifests.

**Version Control:** Since Kustomize operates on declarative configurations, you can version-control your customization instructions, making it easy to track changes and roll back if needed.

**Integration with CI/CD:** Kustomize seamlessly integrates into CI/CD pipelines, automating configuration management and ensuring consistency during deployment.

## Practical Usage

**Building a Kustomize Base** 

1. Start by creating a base directory containing your base configuration. This directory should include all the common YAML files for your application.
 
2. Define a Kustomization file (kustomization.yaml) in the base directory to specify the resources and transformations to apply.
 

**Creating Overlays for Environment-Specific Configurations**

1. For each environment or variation, create an overlay directory that includes only the YAML files that need customization.
 
2. In the overlay directory, define a Kustomization file to specify which base to use and any additional transformations required.
 

**Customizing Resources with Kustomize Transformers**

1. Use Kustomize transformers such as patches, replacements, and field setters to customize specific resources.
 
2. Transformers are defined in the Kustomization file of the overlay or base.

## To know more, Use Kustomize Docs - **`https://kubectl.docs.kubernetes.io/guides/`**

## Kustomize Practicle -
We have some basic k8s configuration under base folder. Each configuration is placed under their respective folders - api, db and monitoring. 

We have components folder having auth and logging related docs which will be generic to all env. 

Last, We have Overlays folder; where we have patch configuration for different env like dev, stage and prod. 

Here are some practicle question that I've attempted as for knowledge shake - 
1. Assign a Label Transformer to add the label defined below to every resource across all 3 environments: **project: lambda** [Only modify one Kustomization.yaml file].

2. Please make the necessary modifications so that all resources in the prod environment should be suffixed with the word **-prod**.

3. For development environment: Perform an **inline json6902 patch** to change the image of the db container in the db-deployment to be a mongo image instead of postgres.

4. For the production environment, change the replicaCount of the api-deployment to 5 [Use a strategic merge patch in a seperate file called api-patch.yaml].

5. Create a secretGenerator called db-secret with the following literals: `port=5432` and `password=mypass` then Apply an env variable to the db-deployment container called POSTGRES_PASSWORD and set the value to be the value of the password key in the newly created db-secret secretGenerator [This db secret will be common for all env so create it in /base/db/kustomization.yaml file]. 

6. In the previous section, a secretGenerator was created with the two following literals: `port=5432` and `password=mypass`. Now; In the dev overlay, update the secretGenerator so the **password=mypass** is changed to **password=devpassword** [The port literal should remain untouched.].

7. Now let's add the auth component to the prod overlay and the logging component to the dev overlay.

8. We want to modify the postgres container default configuration only in our staging environment. The database team has created a `postgresql.conf` file in the staging overlay directory that contains all the custom configuration.
Load the configuration in a configMapGenerator called db-config and mount it in the db container. To add the mount, create a Strategic Merge Patch in a separate file called db-patch.yaml **(This should be done in the staging overlay only)**. Also, create a volume and volume mount called config-volume [The mountPath should be /var/lib/postgresql/config/].

### You can find the answers in above folders. 
