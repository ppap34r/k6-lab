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
