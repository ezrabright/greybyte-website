# GreyByte live vacancies — deployment checklist

Status: PREPARATION ONLY. Do not merge until the API is deployed and tested.

## Architecture
- Website: https://www.greybyte.co.za (existing static hosting)
- API: https://api.greybyte.co.za (proposed ASP.NET Core .NET 10 deployment)
- API database: separate private GreyByte volume/database; do not copy development candidate records to production
- Reverse proxy: Caddy or Nginx, HTTPS and automatic certificate renewal
- DNS: add api.greybyte.co.za A/AAAA record only after server approval/provisioning

## API readiness
1. Host the ASP.NET Core API behind HTTPS and configure persistent ASP.NET Core Data Protection keys (required for Identity cookies across restarts).
2. Use a separate production connection string, persistent database storage and tested backups. Apply EF Core migrations explicitly during a controlled deployment.
3. Configure CORS for the exact website origins (https://greybyte.co.za and https://www.greybyte.co.za) if the API is called cross-origin. No wildcard origin with credentials.
4. Keep internal authentication and administrative endpoints protected; do not expose development seeding credentials or Swagger/OpenAPI in production without access controls.
5. Check cookie SameSite/Secure policy and CSRF defenses before exposing authenticated browser actions.
6. Rate-limit and validate public endpoints; never expose applicant PII in the public vacancies endpoint.
7. Verify GET https://api.greybyte.co.za/api/vacancies/public returns only published, non-expired jobs, with a valid certificate.

## Website readiness
1. Edit vacancies.html meta tag: <meta name="greybyte-api-base" content="https://api.greybyte.co.za">
2. Preview the branch and test loading, zero results, API errors, View Job modal, mobile responsiveness and accessibility.
3. Confirm the browser can fetch the API without CORS errors.
4. Review the PR and merge only after verification.

## Application submission
The website currently displays View Job and Candidate Enquiries only. Do not add a live Apply Now form until candidate identity, ownership validation, anti-spam/rate limits, data privacy and application permissions have been tested.

## Rollback
Revert the website commit if listing breaks. Maintain a backed-up API database and previous deploy image for rollback.
