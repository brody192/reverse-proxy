# [Caddy](https://caddyserver.com/) Frontend & Backend Reverse Proxy

**Combine your **separate** frontend and backend services into one domain!**

### [View the example public project here](https://railway.app/project/35d8d571-4313-4049-9699-4e7db7f02a2f)

Access the frontend from `/*` and access the backend from `/api/*` on the same domain

**Frontend - React:** https://mysite.up.railway.app/

**Backend - FastAPI:** https://mysite.up.railway.app/api/

The proxy configurations are done in the [`Caddyfile`](https://github.com/brody192/reverse-proxy/blob/main/Caddyfile) everything is commented for your ease of use!

**Relevant Caddy documentation:**

- [The Caddyfile](https://caddyserver.com/docs/caddyfile)
- [Caddyfile Directives](https://caddyserver.com/docs/caddyfile/directives)
- [reverse_proxy](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy)

**Some prerequisites to help with common issues that could arise:**

- Both the frontend and backend need to listen on fixed ports, in my Caddyfile I have used port `3000` in the proxy address, and configured my frontend and backend to both listen on port `3000`
    - This can be done by [configuring your frontend and backend apps to listen on the `$PORT`](https://docs.railway.app/troubleshoot/fixing-common-errors) environment variable, then setting a `PORT` service variable to `3000`

- Since Railway's internal network is IPv6 only the frontend and backend apps will need to listen on `::` (all interfaces - both IPv4 and IPv6)

    **Start commands for some popular frameworks:**

    - **Gunicorn:** `gunicorn main:app -b [::]:$PORT`

    - **Uvicorn:** `uvicorn main:app --host :: --port $PORT`

        - Uvicorn does not support dual stack binding (IPv6 and IPv4) from the CLI, so while that start command will work to enable access from within the private network, this prevents you from accessing the app from the public domain if needed, I recommend using [Hypercorn](https://pgjones.gitlab.io/hypercorn/) instead

    - **Hypercorn:** `hypercorn main:app --bind [::]:$PORT`

    - **Next:** `next start -H :: --port $PORT`

    - **Express/Nest:** `app.listen(process.env.PORT, "::");`
