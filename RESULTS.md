# API lab results

## Part 0
- Step 0.1, the number of pizza names: 18
- Step 0.2, status line `HTTP/2 200`, `content-type: application/json`, and a `set-cookie: AWSALB=...` header
- Step 0.3, status code of the request without a token: 401 (`{"error":"authentication failed"}`)
- Step 0.4, the name of your pizza: The Solid Siciliana
- Step 0.12, results of the three runs without a session (optional): run 1 passed, run 2 failed (login 401 instead of 200), run 3 failed (login 401). The test is flaky, so it was deleted.
- Step 0.16, blocks of "how many of the three tools are excluded": 0 tools (`[]`), 1 tool (`["Knife"]`, `["Scissors"]`), 2 tools (`["Knife", "Scissors"]`), 3 tools (`["Knife", "Pizza cutter", "Scissors"]`). The original test covered only the block "1 tool". Blocks 0 and 2 answer 200; block 3 answers 500 with an empty body.

## Defect report
- Title: Defect 1, excluding all three tools makes `POST /api/pizza` crash with 500 and an empty body
- Request: `POST https://quickpizza.grafana.com/api/pizza` with header `Authorization: token <token of a logged-in user>` and body `{"excludedTools": ["Knife", "Pizza cutter", "Scissors"]}` (`curl -i -H "@auth.txt" -d "@all_tools.json" https://quickpizza.grafana.com/api/pizza`)
- Expected result: No pizza can be cut without a tool, so the API refuses the request with status 400 and a JSON error message, for example `{"error": "no pizza matches the restrictions"}`.
- Actual result: Status `HTTP/2 500`, `content-length: 0`, no body at all. Covered by `test_excluded_tools.py::test_all_tools_excluded_is_refused` (xfail, strict).
- Severity: Medium. Only an unusual input triggers it and no data is lost, but the server fails instead of validating its input, and an app that calls the API gets no message that it can show to the customer.
