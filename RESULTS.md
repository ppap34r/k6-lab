# K6 lab results

## Part 0

- Step 0.1, http_req_duration: 278.42 ms

- Step 0.2, requests, rate and p(95), and why the rate stays below 5:
  - http_reqs: 75
  - Request rate: 3.595 req/s
  - p(95): 348.4 ms
  - The request rate is below 5 requests per second because each VU waits for 1 second after each iteration using `sleep(1)`.

- Step 0.3:
  - VUs: 5
  - Iterations: 20
  - http_reqs: 20
  - Request rate: 3.397 req/s
  - http_req_duration avg: 306.95 ms
  - http_req_duration p(95): 354.17 ms
  - http_req_failed: 0%
  - Checks succeeded: 100% (20/20)
  - Total duration: 5.9 seconds

- Step 0.4:
  - Script: `04-pizza-api.js`
  - Checks succeeded: 100% (3/3)
  - http_reqs: 1
  - http_req_duration: 406.2 ms
  - http_req_failed: 0%
  - iteration_duration: 2.24 s
  - Result: All checks passed. The API returned HTTP 200 and a vegetarian pizza with a name.

- Step 0.5:
  - Script: `05-no-token.js`
  - Checks succeeded: 100% (1/1)
  - http_reqs: 1
  - http_req_duration: 277.52 ms
  - http_req_failed: 0%
  - Result: The API successfully refused a request without an authorization token.

- Step 0.6, exit code of the normal run and of the run with P95=300:
  - Normal run: 0 (p(95) = 616.09 ms)
  - Strict run (P95=300): 99 (p(95) = 668.52 ms)

- Step 0.7, rules that passed, p(95), p(99), exit code after your change:
  - First run:
    - p(95): 482.33 ms (PASS)
    - p(99): 762.25 ms (PASS)
    - http_req_failed: 0.00% (PASS)
    - Exit code: 0
  - Second run (p(95)<300):
    - p(95): 461.38 ms (FAIL)
    - p(99): 482.84 ms (PASS)
    - http_req_failed: 0.00% (PASS)
    - Exit code: 99

- Step 0.8, what was wrong, and how the checks showed it:
  - The topping name needed to be `Pepperoni` instead of `pepperoni`.
  - The tool name needed to be `Knife` instead of `knife`.
  - The first run showed failed checks for these items.
  - After correcting the names, all checks passed (201/201).

## Part A results

| Step | Test type | Users | Req/s | p(95) | Max | Failed | Checks |
|---|---|---:|---:|---:|---:|---:|---:|
| A1 | Smoke | 1 | 0.49 | 502.07 ms | Not recorded | 0.00% | 20/20 |
| A2 | Average load | 3 | 1.28 | 554.51 ms | Not recorded | 0.00% | 312/312 |
| A3 | Stress | 5 | 2.04 | 699.06 ms | Not recorded | 0.00% | 496/496 |
| A4 | Spike | 5 | 0.97 | 654.88 ms | 1.63 s | 0.00% | 184/184 |
| A5 | Soak | 2 | 0.97 | 458.12 ms | 2.00 s | 0.00% | 856/856 |

## Part B

1. **Fill in the results table for Steps A1 to A5.**

| Step | Test | VUs | Duration | Requests | Request Rate | Avg Response Time | p(95) Response Time | Failed Requests | Checks |
|---|---|---:|---|---:|---:|---:|---:|---:|---|
| A1 | Smoke Test | 1 | 30 sec | 15 | 0.49 req/s | 364.92 ms | 502.07 ms | 0.00% | 20/20 PASS |
| A2 | Average Load Test | 3 | 3 min | 234 | 1.28 req/s | 362.44 ms | 554.51 ms | 0.00% | 312/312 PASS |
| A3 | Stress Test | 5 | 3 min | 372 | 2.04 req/s | 385.08 ms | 699.06 ms | 0.00% | 496/496 PASS |
| A4 | Spike Test | 1 → 5 → 1 | 2 min 20 sec | 138 | 0.97 req/s | 364.68 ms | 654.88 ms | 0.00% | 184/184 PASS |
| A5 | Soak Test | 2 | 11 min | 642 | 0.97 req/s | 349.33 ms | 458.12 ms | 0.00% | 856/856 PASS |

2. **Why was the request rate below 5 requests per second with 5 virtual users in Step 0.2?**

   The request rate is below 5 requests per second because each VU waits for 1 second after each iteration using `sleep(1)`.

3. **Did the response time grow from the average-load test to the stress test? Explain with the operational envelope.**

   Yes. The p(95) response time increased from 554.51 ms in A2 to 699.06 ms in A3. This means the system became slower with more users. However, it stayed within the tested performance limits, so it was still inside the operational envelope.

4. **In the spike test, how long did the response time take to return to normal after the spike?**

   The response time returned close to normal shortly after the spike. It did not stay high for a long time.

5. **In the soak test, did the response time or the error rate get worse over time? Name one kind of defect that a soak test of several hours could reveal.**

   No. The response time stayed mostly stable, and the error rate remained at 0%. A longer soak test could reveal a memory leak, which slowly uses up the system's memory over time.

6. **Why do the thresholds in this lab use p(95) rather than the average?**

   The average can hide slow requests. p(95) shows the response time that 95% of requests finish within, so it helps us understand the experience of most users.

7. **Give two reasons why the same tests against a system in a lab could give different numbers.**

   1. A lab usually has a more stable network, while internet speed and traffic can change.
   2. A lab system may have different hardware, resources, or server settings from the public website.

8. **Why must you never run a stress, spike or breakpoint test with hundreds of users against a site that you do not own?**

   It could overload the website, slow it down, or make it unavailable to other users. This could be treated as an attack if we do not have permission.

9. **Write one vague performance requirement for an app you use every day. Rewrite it as a testable requirement, and show the k6 thresholds that check it.**

   **Vague requirement:**

   The messaging app should be fast.

   **Testable requirement:**

   95% of message-sending requests should finish within 1 second when 100 users use the app at the same time. The request failure rate should be below 1%.

   **k6 thresholds:**

   ```javascript
   thresholds: {
     http_req_duration: ['p(95)<1000'],
     http_req_failed: ['rate<0.01'],
   }
   ```
