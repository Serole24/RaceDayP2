# RaceDayP2

ASP.NET Core 8 Web API project in C# using Entity Framework Core, SQL Server LocalDB, server-side sessions, Swagger, xUnit and GitHub Actions.

If `dotnet-ef` is already installed, skip the tool-install command. If you already created a migration, do not create another `InitialCreate`; run `dotnet ef database update` instead.

## Database
The EF Core models map to the six Part 1 tables: `Users`, `UserProfiles`, `Events`, `Categories`, `Enrolments`, and `Results`. The model includes unique email, unique profile per user, unique bib number, unique user/category enrolment, one result per enrolment, foreign keys and check constraints. Do not run the Part 1 script and EF migrations against the same database without first deciding which creation path you are using. For this Part 2 project, use EF Core migrations as shown above.

## Authentication and roles
- Public registration creates **Participant** accounts and a blank `UserProfiles` record.
- Public self-registration cannot grant Organiser privileges. This prevents a user from making themselves an Organiser. To provision your first Organiser, register an account, then use an authorised database administrator to change that account's `Role` to `Organiser` in the `Users` table. Never expose this operation as a public API endpoint.
- Login stores user ID, role and email in an HTTP-only server-side session cookie. Session timeout is 60 minutes.
- Passwords are hashed with PBKDF2-HMAC-SHA256, a random 16-byte salt and 100,000 iterations; plaintext passwords are not stored.
- For production deployment, use a distributed persistent session store, HTTPS only, and configure CSRF protection for cookie-authenticated write requests.


## Tests
The xUnit test project covers registration, rejected Organiser self-registration, invalid login, unauthenticated profile access, and role rejection for event creation. 
