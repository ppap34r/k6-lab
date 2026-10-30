# K6 lab results

## Part 0
### Step 0.1
- Step 0.1, http_req_duration: 278.42 ms
    HTTP
    http_req_duration..............: avg=278.42ms min=278.42ms med=278.42ms max=278.42ms p(90)=278.42msp(95)=278.42ms
      { expected_response:true }...: avg=278.42ms min=278.42ms med=278.42ms max=278.42ms p(90)=278.42msp(95)=278.42ms
    http_req_failed................: 0.00%  0 out of 1
    http_reqs......................: 1      0.519783/s

### Step 0.2
- http_reqs: 75
- Request rate: 3.595 req/s
- http_req_duration p(95): 348.4 ms
- Reason: The request rate is below 5 requests per second because each VU waits for 1 second after each iteration using sleep(1).
    HTTP
    http_req_duration..............: avg=302.42ms min=262.58ms med=286.83ms max=614.92ms p(90)=331.2ms p(95)=348.4ms
      { expected_response:true }...: avg=302.42ms min=262.58ms med=286.83ms max=614.92ms p(90)=331.2ms p(95)=348.4ms
    http_req_failed................: 0.00%  0 out of 75
    http_reqs......................: 75     3.595033/s


### Step 0.3
- VUs: 5
- Iterations: 20
- http_reqs: 20
- Request rate: 3.397 req/s
- http_req_duration avg: 306.95 ms
- http_req_duration p(95): 354.17 ms
- http_req_failed: 0%
- Checks succeeded: 100% (20/20)
- Total duration: 5.9 seconds

### Step 0.4
- Script: 04-pizza-api.js
- Checks succeeded: 100% (3/3)
- http_reqs: 1
- http_req_duration: 406.2 ms
- http_req_failed: 0%
- iteration_duration: 2.24 s
- Result: All checks passed. The API returned HTTP 200 and a vegetarian pizza with a name.

### Step 0.5
- Script: 05-no-token.js
- Checks succeeded: 100% (1/1)
- http_reqs: 1
- http_req_duration: 277.52 ms
- http_req_failed: 0%
- Result: The API successfully refused a request without an authorization token.

### Step 0.6
- Step 0.6, exit codes:
  - Normal run: 0 (all thresholds passed) = Threshold p(95): 616.09 ms	
  - Strict run (P95=300): 99 (http_req_duration threshold failed) = Threshold p(95): 668.52 ms

### Step 0.7
#### First Run
- p(95): 482.33 ms (PASS)
- p(99): 762.25 ms (PASS)
- http_req_failed: 0.00% (PASS)
- Total requests: 64
- Checks: 128 succeeded, 0 failed
- Overall: All thresholds passed.

#### Second Run p(95)<300: Strict Run
- Modified threshold: p(95)<300 ms
- Measured p(95): 461.38 ms (FAIL)
- Measured p(99): 482.84 ms (PASS)
- http_req_failed: 0.00% (PASS)
- Total requests: 65
- Checks: 130 succeeded, 0 failed
- Exit code: 99
- Overall: Failed because p(95) exceeded the strict threshold.

#### Conclusion
The API successfully handled all requests, but it did not meet the strict response time requirement of p(95) at 300 ms . This demonstrates that K6 thresholds can fail even when HTTP checks succeed.

### Step 0.8 – Bug Hunt
- Test file: `08-bug-hunt.js`
- Test command: `k6 run --vus 5 --duration 20s 08-bug-hunt.js`
- Virtual users: 5
- Duration: 20 seconds
- Total checks: 201
- Checks succeeded: 100% (201/201)
- Checks failed: 0% (0/201)
- HTTP requests: 67
- Request rate: 3.14 req/s
- Average response time: 466.31 ms
- p(95) response time: 1.09 s
- HTTP request failed: 0%

#### Bug found and fixed:
- The topping name needed to be `Pepperoni` instead of `pepperoni`.
- The tool name needed to be `Knife` instead of `knife`.
- Explored the API to identify the correct names and fixed the test data.

#### Result: 
- All three checks passed successfully under 5 virtual users for 20 seconds.