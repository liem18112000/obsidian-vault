---
ai_hash: 831847f5d7f11d18
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49318461454'
confluence_path: 'Overview > Manual Prompt Compression Techniques: Deep Dive'
created: 2026-04-13
entities: []
source: Confluence · ~71202087b0f7f1aaab4406a25dfa0fc075c4d4 - liem.doanvanthanh
status: reference
tags:
- confluence
- prompt-engineering
title: 'Text Compression Techniques: Examples'
type: source
updated: 2026-04-13
url: https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49318461454/Text+Compression+Techniques+Examples
---

# Text Compression Techniques: Examples

*Confluence source · Overview › Manual Prompt Compression Techniques: Deep Dive · [view original](https://axonivy.atlassian.net/wiki/spaces/~71202087b0f7f1aaab4406a25dfa0fc075c4d4/pages/49318461454/Text+Compression+Techniques+Examples) · updated 2026-04-13*

## Remove Filler Phrases

Filler phrases are words that add no semantic value. They make prompts polite but burn tokens.

#### Common Filler Phrases Reference

|                                                    |        |                |
|----------------------------------------------------|--------|----------------|
| Filler                                             | Tokens | Replace With   |
| "I would really appreciate it if you could please" | 10     | (remove)       |
| "Could you take a look at"                         | 6      | (remove)       |
| "I'd like you to"                                  | 5      | (remove)       |
| "Please make sure to"                              | 4      | (remove)       |
| "It would be great if you could"                   | 7      | (remove)       |
| "I was wondering if maybe you could"               | 8      | (remove)       |
| "If it's not too much trouble"                     | 6      | (remove)       |
| "When you get a chance"                            | 5      | (remove)       |
| "Feel free to"                                     | 3      | (remove)       |
| "Go ahead and"                                     | 3      | (remove)       |
| "In order to"                                      | 3      | "To"           |
| "Due to the fact that"                             | 5      | "Because"      |
| "At this point in time"                            | 5      | "Now"          |
| "In the event that"                                | 4      | "If"           |
| "For the purpose of"                               | 4      | "For" / "To"   |
| "In spite of the fact that"                        | 6      | "Although"     |
| "With regard to"                                   | 3      | "About" / "On" |
| "A large number of"                                | 4      | "Many"         |
| "A small number of"                                | 4      | "Few"          |
| "Take into consideration"                          | 3      | "Consider"     |

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **1.1** | "I would really appreciate it if you could please take a look at the following text and provide me with a comprehensive summary that captures all of the main points and key ideas. The summary should be concise but thorough, covering the most important aspects of the text. Here is the text I'd like you to summarize:" | 87 | "Summarize the following text, capturing all main points concisely:" | 14 | **84%** |
| **1.2** | "If you don't mind, could you possibly go through this code and let me know whether there might be any bugs or issues that I should be aware of? I'd really value your input on this." | 44 | "Find bugs in this code:" | 6 | **86%** |
| **1.3** | "When you have a moment, would it be possible for you to generate some test cases for the `calculateTotal` function? I need them for my unit test suite." | 34 | "Generate unit test cases for `calculateTotal`:" | 10 | **71%** |
| **1.4** | "I was hoping that maybe you could help me out by explaining what this regex pattern is supposed to do, because I'm having some trouble understanding it." | 34 | "Explain this regex pattern:" | 6 | **82%** |
| **1.5** | "Due to the fact that the application has been experiencing some performance issues, could you please take a look at the code and suggest improvements that could help address these problems?" | 38 | "The app has performance issues. Suggest improvements:" | 9 | **76%** |
| **1.6** | "In order to make sure that the API is functioning correctly, I would like for you to write some integration tests for all the endpoints in the user service." | 33 | "Write integration tests for all user service endpoints:" | 10 | **70%** |
| **1.7** | "I was wondering if maybe you could help me figure out why this function is returning `undefined` when it should be returning an array of user objects." | 32 | "Why does this function return `undefined` instead of `User[]`?" | 13 | **59%** |
| **1.8** | "Go ahead and take a look at the database schema and feel free to suggest any optimizations that you might think would be beneficial for our query performance." | 32 | "Suggest schema optimizations for query performance:" | 8 | **75%** |
| **1.9** | "With regard to the authentication flow, I'd really like to understand how the JWT refresh token mechanism works in our current implementation." | 28 | "Explain how JWT refresh tokens work in our auth flow:" | 13 | **54%** |
| **1.10** | "For the purpose of improving code quality, take into consideration refactoring the UserService class to follow the single responsibility principle." | 26 | "Refactor UserService to follow SRP:" | 9 | **65%** |

------------------------------------------------------------------------

### 2. Consolidate Redundant Instructions

Multiple instructions that say similar things can be merged into a single directive.

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **2.1** | "Make sure the code is well-documented. Add comments to explain complex logic. Include docstrings for all functions. Document any non-obvious behavior. Add inline comments for tricky parts." | 42 | "Add docstrings to all functions and inline comments for complex logic." | 13 | **69%** |
| **2.2** | "Write tests for this function. Make sure to test edge cases. Include tests for error conditions. Test with invalid inputs. Test with boundary values. Make sure the tests cover all the paths." | 41 | "Write tests covering edge cases, errors, invalid inputs, and all paths." | 14 | **66%** |
| **2.3** | "The code should be clean. Follow best practices. Make it readable. Use meaningful variable names. Keep functions small. Make sure it's maintainable." | 32 | "Write clean, readable code: meaningful names, small functions, maintainable." | 13 | **59%** |
| **2.4** | "Validate the user input. Check that the input is not empty. Make sure it's not null. Ensure it's the correct type. Verify it's within the acceptable range. Sanitize it to prevent injection attacks." | 43 | "Validate input: non-null, non-empty, correct type, in range, sanitized." | 15 | **65%** |
| **2.5** | "Handle the error appropriately. Log the error. Return a meaningful error response to the client. Make sure to include an error code. Include a helpful error message. Don't expose sensitive information." | 41 | "On error: log, return structured response with code + safe message." | 14 | **66%** |
| **2.6** | "Optimize the query. Add an index if needed. Use EXPLAIN to check the query plan. Avoid N+1 queries. Use joins where appropriate. Consider caching the results." | 34 | "Optimize query: add indexes, check EXPLAIN plan, fix N+1, use joins, consider caching." | 20 | **41%** |
| **2.7** | "Check for security issues. Look for SQL injection. Check for XSS. Look for CSRF vulnerabilities. Check authorization. Verify input validation. Look at authentication logic." | 35 | "Check: SQL injection, XSS, CSRF, authZ, authN, input validation." | 18 | **49%** |
| **2.8** | "Write the commit message. Use the imperative mood. Keep it under 72 characters. Be descriptive. Include the ticket number. Explain what changed and why." | 31 | "Commit message: imperative mood, \<72 chars, with ticket number, explain what+why." | 18 | **42%** |
| **2.9** | "Review the PR. Check the code quality. Look at the test coverage. Verify documentation is updated. Check for breaking changes. Make sure the CI is passing. Look at the diff size." | 38 | "Review PR: code quality, tests, docs, breaking changes, CI, diff size." | 17 | **55%** |
| **2.10** | "Deploy to production. Make sure the build passes. Run the tests. Check the staging environment first. Notify the team. Monitor the deployment. Verify the health checks pass." | 36 | "Deploy to prod: build+tests pass, staging verified, team notified, monitor health checks." | 19 | **47%** |

------------------------------------------------------------------------

### 3. Replace Prose with Constraints

Prose is human-friendly; structured constraints are token-efficient and equally clear to LLMs.

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **3.1** | "The output should be formatted as a JSON object. Each entry should have a 'name' field that is a string, an 'age' field that is a number, and an 'email' field that is a valid email address. Make sure to include all three fields for every entry." | 54 | `Output JSON: {name: string, age: number, email: string}` | 10 | **81%** |
| **3.2** | "Return the result as an array of user objects. Each user object must contain an id (integer), a username (string, 3-20 chars), an email (valid email format), and a created_at timestamp in ISO 8601 format." | 46 | `Return User[]: {id: int, username: str(3-20), email: valid-email, created_at: ISO8601}` | 26 | **43%** |
| **3.3** | "The API response should include a status field which can be either 'success' or 'error'. It should also include a data field containing the actual payload, and a message field with a human-readable description." | 42 | `Response: {status: "success"|"error", data: any, message: str}` | 16 | **62%** |
| **3.4** | "Write a function that takes two parameters: a string representing the user's name and a number representing their age. The function should return a greeting message that includes both the name and age." | 40 | `Function: greet(name: str, age: int) -> str` | 12 | **70%** |
| **3.5** | "The component should accept three props: a title which is a string and is required, a description which is an optional string, and an onClick handler which is a function that takes no arguments and returns nothing." | 43 | `Props: {title: str (required), description?: str, onClick: () => void}` | 19 | **56%** |
| **3.6** | "The database schema should have a users table with columns for id (primary key, auto-increment), email (unique, not null), password_hash (not null), created_at (timestamp, default now()), and updated_at (timestamp, nullable)." | 46 | `users table: id PK AI, email UNIQUE NN, password_hash NN, created_at TS DEFAULT now(), updated_at TS NULL` | 30 | **35%** |
| **3.7** | "The regex should match any valid email address. It should allow letters, numbers, dots, hyphens, and underscores in the local part. The domain part should contain letters, numbers, and dots. The TLD should be 2-6 characters." | 47 | `Regex: email, local=[\w.-]+, domain=[\w.]+, TLD=[a-z]{2,6}` | 22 | **53%** |
| **3.8** | "The error response should contain an HTTP status code between 400 and 599, an error code that is a string identifier in UPPER_SNAKE_CASE, a human-readable message, and an optional details object with additional context." | 43 | `Error: {status: 400-599, code: UPPER_SNAKE_CASE, message: str, details?: object}` | 25 | **42%** |
| **3.9** | "The CSV file should have a header row. The first column should be the date in YYYY-MM-DD format. The second column should be the amount as a decimal number. The third column should be the category as a string." | 45 | `CSV header: date (YYYY-MM-DD), amount (decimal), category (str)` | 17 | **62%** |
| **3.10** | "Write a function that validates a password. It must be at least 8 characters long. It must contain at least one uppercase letter. It must contain at least one lowercase letter. It must contain at least one number. It must contain at least one special character." | 53 | `validatePassword(pw): len>=8, 1+ upper, 1+ lower, 1+ digit, 1+ special` | 22 | **58%** |

------------------------------------------------------------------------

### 4. Use Abbreviations for Repeated Terms

Define abbreviations once; reference them throughout the session.

#### Abbreviation Tables

**Technical abbreviations (already single tokens in most tokenizers):**

|  |  |  |  |  |
|----|----|----|----|----|
| Full Term | Abbreviation | Tokens (full) | Tokens (abbr) | Savings Per Use |
| Application Programming Interface | API | 5 | 1 | 4 |
| Database | DB | 2 | 1 | 1 |
| Uniform Resource Locator | URL | 5 | 1 | 4 |
| Central Processing Unit | CPU | 4 | 1 | 3 |
| Random Access Memory | RAM | 4 | 1 | 3 |
| User Interface | UI | 2 | 1 | 1 |
| User Experience | UX | 2 | 1 | 1 |
| Continuous Integration | CI | 2 | 1 | 1 |
| Continuous Deployment | CD | 2 | 1 | 1 |
| JavaScript Object Notation | JSON | 5 | 1 | 4 |
| HyperText Transfer Protocol | HTTP | 4 | 1 | 3 |
| HTTP Secure | HTTPS | 2 | 1 | 1 |

**Custom session-specific abbreviations:**

|  |  |  |
|----|----|----|
| Full Term | Abbreviation | Tokens Saved Per Use |
| `UserAuthenticationService` (src/auth/service.ts) | `SVC` | ~6 |
| `PostgreSQL database` | `DB` | ~2 |
| `RabbitMQ message queue` | `Q` | ~3 |
| `React frontend application` | `FE` | ~3 |
| `Node.js backend server` | `BE` | ~3 |
| `Kubernetes cluster` | `K8s` | ~2 |
| `authentication` | `authN` | ~2 |
| `authorization` | `authZ` | ~2 |

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **4.1** | "Check if `UserAuthenticationService` correctly validates JWT tokens before querying the PostgreSQL database." | ~18 | "Check if `SVC` correctly validates JWT tokens before querying `DB`." | ~11 | **39%** |
| **4.2** | "The `UserAuthenticationService` needs to call the `PostgreSQL database` and also publish an event to the `RabbitMQ message queue` for the `React frontend application` to consume." | ~36 | "`SVC` calls `DB` and publishes to `Q` for `FE`." | ~13 | **64%** |
| **4.3** | "When the user submits a form in the React frontend application, the request goes to the Node.js backend server, which then calls the UserAuthenticationService to authenticate, then queries the PostgreSQL database for user data, and finally publishes an audit event to the RabbitMQ message queue." | ~62 | "Form submit: `FE` -\> `BE` -\> `SVC` (authN) -\> `DB` (user data) -\> `Q` (audit event)." | ~28 | **55%** |
| **4.4** | "We need to deploy the Node.js backend server to the Kubernetes cluster with the following configuration: 3 replicas, horizontal pod autoscaler enabled, and a PostgreSQL database connection pool of 20 connections." | ~42 | "Deploy `BE` to `K8s`: 3 replicas, HPA on, `DB` pool=20." | ~20 | **52%** |
| **4.5** | "Configure the HyperText Transfer Protocol Secure endpoint with rate limiting at 100 requests per minute, enable Cross-Origin Resource Sharing for the specified domains, and add authentication middleware." | ~38 | "Configure HTTPS endpoint: rate=100/min, CORS for domains, authN middleware." | ~19 | **50%** |
| **4.6** | "The application needs to support continuous integration and continuous deployment pipelines. The continuous integration should run tests and the continuous deployment should push to production after tests pass." | ~37 | "App needs CI/CD: CI runs tests, CD pushes to prod after tests pass." | ~18 | **51%** |
| **4.7** | "Update the user interface to show a loading spinner while the JavaScript Object Notation response is being parsed from the Application Programming Interface." | ~28 | "Update UI: show spinner while JSON from API is parsing." | ~12 | **57%** |
| **4.8** | "Configure the Central Processing Unit request limit and Random Access Memory limit for each pod in the Kubernetes cluster." | ~22 | "Configure CPU/RAM limits per pod in K8s." | ~11 | **50%** |
| **4.9** | "Write integration tests that verify authentication and authorization work correctly for the REST Application Programming Interface endpoints." | ~23 | "Integration tests: authN + authZ for REST API endpoints." | ~13 | **43%** |
| **4.10** | "The continuous integration pipeline should validate the uniform resource locator format, check that the HyperText Transfer Protocol Secure certificate is valid, and verify database connectivity to PostgreSQL." | ~37 | "CI pipeline: validate URL format, check HTTPS cert, verify DB connectivity." | ~17 | **54%** |

**Break-even analysis:** Defining an abbreviation costs ~10-15 tokens once. It pays for itself after 3+ uses. In a 20-turn session using 5 abbreviations 10+ times each, the net savings is ~300-500 tokens.

------------------------------------------------------------------------

### 5. Eliminate Unnecessary Context

Remove background information that doesn't affect the specific task.

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **5.1** | "I'm building a web application using React and Node.js. The application is designed for managing customer relationships. We've been working on it for about 6 months now. The team consists of 5 developers. We use Git for version control and JIRA for project management. Anyway, I need help with a specific issue in the login form component..." | ~65 | "Fix the login form validation in `src/components/Login.tsx`: \[specific issue\]" | ~20 | **69%** |
| **5.2** | "Our company is a mid-sized e-commerce business that's been growing rapidly. We switched to microservices last year and now have about 15 services. The DevOps team maintains everything on AWS. I was looking at our order processing service and noticed that one of the endpoints is slower than expected..." | ~56 | "Profile slow endpoint in order processing service:" | ~8 | **86%** |
| **5.3** | "I've been a developer for 10 years, mostly working in backend systems. Recently I've been learning more about frontend development and I'm trying to understand some React patterns. I came across this useEffect hook and I'm confused about the dependency array..." | ~50 | "Explain the useEffect dependency array:" | ~7 | **86%** |
| **5.4** | "The project was started in 2021 and has gone through several refactorings. Originally it was a monolith, then we split it into services. The authentication was initially in the main app, then moved to a separate service. Now I need to add two-factor authentication to the existing auth service..." | ~55 | "Add 2FA to the auth service:" | ~8 | **85%** |
| **5.5** | "We recently migrated from MySQL to PostgreSQL because of some performance issues we were having with large joins. The migration took about 2 months and involved updating all our ORM code. Now I'm seeing some slow queries in the analytics module and I'd like to optimize them..." | ~54 | "Optimize slow queries in analytics module (PostgreSQL):" | ~11 | **80%** |
| **5.6** | "I work for a financial services company, so we have strict compliance requirements. All our code goes through multiple rounds of security review. I'm implementing a new API endpoint that accepts credit card information, and I want to make sure I'm following best practices..." | ~52 | "Implement PCI-compliant API endpoint for credit cards:" | ~11 | **79%** |
| **5.7** | "Our application has been experiencing intermittent crashes in production. The DevOps team has been working on improving observability and we've recently added more logging and monitoring. Looking at the stack traces from the last crash, it seems to be related to the caching layer..." | ~50 | "Debug crash related to caching layer. Stack trace: \[paste trace\]" | ~13 | **74%** |
| **5.8** | "As part of our Q3 initiatives, we're focusing on improving developer experience. One of the items on our roadmap is to improve the build times. Currently, our CI pipeline takes about 20 minutes per run, which is slowing down our team. Can you help me figure out how to speed it up?" | ~58 | "Speed up CI pipeline (currently 20 min):" | ~9 | **84%** |
| **5.9** | "I've been reading about different testing strategies. I've tried unit tests, integration tests, and end-to-end tests. I'm trying to figure out the right balance for our project. The codebase has about 50,000 lines of TypeScript and we have about 200 test files currently..." | ~53 | "Recommend test strategy for 50K-line TS codebase with 200 test files:" | ~16 | **70%** |
| **5.10** | "Our team is considering adopting a new linter configuration. We've been using ESLint with the Airbnb config, but we're thinking of switching to the Biome linter because it's faster. Before we make the switch, I want to understand the trade-offs..." | ~47 | "Compare ESLint (Airbnb) vs Biome linter:" | ~10 | **79%** |

------------------------------------------------------------------------

### 6. Remove Hedging and Qualifiers

Hedging adds uncertainty without adding information. LLMs don't need softened language.

#### Common Hedges Reference

|                             |        |                              |
|-----------------------------|--------|------------------------------|
| Hedge                       | Tokens | Action                       |
| "It seems like maybe"       | 4      | Remove                       |
| "I think perhaps"           | 3      | Remove                       |
| "It might be the case that" | 6      | Remove                       |
| "It's possible that"        | 4      | Remove                       |
| "Sort of" / "Kind of"       | 2      | Remove                       |
| "A bit" / "A little"        | 2-3    | Remove                       |
| "Somewhat"                  | 1      | Remove                       |
| "Basically"                 | 1      | Remove                       |
| "Essentially"               | 1      | Remove                       |
| "Actually"                  | 1      | Remove (usually unnecessary) |
| "Really" (as intensifier)   | 1      | Remove                       |
| "Very" (as intensifier)     | 1      | Remove (usually)             |

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **6.1** | "I think perhaps this function might possibly be doing too much. It seems like maybe we should break it down into smaller pieces." | 27 | "This function does too much. Break it into smaller pieces." | 12 | **56%** |
| **6.2** | "It's possible that the bug is somewhat related to the caching layer, but I'm not entirely sure." | 20 | "The bug may be in the caching layer." | 9 | **55%** |
| **6.3** | "This is basically a really simple function that essentially just returns the user's name, but I actually think we might want to add some validation." | 30 | "Add validation to the user name function." | 9 | **70%** |
| **6.4** | "It might be the case that we need to refactor this class, because it seems like it's becoming somewhat unwieldy and difficult to maintain." | 28 | "Refactor this class -- it's hard to maintain." | 11 | **61%** |
| **6.5** | "I was sort of wondering if maybe we could possibly think about considering whether we should perhaps add some kind of caching here." | 26 | "Add caching here." | 4 | **85%** |
| **6.6** | "The performance is actually quite slow, and I think it's probably due to the fact that we're basically making too many database calls." | 28 | "Performance is slow due to too many DB calls." | 11 | **61%** |
| **6.7** | "This code is kind of messy and it's somewhat hard to follow. I think we should probably clean it up at some point." | 26 | "Clean up this messy code." | 6 | **77%** |
| **6.8** | "It seems like maybe there might be a race condition here, but I'm not 100% sure about it." | 21 | "Check for race condition here." | 7 | **67%** |
| **6.9** | "The API response is actually really slow, and I'm thinking it might be because of the way we're essentially doing multiple queries." | 26 | "Slow API response -- likely due to multiple queries." | 11 | **58%** |
| **6.10** | "I feel like we should probably add some tests here, because it's kind of important to make sure the code works correctly." | 24 | "Add tests here." | 4 | **83%** |

------------------------------------------------------------------------

### 7. Convert Questions to Commands

Commands are more direct and consume fewer tokens than questions.

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before (Question) | Tokens | After (Command) | Tokens | Savings |
| **7.1** | "Could you please tell me what this function does?" | 11 | "Explain this function:" | 5 | **55%** |
| **7.2** | "Would you be able to refactor this code to be more readable?" | 14 | "Refactor this for readability:" | 7 | **50%** |
| **7.3** | "Is there a way to make this query run faster?" | 12 | "Optimize this query:" | 5 | **58%** |
| **7.4** | "Do you think you could write some tests for this?" | 12 | "Write tests for this:" | 6 | **50%** |
| **7.5** | "What would be the best way to handle this error?" | 12 | "Handle this error:" | 5 | **58%** |
| **7.6** | "Can you help me understand how this algorithm works?" | 11 | "Explain this algorithm:" | 5 | **55%** |
| **7.7** | "Would it be possible to add pagination to this endpoint?" | 12 | "Add pagination to this endpoint:" | 7 | **42%** |
| **7.8** | "How can I improve the security of this API?" | 11 | "Improve API security:" | 5 | **55%** |
| **7.9** | "Is it a good idea to use Redis for session storage?" | 13 | "Evaluate Redis for session storage." | 7 | **46%** |
| **7.10** | "What's the correct way to handle authentication in React?" | 12 | "Explain React authentication patterns:" | 7 | **42%** |

------------------------------------------------------------------------

### 8. Eliminate Self-Reference and Meta-Talk

Remove phrases where you talk about the conversation instead of the task.

#### Common Meta-Talk Patterns

|                              |        |                     |
|------------------------------|--------|---------------------|
| Pattern                      | Tokens | Action              |
| "As I mentioned earlier"     | 4      | Remove              |
| "Going back to what I said"  | 6      | Remove              |
| "To summarize what I want"   | 5      | Remove              |
| "Let me explain what I need" | 5      | Just explain it     |
| "What I'm trying to do is"   | 5      | Just state the goal |
| "The thing is"               | 3      | Remove              |
| "The way I see it"           | 5      | Remove              |
| "From my perspective"        | 3      | Remove              |
| "I guess what I'm asking is" | 6      | Just ask            |
| "If that makes sense"        | 4      | Remove              |

#### Before/After Examples

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| \# | Before | Tokens | After | Tokens | Savings |
| **8.1** | "As I mentioned earlier, we need to add authentication. Let me explain what I need -- I want to use JWT tokens, if that makes sense." | 28 | "Add JWT-based authentication." | 5 | **82%** |
| **8.2** | "What I'm trying to do is optimize this query. The thing is, it's currently running really slow. I guess what I'm asking is, how can we speed it up?" | 33 | "Optimize this slow query:" | 5 | **85%** |
| **8.3** | "Going back to what I said about the database, from my perspective, the way I see it, we probably need to add some indexes." | 26 | "Add indexes to the database." | 7 | **73%** |
| **8.4** | "Let me explain what I need. I'm trying to understand how to implement pagination. The thing is, I want to use cursor-based pagination if that makes sense." | 31 | "Implement cursor-based pagination." | 4 | **87%** |
| **8.5** | "To summarize what I want, what I'm asking is basically this: can you help me set up a CI/CD pipeline?" | 22 | "Set up a CI/CD pipeline." | 7 | **68%** |
| **8.6** | "From my perspective, the way this should work is we need to add error handling. The thing is, we need to do it without breaking existing code, if that makes sense." | 34 | "Add error handling without breaking existing code." | 9 | **74%** |
| **8.7** | "As I mentioned, we need to migrate the database. I guess what I'm trying to figure out is what's the best approach for zero-downtime migration." | 30 | "Plan a zero-downtime database migration." | 7 | **77%** |
| **8.8** | "Let me explain the situation. The thing is, we have a memory leak. What I'm trying to do is find and fix it, if that makes sense." | 27 | "Find and fix the memory leak." | 7 | **74%** |
| **8.9** | "Going back to our earlier discussion, from my perspective, we should probably refactor the service layer to follow clean architecture principles." | 26 | "Refactor service layer to follow clean architecture." | 9 | **65%** |
| **8.10** | "What I'm trying to understand is, basically, how does the garbage collector work in Node.js? I guess what I'm really asking is, when does it run?" | 31 | "Explain Node.js garbage collection: when does it run?" | 12 | **61%** |

------------------------------------------------------------------------

### 9. Cumulative Impact

Applying multiple techniques to the same prompt compounds savings.

#### Progressive Compression Example

**Original prompt:**

> "I would really appreciate it if you could please help me out with something. As I mentioned earlier, we've been working on this project for a while now, and we've been experiencing some performance issues. I was wondering if maybe you could take a look at the UserAuthenticationService code and let me know what you think. It might be the case that there's a way to optimize the database queries to the PostgreSQL database, but I'm not entirely sure. The thing is, I want to make sure we handle authentication and authorization correctly, and I'd like you to basically suggest some improvements. Could you please also make sure to add comments to explain complex logic, include docstrings for all functions, and document any non-obvious behavior? If that makes sense, I'd really value your input on this."

**Token count:** ~130 tokens

#### Compression Stages

|  |  |  |  |  |
|----|----|----|----|----|
| Stage | Technique Applied | Result | Tokens | Cumulative Savings |
| **Original** | None | (full verbose prompt above) | 130 | Baseline |
| **Stage 1** | Remove filler phrases (§1) | "Help me with something. We've been working on this project for a while and experiencing performance issues. Take a look at the UserAuthenticationService code. It might be the case that there's a way to optimize the database queries to the PostgreSQL database. The thing is, I want to make sure we handle authentication and authorization correctly. Suggest some improvements. Add comments to explain complex logic, include docstrings for all functions, and document any non-obvious behavior." | 78 | 40% |
| **Stage 2** | Remove hedging (§6) | "Help me with something. We've been working on this project and experiencing performance issues. Take a look at the UserAuthenticationService code. Optimize the database queries to the PostgreSQL database. Handle authentication and authorization correctly. Suggest improvements. Add comments for complex logic, docstrings for all functions, document non-obvious behavior." | 62 | 52% |
| **Stage 3** | Eliminate meta-talk (§8) | "Take a look at the UserAuthenticationService code. Optimize database queries to PostgreSQL. Handle authentication and authorization correctly. Suggest improvements. Add comments for complex logic, docstrings for functions, document non-obvious behavior." | 44 | 66% |
| **Stage 4** | Eliminate unnecessary context (§5) | "Optimize UserAuthenticationService. Focus: DB queries, auth correctness. Add docstrings + comments for complex logic." | 26 | 80% |
| **Stage 5** | Use abbreviations (§4) | "Optimize `SVC`. Focus: `DB` queries, authN/authZ correctness. Add docstrings + comments for complex logic." | 22 | 83% |
| **Stage 6** | Consolidate instructions (§2) | "Optimize `SVC` (`DB` queries, authN/authZ). Add docstrings + complex-logic comments." | 17 | **87%** |

#### Final Result

|                |         |                       |
|----------------|---------|-----------------------|
| Version        | Tokens  | Cost (Sonnet 4 input) |
| **Original**   | 130     | \$0.00039             |
| **Compressed** | 17      | \$0.00005             |
| **Savings**    | **87%** | **\$0.00034/request** |

At **10,000 requests/day**, this saves **3.40/���=102/month** on just this one prompt template.

------------------------------------------------------------------------

### Quick Reference: All Techniques

|  |  |  |  |  |
|----|----|----|----|----|
| \# | Technique | Section | Typical Savings | Example Count |
| 1 | Remove Filler Phrases | [§1](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#1-remove-filler-phrases) | 54-86% | 10 |
| 2 | Consolidate Redundant Instructions | [§2](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#2-consolidate-redundant-instructions) | 41-69% | 10 |
| 3 | Replace Prose with Constraints | [§3](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#3-replace-prose-with-constraints) | 35-81% | 10 |
| 4 | Use Abbreviations | [§4](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#4-use-abbreviations-for-repeated-terms) | 39-64% | 10 |
| 5 | Eliminate Unnecessary Context | [§5](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#5-eliminate-unnecessary-context) | 69-86% | 10 |
| 6 | Remove Hedging | [§6](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#6-remove-hedging-and-qualifiers) | 55-85% | 10 |
| 7 | Convert Questions to Commands | [§7](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#7-convert-questions-to-commands) | 42-58% | 10 |
| 8 | Eliminate Meta-Talk | [§8](http://localhost:63342/markdownPreview/1084283343/markdown-preview-index-82kkgmu13jl91919edgre5f5g6.html#8-eliminate-self-reference-and-meta-talk) | 61-87% | 10 |

**Combined, these techniques typically achieve 70-90% token reduction on verbose prompts while maintaining or improving output quality.**

------------------------------------------------------------------------

### Related Documents

- [Manual Prompt Engineering & Compression Techniques (Main)](file:///C:/Users/dvtliem/Kepler/docs/prompt_compression/04-manual-techniques.md)

- [Skills-Based Compression](file:///C:/Users/dvtliem/Kepler/docs/prompt_compression/05-skills-based-compression.md)

- [Claude Code & API Compaction](file:///C:/Users/dvtliem/Kepler/docs/prompt_compression/02-claude-compression.md)

- [Gemini CLI Compression](file:///C:/Users/dvtliem/Kepler/docs/prompt_compression/03-gemini-compression.md)

- [LLMLingua: ML-Based Compression](file:///C:/Users/dvtliem/Kepler/docs/prompt_compression/01-llmlingua.md)

### Sources

- [Anthropic: Prompting Best Practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)

- [Portkey: Optimize Token Efficiency](https://portkey.ai/blog/optimize-token-efficiency-in-prompts/)

- [IBM: Token Optimization](https://developer.ibm.com/articles/awb-token-optimization-backbone-of-effective-prompt-engineering/)

- [Redis: LLM Token Optimization](https://redis.io/blog/llm-token-optimization-speed-up-apps/)

%% ai-graph-start %%

**Related notes:**
- [[Text Compression Techniques - Examples]]
- [[Manual Prompt Compression Techniques - Deep Dive]]
- [[Manual Prompt Compression Techniques - Deep Dive]]
- [[Prompt Performance Code Review]]
- [[Prompt Security Code Review]]

%% ai-graph-end %%