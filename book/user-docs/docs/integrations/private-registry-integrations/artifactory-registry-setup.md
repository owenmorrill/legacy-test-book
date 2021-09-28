# Artifactory Registry setup

## **Overview**

{% hint style="info" %}
**Feature availability**  
This feature is available with Enterprise plans. See [pricing plans](https://snyk.io/plans/) for more details.
{% endhint %}

{% hint style="success" %}
The Artifactory Package Repository integration currently supports [Maven](../../snyk-open-source/language-and-package-manager-support/snyk-for-java-gradle-maven.md) and [Node.js](../../snyk-open-source/language-and-package-manager-support/snyk-for-javascript.md) \(npm and Yarn\) projects.
{% endhint %}

Connecting a custom Artifactory Package Repository enables Snyk to resolve all direct and transitive dependencies of packages hosted on the custom registry and calculate a more complete, accurate dependency graph and related vulnerabilities.

You can configure two types of Artifactory Package Repository:

1. Publicly accessible instances protected by basic authentication
2. Instances on a private network accessed via a [Snyk Broker](../snyk-broker/broker-introduction.md) \(with or without basic authentication\)

## Getting started

1. Go to settings ![](../../.gitbook/assets/cog_icon.png) &gt; **Integrations &gt; Package Repositories &gt; Artifactory** 
2. You should see this screen at the beginning

![](../../.gitbook/assets/screenshot_2020-04-17_at_14.38.12.png)

{% hint style="info" %}
If you do not see the “Snyk Broker” switch you do not have the necessary permissions and can only add a publicly accessible instance.  
Contact [support@snyk.io](mailto:support@snyk.io) if you want to add a private registry
{% endhint %}

## Set up publicly accessible instances

1. Enter URL of your Artifactory instance, this **must** end with **/artifactory**
2. Enter Username
3. Enter Password
4. Hit **Save**

## Set up brokered instances

1. Toggle **Artifactory \(publicly accessible\)** switch, you should now see a form for generating an Artifactory Broker token
2. Click on **Generate and Save** button 
3. Copy the token that was generated for you, it will be needed to set up a new Broker Client 
4. Pull Broker Artifactory image from Dockerhub:

   ```text
   docker pull snyk/broker:artifactory
   ```

5. Run docker image and provide [broker variables](artifactory-registry-setup.md#broker-variables)

   ```text
   docker run --restart=always \
     -p 8000:8000 \
     -e BROKER_TOKEN=secret-broker-token \
     -e ARTIFACTORY_URL=acme.com/artifactory \
     -e RES_BODY_URL_SUB=http://acme.com/artifactory \ 
     snyk/broker:artifactory
   ```

6. Check connection status by refreshing Artifactory Integration Settings page, no connection error should be displayed

### Broker variables

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variable</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>BROKER_TOKEN</code>
      </td>
      <td style="text-align:left">The token generated in settings
        <img src="../../.gitbook/assets/cog_icon.png"
        alt/><b>&gt; Integrations &gt; Artifactory</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>ARTIFACTORY_URL</code>
      </td>
      <td style="text-align:left">
        <p>The URL to your Artifactory instance in the format:
          <br /><code>[http://][username:password@]hostname[:port]/artifactory</code>
        </p>
        <p><b>Optional fields</b>
        </p>
        <ol>
          <li><em>Protocol</em>: Defaults to <code>https://</code>
            <br />This should only be specified when no certificate is present and <code>http://</code> is
            required instead for your instance</li>
          <li><em>Basic auth</em>: Omit if no basic auth required.
            <br />URL encode both username and password info to avoid errors that may prevent
            authentication.</li>
          <li><em>Port</em>: Omit if no port number is needed</li>
        </ol>
        <p><b>Minimal example<br /></b><code>acme.com/artifactory</code>
        </p>
        <p><b><br />Complex example<br /></b><code>http://alice:mypassword@acme.com:8080/artifactory</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>RES_BODY_URL_SUB</code>
      </td>
      <td style="text-align:left">
        <p>The URL of the Artifactory instance, including http:// and without basic
          auth credentials.
          <br />
          <br />Required for npm/Yarn integrations only.</p>
        <p><b><br />Example</b>
          <br /><code>http://acme.com/artifactory</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

