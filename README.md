# apigee-config-maven-plugin

Maven plugin to create Apigee config like Cache, Target Server, Developers, Developer Apps that can be used alongwith the apigee-deploy-maven-plugin to deploy a working API in one go.

A new config file edge.json is used to hold all the config data to create the corresponding entities in Apigee Edge.

The org and env information comes from the maven profile and helps deploy an API and the associated config to any environment just by using the right profile name in the command line.

This is the plugin source code project. Refer to samples folder for plugin usage samples.

## Plugin Usage
```
mvn install -Ptest -Dusername=${APIGEE_USER} -Dpassword=${APIGEE_PASS} -Dapigee.config.options=create

  # Options

  -P<profile>
    used to pick a profile in the parent pom.xml (shared-pom.xml in the example)
  

  -Dapigee.config.options
    none   - No action (default)
    create - Create when not found. Existing config is not refreshed even if it is different.
    update - Update when found; create when not found. Refreshes all config to reflect edge.json.
```
The default "none" action is a NO-OP and it helps deploy APIs (using [apigee-deploy-maven-plugin](https://github.com/apigee/apigee-deploy-maven-plugin)) without affecting config.

## Sample project
Refer to an example API project at [/samples/EdgeConfig](https://github.com/apigee/apigee-config-maven-plugin/tree/master/samples/EdgeConfig)

This project demonstrates the creation and management of Apigee Edge Config and performs the following steps in sequence.
  - Creates Caches
  - Creates Target servers
  - Creates API products
  - Creates Developers
  - Creates Developer Apps

To use, edit samples/EdgeConfig/shared-pom.xml, and update org and env elements in all profiles to point to your Apigee org, env. You can add more profiles corresponding to each env in your org.

      <apigee.org>myorg</apigee.org>
      <apigee.env>test</apigee.env>

To run jump to samples project `cd /samples/EdgeConfig` and run 

`mvn install -Ptest -Dusername=<your-apigee-username> -Dpassword=<your-apigee-password> -Dapigee.config.options=create`

Refer to [samples/APIandConfig/HelloWorld](https://github.com/apigee/apigee-config-maven-plugin/tree/master/samples/APIandConfig/HelloWorld) for config management alongwith API deployment using [apigee-deploy-maven-plugin](https://github.com/apigee/apigee-deploy-maven-plugin). More info at [samples/README.md](https://github.com/apigee/apigee-config-maven-plugin/blob/master/samples/README.md)

## edge.json - v1.0
edge.json contains all config entities to be created in Apigee Edge. It is organized into 3 scopes corresponding to the scopes of config entities that can be created in Edge.
   ```
     envConfig
     orgConfig
     apiConfig
   ```
For example, API product is an orgConfig whereas Cache is an envConfig. The envConfig is further organized into individual environments as follows.
   ```
     envConfig.test
     envConfig.dev
     envConfig.pre-prod
   ```
The environment name is intended to be the same as the profile name in pom.xml (parent-pom.xml or shared-pom.xml).

The final level is the config entities. 
   ```
     orgConfig.apiProducts[]
     orgConfig.developers[]
     orgConfig.developerApps[]
     envConfig.test.caches[]
     envConfig.test.targetServers[]
   ```

Each config entity is the payload of the corresponding management API. The example project may not show all the attributes of config entities, but any valid input of a management API would work.

Config entities like "developerApps" are grouped under the developerId (email) they belong to.

## TODO
  - KVM (encrypted data support)
  - Vault (encrypted data support)
  - Keystore, Truststore support
  - Virtual host (on-prem)
  - Custom roles
  - User role association

More TODOs captured as [issues](https://github.com/apigee/apigee-config-maven-plugin/issues)

## Contributing
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History
| Date | Version | Notes |
| ----- | ------ | ----- |
| 12 May 2016 | [Version 1.0](https://github.com/apigee/apigee-config-maven-plugin/releases/tag/apigee-config-maven-plugin-1.0) | First release. Supports Cache, Target Servers, API products, Developers, Developer Apps |

## Authors
  * Madhan Sadasivam
  * Prashanth K S
  * Meghdeep Basu

## License
* see [LICENSE](https://github.com/apigee/apigee-config-maven-plugin/blob/master/LICENSE) file

## Support
* Post a question in [Apigee community](https://community.apigee.com/index.html)
* Create an [issue](https://github.com/apigee/apigee-config-maven-plugin/issues/new)
