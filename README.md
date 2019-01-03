# How to deploy Kong into Openshift

**Setup the project**

To configure Kong into openshift is necessary to create a project. The prooject is responsiblem to mantain all configuration about the Kong container.

````
oc new-project gateway-dev --display-name="Kong Dev"
````

Now we have to give access to a specific registry of **Red Hat**, is required own an account in the site _access.redhat.com_.

Create a secret to authorize the registry.
````
oc secrets new-dockercfg registry-connect-redhat --docker-server=registry.connect.redhat.com \
  --docker-username=$<email-login> --docker-password=$<password> --docker-email=$<email>
````

Add the secrets to the serviceaccount of project.
````
oc secrets add serviceaccount/default secrets/registry-connect-redhat --for=pull
oc secrets add serviceaccount/builder secrets/registry-connect-redhat
````

To import the Kong container execute the comando into of the project.
````
oc import-image kong --from=registry.connect.redhat.com/kong/kong --confirm
````

**Importing the Temaplate**
