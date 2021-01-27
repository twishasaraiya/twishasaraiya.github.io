Rate limiting can be server side or client side or both

429 HTTP code - too many request


### Client Side Strategy
- Defer request with constant time
- Retry Mechanism
- Retry limit + exponential backoff


### Server side rate limiting

- Rate limiter can be implemented as part of the backend service or as a separate middleware

- There are multiple algorithms to limit the number of requests to our API service. 2

- Some terms and terminologies - Throttling and debouncing


#### Algorithms

1. Token Bucket

- Used by: [Amazon](), [Stripe]()
- Bucket size, refill rate(that adds new token at specific rate)

- 
2. Leaking Bucket algorithm

- Used by: [Shopify]()
- 