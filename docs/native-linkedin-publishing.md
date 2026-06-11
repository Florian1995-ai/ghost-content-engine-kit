# Native LinkedIn Publishing

This service is the direct fallback when Postiz is unavailable. It uses LinkedIn's official OAuth flow and `Share on LinkedIn` API, not browser automation or cookies.

## Coolify App

- Repository: `https://github.com/Florian1995-ai/ghost-content-engine-kit`
- Branch: `main`
- Dockerfile: `deploy/linkedin-native.Dockerfile`
- Exposed port: `8080`
- Domain: `https://linkedin-api.florianrolke.com`
- Health check path: `/health`

## Required Environment Variables

Copy `deploy/linkedin-native.env.example` into the Coolify application environment.

`LINKEDIN_CLIENT_ID` and `LINKEDIN_CLIENT_SECRET` come from the LinkedIn Developer Portal app.

`LINKEDIN_REDIRECT_URI` must be added in LinkedIn Developer Portal under:

`Auth` -> `OAuth 2.0 settings` -> `Authorized redirect URLs for your app`

Use:

`https://linkedin-api.florianrolke.com/linkedin/callback`

`LINKEDIN_NATIVE_API_KEY` is not from LinkedIn. Generate a long private random value and use the same value locally when publishing through the hosted service.

## Connect LinkedIn

After deployment and DNS are live, open:

`https://linkedin-api.florianrolke.com/auth/start`

Approve the LinkedIn permissions. The callback stores the token in `/data/linkedin-native-token.json`.

## Test

Health:

```bash
curl https://linkedin-api.florianrolke.com/health
```

Local dry run through the hosted service:

```bash
python scripts/linkedin_native_client.py publish-text-remote --text "Test post from the native LinkedIn service." --dry-run --skip-image
```

Publish a draft through the hosted service:

```bash
python scripts/linkedin_native_client.py publish-draft-remote path/to/linkedin-draft.json
```
