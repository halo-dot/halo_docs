### Getting Started with Intents

Pick the Intent Integration Method. Get required information from the <ins>[Merchant Portal](https://go.merchantportal.prod.haloplus.io/auth/login)</ins>.

##### 1. Intent Integration Methods
The Halo.Go application currently provides different mechanisms to integrate and try our payments solution depending on your use-case. The different methods are:

  1. Android Intents Mechanism
  2. Deep linking Mechanism

Have a look at the image below as a guideline:

<img src="assets/docs/app-to-app/android_vs_deeplinking.png" alt="Android vs Deeplinking" width="600"/>


##### 2. Intent Authorization
Retrieve your ```Merchant ID``` and ```API Key``` from the Merchant Portal.

###### Retrieve details from the Merchant Portal
Get Merchant ID: The ```Merchant ID``` can easily be found on the Halo.Go Portal. From the Portal, navigate to Users. There, you will see your unique ```Merchant ID```.

Get x-api-key: In the user table generate your ```API Key```. Note the warning pop-up when generating an ```API Key```.

<img src="assets/docs/app-to-app/api_key_generate.png" alt="API Key" width="400"/>

Since an ```API Key``` is a sensitive value, it is stored as an encrypted value in our database and only presented in clear text once during the initial retrieval. It is the responsibility of the user to keep their ```API Key``` safe.

<img src="assets/docs/app-to-app/api_key_success.png" alt="API Key" width="400"/>


##### 3. Intent Use Cases
With Halo Dot the exact same technology can be used for different use cases. However, the integration is slightly different to allow us to distinguish the use cases. The use cases and their specific integration guides are:

- <ins>[Transactions / Payments Acceptance](https://halo-dot-developer-docs.gitbook.io/halo-dot/readme/transaction-app2app-integration-guide)</ins>.
- <ins>[TT3 / Debicheck](https://halo-dot-developer-docs.gitbook.io/halo-dot/readme/tt3-app2app-integration-guide)</ins>.

<br />

##### 4. Conclusion
Not what you are looking for? To get started with the Halo.SDK, have a look at this <ins>[guide](https://halo-dot-developer-docs.gitbook.io/halo-dot/sdk/1.-getting-started)</ins>.
