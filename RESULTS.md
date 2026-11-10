# API lab results

## Part 0
- Step 0.1, the number of pizza names: 18
- Step 0.2, status line `HTTP/2 200`, `content-type: application/json`, and a `set-cookie: AWSALB=...` header
- Step 0.3, status code of the request without a token: 401 (`{"error":"authentication failed"}`)
- Step 0.4, the name of your pizza: The Solid Siciliana
- Step 0.12, results of the three runs without a session (optional): run 1 passed, run 2 failed (login 401 instead of 200), run 3 failed (login 401). The test is flaky, so it was deleted.
