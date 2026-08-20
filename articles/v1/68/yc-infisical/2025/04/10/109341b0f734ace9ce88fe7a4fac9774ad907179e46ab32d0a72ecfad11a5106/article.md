---
schema_version: "1.0.0"
document_id: "109341b0f734ace9ce88fe7a4fac9774ad907179e46ab32d0a72ecfad11a5106"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/vibe-coding-security-playbook"
published_at: "2025-04-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:4826086a25ec75372588584f48598e4349c2167e3611dda577208f23dc7e283b"
---

# A Vibe Coding Security Playbook: Keeping AI-Generated Code Safe

## What's Vibe Coding?


Vibe coding is a new term[coined by Andrej Karpathy](https://twitter.com/karpathy/status/1886192184808149383) to describe a new approach to software development that leverages AI tools to handle most of the coding work. In a tweet that went viral, he explained:


> There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and **forget that the code even exists** . It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. \[...\] I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works.


Vibe coding involves heavy reliance on AI tools to generate, refine, and debug code, allowing developers to rapidly iterate and deploy applications with minimal manual coding effort.


This practice is rapidly gaining popularity and has since been featured[in the New York Times](https://www.nytimes.com/2025/02/27/technology/personaltech/vibecoding-ai-software-programming.html) ,[Ars Technica](https://arstechnica.com/ai/2025/03/is-vibe-coding-with-ai-gnarly-or-reckless-maybe-some-of-both/) ,[the Guardian](https://www.theguardian.com/technology/2025/mar/16/ai-software-coding-programmer-expertise-jobs-threat) and countless online discussions.


It's particularly appealing to people who don't speak code yet want to create their own app, for fun or even for profit. **Yet, over-reliance on AI-generated code can lead to security vulnerabilities, inefficiencies, and errors if not carefully reviewed by developers.**


## The OWASP Top 10: A Security Foundation for Vibe Coders


Before diving into specific vulnerabilities, let's establish why security matters for vibe coders. The[OWASP Top 10](https://owasp.org/www-project-top-ten/) represents the most critical web application security risks and provides an excellent framework for understanding potential issues in AI-generated code.


AI code generators have specific limitations that make them prone to introducing these security issues:


Limitation Description Security Impact


**Pattern Reproduction** AIs replicate code patterns from training data without understanding security implications May reproduce vulnerable code that's common in public repositories


**Context Blindness** AI lacks awareness of the broader application security context Generates code that might be secure in isolation but creates vulnerabilities when integrated


**Training on Legacy Code** Large portions of training data contain outdated security practices Propagates deprecated patterns that don't meet modern security standards


**Incomplete Implementation** AI focuses on functional requirements rather than security requirements Omits crucial validation, error handling, and security checks


## Major Security Risks in AI-Generated Code


Let's examine the most critical security vulnerabilities you're likely to encounter when using AI coding assistants, with practical examples and solutions for each.


### 1. Data Security Vulnerabilities


#### Hardcoded Credentials and Exposed Secrets


AI code assistants frequently suggest hardcoding credentials directly in source code. Consider this example:


```text
import { Pool } from 'pg';


// Create a connection pool to PostgreSQL
const pool = new Pool({
user: 'postgres',
host: 'localhost',
database: 'myapp',
password: 'admin123',  // Hardcoded password!
port: 5432,
});


```


This approach creates serious vulnerabilities because:


- Credentials are visible to anyone with access to the codebase
- Secrets can persist in git history indefinitely
- Rotating credentials requires code changes
- Different environments require hardcoded credential variations


According to[a recent report](https://www.gitguardian.com/files/the-state-of-secrets-sprawl-report-2025) by GitGuardian, nearly 24 million secrets were inadvertently exposed on GitHub just last year, with repositories using AI coding tools showing a 40% higher rate of secret exposure.


Read:[What is Secret Sprawl and How to Solve It?](https://infisical.com/blog/what-is-secret-sprawl)


Some concerns have even been raised recently about AI coding assistants with read access to the entire codebase[leaking sensitive credentials stored in .env files](https://forum.cursor.com/t/env-file-question/60165) to remote servers, or overriding the .gitignore file, leading to a risk of committing sensitive .env files to git.


**Secure alternative:** Use environment variables or a dedicated secrets management solution. Here's a simple environment variable approach:


```text
import { Pool } from 'pg';


// Create a connection pool using environment variables
const pool = new Pool({
user: process.env.DB_USER,
host: process.env.DB_HOST,
database: process.env.DB_NAME,
password: process.env.DB_PASSWORD,
port: parseInt(process.env.DB_PORT || '5432'),
});


```


For more robust solutions, consider dedicated secrets managers like Infisical, AWS Secrets Manager, or HashiCorp Vault, which offer:


- Separation of code and credentials
- Granular access control
- Automated secret rotation
- Audit logging capabilities
- Environment-specific configuration


### 2. Input Validation Vulnerabilities


#### SQL Injection


AI-generated code often prioritizes functionality over security, leading to unsafe database queries:


```text
// VULNERABLE CODE
import { Pool } from 'pg';


export async function getUserData(username: string): Promise<any> {
const pool = new Pool();
const query = `SELECT * FROM users WHERE username = '${username}'`;
const result = await pool.query(query);
return result.rows[0];
}


```


This code directly concatenates user input into a SQL query without proper sanitization or parameterization. An attacker could input a value like` admin' OR '1'='1` to bypass authentication or execute arbitrary SQL commands.


**Secure alternative:**


```text
// SECURE CODE
import { Pool } from 'pg';


export async function getUserData(username: string): Promise<any> {
const pool = new Pool();
// Use parameterized queries to prevent SQL injection
const query = 'SELECT * FROM users WHERE username = $1';
const result = await pool.query(query, [username]);
return result.rows[0];
}


```


#### Cross-Site Scripting (XSS)


AI assistants often miss proper output encoding, creating XSS vulnerabilities:


```text
// VULNERABLE CODE
import express from 'express';


const app = express();


app.get('/search', (req, res) => {
const searchTerm = req.query.term as string;
res.send(`<h1>Search results for: ${searchTerm}</h1>`);
});


```


This code inserts user input directly into HTML without sanitization, allowing attackers to inject malicious scripts that execute in users' browsers.


**Secure alternative:**


```text
// SECURE CODE
import express from 'express';
import escapeHtml from 'escape-html';


const app = express();


app.get('/search', (req, res) => {
const searchTerm = escapeHtml(req.query.term as string);
res.send(`<h1>Search results for: ${searchTerm}</h1>`);
});


```


### 3. Authentication and Authorization Flaws


AI assistants often generate code that handles user authentication but lacks critical security protections:


```text
// VULNERABLE CODE
function loginUser(username, password) {
const user = findUserByUsername(username);
if (user && user.password === password) {
return generateToken(user.id);
}
return null;
}


```


This implementation has multiple security issues:


- Passwords are compared in plaintext rather than being hashed
- No protection against brute force attacks
- No account lockout mechanism
- No logging of failed login attempts


**Secure alternative:**


```text
// SECURE CODE
import bcrypt from 'bcrypt';


async function loginUser(username, password, ip) {
// Rate limiting check
if (isRateLimited(ip)) {
logFailedAttempt(ip, username, 'rate_limited');
throw new Error('Too many attempts. Try again later.');
}


const user = await findUserByUsername(username);


// Always perform hash comparison even if user isn't found to prevent timing attacks
const passwordValid = user ? await bcrypt.compare(password, user.passwordHash) : false;


if (!user || !passwordValid) {
logFailedAttempt(ip, username, 'invalid_credentials');
throw new Error('Invalid username or password');
}


return generateToken(user.id);
}


```


## Security Review Guide for Vibe Coders


When reviewing AI-generated code, be vigilant for these common warning signs:


### Security Red Flags


-


**Sensitive Information Exposure**


- Hardcoded credentials, API keys, or tokens
- Database connection strings with embedded passwords
- Debug or error messages revealing internal details


-


**Unsafe Data Handling**


- Unparameterized queries (SQL, NoSQL, LDAP)
- Direct insertion of user input into HTML, JavaScript, or command strings
- Missing input validation or reliance only on client-side validation


-


**Weak Security Controls**


- Insufficient error handling that leaks sensitive information
- Missing or weak authentication mechanisms
- Absence of authorization checks on sensitive functions
- Outdated cryptographic methods (MD5, SHA1, DES)


-


**Configuration Issues**


- Development features enabled in production code
- Overly permissive CORS settings
- Unnecessary services or features enabled by default


## Best Practices for Secure Vibe Coding


Implement these practical guidelines to maximize the benefits of AI code assistants while minimizing security risks:


### Prompt Engineering for Security


The way you phrase your prompts significantly affects the security of generated code:


**Instead of:**


```text
Write me a login function


```


**Try:**


```text
Write a secure login function using bcrypt for password hashing, with rate limiting and protection against timing attacks, following OWASP best practices


```


### Multi-Stage Security Reviews


When working with AI-generated code, adopt this review workflow:


1.


**Initial Generation** : Get your working code solution


2.


**Security Verification** : Follow up with prompts like:


- "What security vulnerabilities might exist in this code?"
- "How can we improve error handling to prevent information leakage?"
- "Are there any edge cases that could lead to security issues?"


3.


**Challenge Testing** : Test with problematic inputs:


- "How would this code handle a user input of: admin'; DROP TABLE users; --"
- "What happens if a file upload contains malicious content?"


### Automate Security Checks


Integrate security tools into your development workflow:


Security Layer Tools to Consider


**Code Analysis** SonarQube, ESLint Security Plugin, Semgrep


**Dependency Scanning** OWASP Dependency-Check, Snyk, GitHub Dependabot


**Secret Detection** GitLeaks, TruffleHog, detect-secrets


**Runtime Protection** RASP tools, WAF configuration


### Educate Yourself on Core Security Concepts


Even with minimal coding experience, understanding these fundamental security principles will help you evaluate AI-generated code:


- **Defense in Depth** : Multiple layers of protection are better than a single strong defense
- **Principle of Least Privilege** : Give components access only to what they absolutely need
- **Input Validation** : Never trust external input without proper validation
- **Secure by Default** : Start with the most secure configuration and only open what's necessary


## Conclusion: Responsible Vibe Coding


Vibe coding is an exciting development that makes software creation accessible to more people than ever before. However, with this power comes responsibility, particularly around security.


Treat AI assistants as collaborators whose work requires careful review rather than infallible experts whose code can be blindly trusted. By applying the security principles outlined in this playbook, you can confidently vibe code while keeping your applications secure.


Remember: The most secure code is code that you understand. Take time to have your AI assistant explain its security choices and learn from them as you go.


Next, check our guide on[how to manage secrets in your MCP servers](https://infisical.com/blog/managing-secrets-mcp-servers) to learn how to keep your secrets safe and secure.
