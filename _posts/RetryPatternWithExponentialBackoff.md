## Handling limiting gracefully

In one of the projects we were making a lot of API requests to third party provider(for example: Google APIs). These APIs had certain rate limits implemented. We were being throttled, most of our API calls failed with 429 requests

You can read more about different rate limiting techniques applied [here](3) but in this blog post I want to discuss about one of the approaches that we used to handle them

## Part 1: Retry Mechanism with Exponential Backoff

Unlike other HTTP code(like 401, 500), the 429 is not because of an issue in calling the API or from the server side. 429 occurs because there are certain limits that are applied to the resource to prevent it from being overused due to sudden attack. Think of it as max load limit kept on a particular API resource. But what should the application that is consuming these APIs do to overcome it? Letting it simply fail would not be the right approach because if you call the same API after cool off period it will work. That shows the application is showing inconsistent behvaiour. In order to avoid this, we implemented a axios interceptor to retry the failed request. You can either use some of the open source npm packages but we decided to write our own version. Some factors that need to be pre-defined are MAX_RETRIES - number of time you would like to retry the request before considering it failed.

[Exponential Backoff](https://en.wikipedia.org/wiki/Exponential_backoff) is a common error handling strategy that retries the request after an exponential time period for failed API call.

 
<div class="code_container">
<iframe src="https://stackblitz.com/edit/retry-api-call-axios-interceptor?embed=1&file=index.ts&hideNavigation=1&view=editor"></iframe>
</div>

## Part 2: Batch Processing

<div class="code_container">
<iframe src="https://stackblitz.com/edit/batch-processing?embed=1&file=index.ts&hideNavigation=1&view=editor"></iframe>
</div>
