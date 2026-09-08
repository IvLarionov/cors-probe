# CORS Probe

Minimal web application used for browser-level CORS tests.

The application serves `cors-probe.html`, which performs a cross-origin
GET request to a target URL supplied through the `target` query parameter.

## Usage

Open:

    /cors-probe.html?target=<API_URL>

For example:

    http://cors-test-allowed.example/cors-probe.html?target=http://dev2.vqs.net:8080/api/v3/server_info

The page displays:

- `CORS_ALLOWED` when the browser can read the response.
- `CORS_BLOCKED` when the browser blocks the request because of CORS
  or the request otherwise fails.
- `ERROR` when the `target` parameter is missing.

## Docker

Build:

    docker build -t cors-probe .

Run:

    docker run --rm -p 8080:80 cors-probe

The probe is then available at:

    http://localhost:8080/cors-probe.html

## CORS browser tests

The browser determines the `Origin` from the URL of the probe page.

Therefore, the allowed and disallowed scenarios must use different origins.

For example:

    CORS_PROBE_ALLOWED_ORIGIN=http://cors-test-allowed.example
    CORS_PROBE_DISALLOWED_ORIGIN=http://cors-test-disallowed.example

The API `AllowedOrigins` configuration should contain only:

    http://cors-test-allowed.example

and must not contain:

    http://cors-test-disallowed.example

The same application can be deployed behind two different hostnames.
No application-level configuration is required for the two origins.

## Expected behavior

### Allowed origin

Open:

    http://cors-test-allowed.example/cors-probe.html?target=http://dev2.vqs.net:8080/api/v3/server_info

Expected result:

    CORS_ALLOWED

### Disallowed origin

Open:

    http://cors-test-disallowed.example/cors-probe.html?target=http://dev2.vqs.net:8080/api/v3/server_info

Expected result:

    CORS_BLOCKED
