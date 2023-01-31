# ID for Identity Providers Reference Implementation

[![](https://developer.mastercard.com/_/_/src/global/assets/svg/mcdev-logo-dark.svg)](https://developer.mastercard.com/)

[![](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)
[![](https://sonarcloud.io/api/project_badges/measure?project=Mastercard_mastercard-idservice-idp-reference-app&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=Mastercard_mastercard-idservice-idp-reference-app)
[![](https://sonarcloud.io/api/project_badges/measure?project=Mastercard_mastercard-idservice-idp-reference-app&metric=coverage)](https://sonarcloud.io/summary/new_code?id=Mastercard_mastercard-idservice-idp-reference-app)
[![](https://sonarcloud.io/api/project_badges/measure?project=Mastercard_mastercard-idservice-idp-reference-app&metric=vulnerabilities)](https://sonarcloud.io/summary/new_code?id=Mastercard_mastercard-idservice-idp-reference-app)

## Table of Contents
- [Overview](#overview)
  * [References](#references)
- [Usage](#usage)
  * [Prerequisites](#prerequisites)
  * [Configuration](#configuration)
  * [Integrating with OpenAPI Generator](#integrating-with-openapi-generator)
  * [OpenAPI Generator Plugin Configuration](#openapi-generator-plugin-configuration)
  * [Build the Project](#build-project)
  * [Generating The API Client Sources](#generating-the-api-client-sources)
  * [Test Case Execution](#test-case-execute)
  * [Use Cases](#use-cases)
  * [Execute the Use-Cases](#execute-use-cases)
- [API Reference](#api-reference)
  * [Authorization](#authorization)
  * [Request Examples](#request-examples)
  * [Client Encryption](#client-encryption)
  * [Recommendation](#recommendation)
- [Support](#support)
- [License](#license)

## Overview <a name="overview"></a>
ID is a digital identity service from Mastercard that helps you apply for, enroll in, log in to, and access services more simply, securely and privately. Rather than manually providing your information when you are trying to complete tasks online or in apps, ID enables you to share your verified information automatically, more securely, and with your consent and control. ID also enables you to do away with passwords and protects your personal information. Please see here for more details on the API: [Mastercard Developers](https://developer.mastercard.com/mastercard-id-for-idp/documentation/).

For more information regarding the program, refer to [ID Service](https://idservice.com/)

### References <a name="references"></a>
* [Mastercard's OAuth Signer library](https://github.com/Mastercard/oauth1-signer-java)
* [Using OAuth 1.0a to Access Mastercard APIs](https://developer.mastercard.com/platform/documentation/using-oauth-1a-to-access-mastercard-apis/)
* [Mastercard's Payload Encryption/Decryption library](https://github.com/Mastercard/client-encryption-java)
* [Using Payload Encryption](https://developer.mastercard.com/platform/documentation/security-and-authentication/securing-sensitive-data-using-payload-encryption/)

## Usage <a name="usage"></a>
### Prerequisites <a name="prerequisites"></a>
* [Mastercard Developers Account](https://developer.mastercard.com/dashboard) with access to ID for Identity Providers API
* IntelliJ IDEA (or any other IDE)
* [Spring Boot 2.2+ up to 2.7.x](https://spring.io/projects/spring-boot)
* [Apache Maven 3.3+](https://maven.apache.org/download.cgi)
* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Set up the `JAVA_HOME` environment variable to match the location of your Java installation

### Configuration <a name="configuration"></a>
* Create an account at [Mastercard Developers](https://developer.mastercard.com/account/sign-up).
* Create a new project and add `ID for Identity Providers` API to your project.
* Configure project and download all the keys. It will download multiple files.
* Select all `.p12` files, `.pem` file and copy it to `src/main/resources` in the project folder.
* Open `${project.basedir}/src/main/resources/application.properties` and configure below parameters.

  >**mastercard.api.base.path=corresponding MC ID Service Url, example : https://developer.mastercard.com/mastercard-id-for-idp/documentation/**, it is a static field, will be used as a host to make API calls.

  **The properties below will be required for authentication of API calls.**

  >**mastercard.api.key.file=** this refers to .p12 file found in the signing key. Please place .p12 file at src\main\resources in the project folder and add classpath for .p12 file.

  >**mastercard.api.consumer.key=** this refers to your consumer key. Copy it from "Keys" section on your project page in [Mastercard Developers](https://developer.mastercard.com/dashboard)

  >**mastercard.api.keystore.alias=keyalias**, this is the default value of key alias. If it is modified, use the updated one from keys section in [Mastercard Developers](https://developer.mastercard.com/dashboard).

  >**mastercard.api.keystore.password=keystorepassword**, this is the default value of key alias. If it is modified, use the updated one from keys section in [Mastercard Developers](https://developer.mastercard.com/dashboard).

  >**arid=** this is the request id generated in the original relying party request. For more information about its usage you can find in the API reference section in [Mastercard IDP API Reference](https://developer.mastercard.com/mastercard-id-for-idp/documentation/api-reference/).

  >**idp.userIdentifier=** this is the unique identifier which IDP provides upon authenticating an end-user. This value forms 'alias' claim in the JWT token to be generated by IDP while invoking Mastercard's
  IDP API endpoints. For more information on this unique identifier please refer at [Mastercard IDP API Reference app usage](https://developer.mastercard.com/mastercard-id-for-idp/documentation/reference-app/).

  >**mastercard.client.decryption.enable=** this parameter allows to decrypt the payload from the server in case it's true.

  >**mastercard.client.encryption.enable=** this parameter allows to encrypt the payload before to send to the server in case it's true.

  >**mastercard.api.encryption.certificateFile=** contains the reference for the pem file certificate used by the client to verify and decrypt data sent, this file should be located at src/main/resources.

  >**mastercard.api.encryption.fingerPrint=** The fingerprint of the Mastercard public key used to encrypt the ephemeral AES key.
  more information please refer at [Issuer-Initiated Tokenization](https://developer.mastercard.com/mdes-pre-digitization/documentation/use_case/issuer-tokenization/).

  >**mastercard.api.decryption.keystore=** this is the password provided while creating the API project in [Mastercard Developers](https://developer.mastercard.com).

  >**mastercard.api.decryption.alias=** this is the user provide keyalias that is used while creating the API project in [Mastercard Developers](https://developer.mastercard.com).

  >**server.port=** Application Port.

### Integrating with OpenAPI Generator <a name="integrating-with-openapi-generator"></a>
[OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) generates API client libraries from [OpenAPI Specs](https://github.com/OAI/OpenAPI-Specification).
It provides generators and library templates for supporting multiple languages and frameworks.

See also:
* [OpenAPI Generator (maven Plugin)](https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin)
* [OpenAPI Generator (executable)](https://mvnrepository.com/artifact/org.openapitools/openapi-generator-cli)
* [CONFIG OPTIONS for java](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/generators/java.md)

#### OpenAPI Generator Plugin Configuration <a name="openapi-generator-plugin-configuration"></a>
```xml
<!-- https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin -->
<plugin>
    <groupId>org.openapitools</groupId>
    <artifactId>openapi-generator-maven-plugin</artifactId>
    <version>${openapi-generator.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/mastercard-idservice-reference_api_spec.yaml</inputSpec>
                <generatorName>java</generatorName>
                <library>okhttp-gson</library>
                <generateApiTests>false</generateApiTests>
                <generateModelTests>false</generateModelTests>
                <configOptions>
                    <sourceFolder>src/gen/main/java</sourceFolder>
                    <dateLibrary>java8</dateLibrary>
                </configOptions>
            </configuration>
        </execution>
    </executions>
</plugin>
```
#### Build the Project <a name="build-project"></a>
Once you clone the project you have to make sure that IntelliJ IDEA recognise the folders. got to
**(file > project structure > modules)** and select the folder `src/main/java` as a source and `src/test/java` as test folder,
also check the language level at this configuration options and see if its select (8 - lambda type annotation etc.) following your java version
add also the Maven support In the Project tool window, right-click your project and select Add Framework Support.

#### Generating The API Client Sources <a name="generating-the-api-client-sources"></a>
Now that you have all the required dependencies, you can generate the sources. To do this, use one of the following two methods:

`Using IDE`
* **Method 1**<br/>
  In IntelliJ IDEA, open the Maven window **(View > Tool Windows > Maven)**. Click the icons `Reimport All Maven Projects` and `Generate Sources and Update Folders for All Projects`

* **Method 2**<br/>

  In the same menu, navigate to the commands **({Project name} > Lifecycle)**, select `clean` and `compile`, then click the icon `Run Maven Build`.

`Using Terminal`
* Navigate to the project's root directory within a terminal window and execute `mvn clean compile` command.

### Test Case Execution <a name="test-case-execute"></a>
Navigate to the test package and right click to  `Run All Tests`

### Use cases <a name="use-cases"></a>
1. **RP Scopes**
   - Retrieve the scopes and RP details associated with the arid. The ARID must be in PENDING status.
2. **Scope fulfillments**
   - Process the IDP claims and update the authentication request with the claims data.

### Execute the Use-Cases <a name="execute-use-cases"></a>
1. Run mvn clean install from the root of the project directory.
2. There are two ways to execute the user cases :
    1. Execute the test cases
      - At the `src/test/java` which is the main root folder for all Junit tests of the application.
      - Run the tests.
    2. Select the menu options provided by the application
      - At the [ApiClientConfiguration.java](./src/main/java/com/mastercard/dis/mids/reference/config/ApiClientConfiguration.java) file add the header with the key downstream-route and the value the environment available allowed inside the method apiClient().
      - Run ```mvn spring-boot:run``` command to run the application.
      - Once the application is running, you should be able to see and chose the follow three options :
        - 0 Exit
        - 1 RP Scopes
        - 2 Scope-fulfillments
      - The option 1 and 2 are going to ask for the arid value in case you press enter the arid value used is going to be the one found at the `application.properties` file. 

## API Reference <a name="api-reference"></a>

### Authorization <a name="authorization"></a>
The `com.mastercard.dis.mids.reference.config` package will provide you API client. This class will take care of adding the correct `Authorization` header before sending the request.

### Request Examples <a name="request-examples"></a>
You can change the default input passed to APIs, modify values in following file:
* `com.mastercard.dis.mids.reference.constants.Constants`
* The UUID_REGEX variable is responsible to validate the arid values typed by the user, example : f300565f-6b94-48bf-8242-0b854aefd9fa

### Client Encryption <a name="client-encryption"></a>
The public key your application uses to encrypt requests is listed under “Client Encryption Keys” on the Developer Dashboard.
this key file is linked at the [application.properties](./src/main/resources/application.properties).
for more information please go to [Payload Encryption](https://developer.mastercard.com/cross-border-services/documentation/api-ref/encryption/).
We also recommend reading Using Payload Encryption link at the References section at this document.

### Recommendation <a name="recommendation"></a>
It is recommended to create an instance of `ApiClient` per thread in a multithreaded environment to avoid any potential issues.

## Support <a name="support"></a>
If you would like further information, please send an email to `apisupport@mastercard.com`
- For information regarding licensing, refer to the [LICENSE](LICENSE.md).
- For copyright information, refer to the [COPYRIGHT](COPYRIGHT.md).
- For instructions on how to contribute to this project, refer to the [CONTRIBUTING](CONTRIBUTING.md).
- For changelog information, refer to the [CHANGELOG](CHANGELOG.md).

## License <a name="license"></a>
Copyright 2023 Mastercard

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.