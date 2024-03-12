### Accessing the SDK

The SDK is hosted in a maven repo, hosted in an S3 bucket in a Halo AWS account.

A debug version of the SDK is made available to support development efforts, but only the release version will be permitted to transact in production.The debug version has full logging enabled and allows a debugger to be attached to the app.

##### Access Maven Repo
Your AWS access key and secret key will be provided to you separately. These are sensitive and should not be committed to source control. Add the credentials into a local.properties file:

{{SECRET_ACCESS_KEYS}}

<br />

Add the following to your project-level gradle file, to read the access credentials into variables:

```gradle
ext {
    Properties properties = new Properties()
    def propertiesFile = project.rootProject.file('local.properties')
    if (propertiesFile.exists()) {
      properties.load(propertiesFile.newDataInputStream())
    }
    def localAccessKey = properties.getProperty('aws.accesskey')
    def systemEnvAccessKey = System.getenv('AWS_ACCESS_KEY_ID')

    def localSecretKey = properties.getProperty('aws.secretkey')
    def systemEnvSecretKey = System.getenv('AWS_SECRET_ACCESS_KEY')

    accessKey = localAccessKey != null ? localAccessKey : systemEnvAccessKey
    secretKey = localSecretKey != null ? localSecretKey : systemEnvSecretKey
  }
```

<br />

Add the following to your module-level gradle file, to pull the artifacts:
  1. Snapshots: Debug builds
  2. Release: Release builds

```gradle
  repositories {
    def repos = [
      'releases',
      'snapshots'
    ]

    repos.each {repo ->
      maven {
        name = repo
        url = "s3://synthesis-halo-artifacts/$repo"
        credentials(AwsCredentials) {
          accessKey = rootProject.ext.accessKey
          secretKey = rootProject.ext.secretKey
        }
      }
    }
  }          
```

<br />

Finally, add the following to your build.gradle:

```gradle
  configurations.all {
    resolutionStrategy.cacheChangingModulesFor 1, 'days'
    resolutionStrategy.dependencySubstitution {
      substitute(module("androidx.core:core-ktx")).with(module("androidx.core:core-ktx:(*, 1.3.2]"))
      substitute(module("org.jetbrains.kotlin:kotlin-stdlib-jdk7")).with(module("org.jetbrains.kotlin:kotlin-stdlib-jdk7:(*, 1.3.72]"))
      substitute(module("org.jetbrains.kotlin:kotlin-stdlib-jdk8")).with(module("org.jetbrains.kotlin:kotlin-stdlib-jdk7:(*, 1.3.72]"))
      substitute(module("org.jetbrains.kotlin:kotlin-stdlib-common")).with(module("org.jetbrains.kotlin:kotlin-stdlib-common:(*, 1.3.72]"))
    }
  }
```

<br />

After a gradle sync, you should now be able to import from the za.co.synthesis.halo.sdk namespace, e.g:

```kotlin
  import za.co.synthesis.halo.sdk.HaloSDK
```

<br />

For a more technical integration guide, see the <ins>[next guide](https://halo-dot-developer-docs.gitbook.io/halo-dot/sdk/2.-sdk-integration-guide)</ins>.
