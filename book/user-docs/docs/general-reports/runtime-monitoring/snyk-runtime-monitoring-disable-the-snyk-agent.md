# Snyk runtime monitoring: disable the Snyk agent

{% hint style="info" %}
This feature is deprecated
{% endhint %}

Leave the agent installed for future use, while still disabling it for now:

1. Set the enable parameter to **false**.
2. Ensure this Require statement for the agent is entered prior to all other require statements that you may add to the code.
3. Commit and push the changes to your manifest file \(for example package.json\).

