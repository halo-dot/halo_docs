#### Halo.SDK Integration Guide

Welcome to the Halo.SDK integration guide! Follow these steps to quickly integrate the Halo.SDK into your Android application.

##### Getting Started

To start integrating the Halo.SDK into your app, you will need to add it as a dependency in your project.

Both debug and release versions of the SDK are available. The release version is suitable for production use. The debug version is intended for development and testing purposes; it has full logging enabled and allows a debugger to be attached to the app.

The Halo.SDK is hosted in a Maven repository, stored in an S3 bucket in a Halo AWS account.

Before you begin, ensure you have the following:

- **<a href="https://developer.android.com/studio" target="_blank">Android Studio</a>:** You will need Android Studio installed on your system to run the test app.

---

###### Your Issuer Name and Public Key

{{VIEW_ISSUER_NAME}}

---

###### Your Access Credentials

{{VIEW_ACCESS_KEY}}

---

##### Setup

1. **Download the Test App**

    Download our <a href="https://github.com/halo-dot/halo_test_app" target="_blank">test app</a> from GitHub.

<br/>

2. **Configure Your App**

    - Open `za/co/synthesis/halo/halotestapp/Config.kt` and replace the placeholder values of `PRIVATE_KEY_PEM`, `ISSUER`, and `USERNAME` with your own values. You will need the private key you used to generate your public key, your issuer name, and your username (you can use the email or phone number used to register).

        ```kotlin
        object Config {
            const val PRIVATE_KEY_PEM = "your_private_key"
            const val ISSUER = "issuer.claim"
            const val USERNAME = "username"
            const val MERCHANT_ID = "mer12345678"
            const val HOST = "kernelserver.qa.haloplus.io"
        }
        ```

    - Open the `local.properties` file and replace the placeholder values of `aws.accessKey` and `aws.secretKey`. These credentials are sensitive and should not be committed to source control. Add the credentials into a `local.properties` file:

        ```properties
        sdk.dir=~/Library/Android/sdk
        aws.accessKey=your_access_key
        aws.secretKey=your_secret_key
        ```

        `sdk.dir` specifies the location of the Android SDK on your file system.

<br/>

3. **Gradle Setup**

    After a Gradle sync, you should be able to import from the `za.co.synthesis.halo.sdk` namespace:

    ```kotlin
    import za.co.synthesis.halo.sdk.HaloSDK
    ```

---

##### Additional Configuration

1. **Read AWS Credentials from `local.properties`**

    This code snippet reads AWS credentials from a `local.properties` file and sets them as project properties in the Gradle build:

    ```gradle
    ext {
        Properties properties = new Properties()
        def propertiesFile = project.rootProject.file('local.properties')
        if (propertiesFile.exists()) {
            properties.load(propertiesFile.newDataInputStream())
        }

        accessKey = properties.getProperty('aws.accessKey')
        secretKey = properties.getProperty('aws.secretKey')
        tokenKey = System.getenv('AWS_SESSION_TOKEN')
    }
    ```
<br/>

2. **Maven Repository Configuration**

    This configuration sets up Maven repositories hosted in an S3 bucket in a Halo AWS account:

    ```gradle
    repositories {
        def repos = ['releases', 'snapshots']
        repos.each { repo ->
            maven {
                name = repo
                url = "s3://synthesis-halo-artifacts/$repo"

                credentials(AwsCredentials) {
                    accessKey = rootProject.ext.accessKey
                    secretKey = rootProject.ext.secretKey
                    if (tokenKey != null) {
                        sessionToken = rootProject.ext.tokenKey
                    }
                }
            }
        }
    }
    ```
<br/>

3. **SDK Dependencies**

    This code snippet configures the Halo.SDK dependencies for your project in the app-level `build.gradle`:

    ```gradle
    dependencies {
        // Declare dependencies for the Halo SDK
        debugImplementation group: 'za.co.synthesis.halo', name: 'sdk', version: '2.1.26-debug'
        releaseImplementation group: 'za.co.synthesis.halo', name: 'sdk', version: '2.1.26'
    }
    ```

Now, you are ready to start using the Halo.SDK in your Android application!
