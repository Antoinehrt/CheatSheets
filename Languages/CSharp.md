Below is an English table of contents for an ASP.NET cheat sheet covering what you've learned/done in this project so far.
1. Project overview 1. Purpose & scope 2. Solution layout and projects
2. Folder & file structure 1. Common folders: Controllers, Application, Domain, Infrastructure, Tests 2. Key test files: tests/Users/CreateUserTests.cs, BaseFunctionalTest, FunctionalTestWebAppFactory
3. DTOs & request models 1. InputCreateUserDto fields and usage 2. Mapping considerations (manual vs AutoMapper)
4. Controllers & endpoints 1. POST /user semantics and typical responses 2. Routing and attribute usage
5. Validation & model binding 1. Data annotations vs FluentValidation 2. Returning BadRequest and ProblemDetails for invalid input
6. Authentication & authorization (project-specific notes) 1. Registration flow and password confirmation 2. Token generation (if implemented) and protecting endpoints
7. Persistence & user creation 1. User entity basics and password hashing 2. EF Core setup, migrations, in-memory DB for tests (if used)
8. Dependency injection & service registration 1. Registering application services and repositories 2. Scoped/transient/singleton lifetimes
9. Error handling & middleware 1. Global exception handling middleware 2. Validation error formatting
10. Testing strategy 1. Unit tests vs integration/functional tests 2. Test naming conventions (example: Should_ReturnBadRequest_WhenEmailIsMissing)
11. Functional / integration testing details 1. FunctionalTestWebAppFactory usage and configuration 2. BaseFunctionalTest providing HttpClient 3. Using HttpClient.PostAsJsonAsync and asserting HttpStatusCode 4. Test isolation and database seeding/cleanup
12. Test tooling & libraries 1. xUnit test framework 2. FluentAssertions for expressive assertions
13. HTTP & API concerns 1. Status codes to return per scenario (200, 201, 400, 401, 404, 500) 2. JSON payload conventions and content negotiation
14. Local development & run commands 1. dotnet run, dotnet test, launch profiles in JetBrains Rider 2. Environment variables and appsettings per environment
15. CI/CD & deployment notes 1. Typical pipeline steps: restore, build, test, publish 2. Docker considerations (image, multi-stage build)
16. Useful snippets & quick references 1. Example HttpClient test call pattern 2. Common validation attribute quick list
17. Troubleshooting & gotchas 1. Common causes of BadRequest in tests 2. Fixes for flaky integration tests (DB isolation, async timing)
Use this TOC to scaffold a concise cheat sheet; expand each item into short actionable notes and minimal examples for quick lookup.