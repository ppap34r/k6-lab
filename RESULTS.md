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