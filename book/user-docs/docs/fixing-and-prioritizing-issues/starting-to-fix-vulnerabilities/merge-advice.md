# Merge advice

Merge Advice is a badge we display on pull requests to indicate how confident we are that merging the pull request will not result in any breaking changes.

## How it's calculated

We determine this advice based on how well that same change has performed on other Snyk users' pull requests – did their tests on the PR pass or fail? Was the change subsequently rolled back? Did they merge successfully?

## How it's shown:

Once we've gathered enough data, we show a badge on the PR - either giving the advice "review recommended", or "high chance of success".

![](../../.gitbook/assets/merge-advice-review-recommended%20%282%29%20%282%29%20%282%29%20%282%29.png)

![](../../.gitbook/assets/advice-green%20%281%29%20%282%29%20%282%29%20%284%29%20%283%29%20%286%29%20%281%29.png)

If we haven't yet been able to collect enough data to give trustworthy advice, we show the message "not enough data". Once we've gathered enough data, we update this badge automatically with our recommendation – for that reason, a badge that was displaying "not enough data" might later show advice.

![](../../.gitbook/assets/merge-advice%20%282%29%20%282%29%20%284%29%20%282%29%20%281%29%20%2812%29.png)

## Availability:

At the moment, merge advice badges are only available for Yarn and npm, where a single package is being upgraded. More support is coming soon.

All Snyk-supported source control integrations are supported for merge advice.

