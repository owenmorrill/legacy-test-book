# Configure integration for GCR

Configure integration from Snyk with your Google container registries to scan for vulnerabilities and fix security and license issues accordingly.

**Prerequisites:**

[Set up a new service account key](https://support.snyk.io/hc/articles/360004191777#UUID-53c3d159-a436-9605-ec76-6bdc016fd824) from your GCR account.

**Steps:**

1. Navigate to your Snyk organization, and go to **Integrations=&gt;GCR**.
2. From the GCR hostname, enter the [registry storage region](https://cloud.google.com/container-registry/docs/pushing-and-pulling) for the images you want to scan in the format region.gcr.io. For example: gcr.io or asia.gcr.io.
3. Paste the entire contents of the JSON key file you created from Google into the JSON key file field in your Snyk account, like the screenshot below. 
4. Click **Save**.

   Snyk checks the credentials and when successful, the page reloads with a notification that the connection succeeded.

![GCR\_configur.png](../../../.gitbook/assets/uuid-47cf04cb-248e-5d0f-d35a-f36fbb624614-en.png)

