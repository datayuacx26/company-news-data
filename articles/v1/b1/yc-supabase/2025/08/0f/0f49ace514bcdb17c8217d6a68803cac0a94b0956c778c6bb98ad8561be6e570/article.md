---
schema_version: "1.0.0"
document_id: "0f49ace514bcdb17c8217d6a68803cac0a94b0956c778c6bb98ad8561be6e570"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/testing-for-vibe-coders-from-zero-to-production-confidence"
published_at: "2025-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:d68a7d87c275b62343bdbf688f217a88eb79eebc436fc8b67e361bdc63881fe8"
---

# Testing for Vibe Coders: From Zero to Production Confidence

Testing feels like homework until your users find the bugs first. This guide shows you how to build a testing strategy that actually prevents production disasters without turning development into a slog. You will learn which tests matter, which tools are simple enough to stick with, and how to catch the bugs that embarrass you in front of users.


Supabase helps because it is just Postgres at the core with an integrated suite of tools. You can run a full local stack, write tests against real Postgres schema and policies, and promote changes the same way you ship code. Start simple and layer more as your app grows.


## Tests that actually matter#


Most developers write the wrong tests first. Unit tests feel productive because they are fast to write and always pass. But they miss the bugs that actually break your app in production.


Integration tests do the heavy lifting. They check that your database, API routes, auth, and third-party calls work together. These catch the "works on my machine" issues that unit tests miss entirely.


Start with integration tests on your core features. Add unit tests only for complex logic like price calculations, date handling, and data transforms where bugs are expensive. Save end-to-end tests for critical user flows like login, checkout, and content creation. Visual tests are optional unless pixel-perfect UI is your main value proposition.


This order catches real-world bugs without turning testing into a full-time job. You want tests that fail when something is actually broken, not tests that fail because you refactored a function name.


## Pick tools and stick with them#


Tool-hopping burns more hours than imperfect tools ever will. Pick one tool per category and move on. For JavaScript and TypeScript projects, use Jest or Vitest for unit and integration tests. For end-to-end testing, Playwright handles modern web apps better than Selenium ever did.


The secret weapon is Supabase local development. Running` supabase start` gives you a real Postgres database, auth system, and generated APIs on your machine. Your tests run against the same schema, Row Level Security policies, and API endpoints that your production app uses. No mocking, no fake data, no surprises when you deploy.


If you are building Python services, pytest works the same way. For testing SQL policies and functions directly, pgTAP lets you write tests in SQL, but save that for later when your database logic gets complex.


## Start with a minimal setup#


Prove your testing pipeline works before writing complex tests. Add these scripts to your package.json:


`
_10


{


_10


"scripts": {


_10


"test": "vitest run",


_10


"test:watch": "vitest",


_10


"test:e2e": "playwright test"


_10


}


_10


}


`


Write one simple test to verify everything works:


`
_10


import { expect, test } from 'vitest'


_10


_10


import { formatPrice } from '../src/lib/format'


_10


_10


test('formats cents into dollars', () => {


_10


expect(formatPrice(1999)).toBe('$19.99')


_10


expect(formatPrice(0)).toBe('$0.00')


_10


})


`


If this passes in watch mode and in continuous integration, your test harness is solid. Now you can point tests at your real application stack.


## Test against your real database#


Create a test client that connects to your local Supabase instance. Keep your service role keys secure and use the anonymous key for user-level operations:


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


export const supabase = createClient(


_10


process.env.SUPABASE_URL || 'http://localhost:54321',


_10


process.env.SUPABASE_ANON_KEY || 'your-local-anon-key'


_10


)


`


Write integration tests that verify your most critical systems work together. This test confirms that Supabase Auth, database triggers, and Row Level Security all work correctly:


`
_26


import { beforeEach, expect, test } from 'vitest'


_26


_26


import { supabase } from './setup'


_26


_26


beforeEach(async () => {


_26


await supabase.from('profiles').delete().neq('id', '')


_26


})


_26


_26


test('sign up creates a profile row via trigger', async () => {


_26


const email = \`test-${Date.now()}@example.com\`


_26


const { data, error } = await supabase.auth.signUp({


_26


email,


_26


password: 'Pass1234!',


_26


})


_26


_26


expect(error).toBeNull()


_26


expect(data.user?.email).toBe(email)


_26


_26


const { data: profile } = await supabase


_26


.from('profiles')


_26


.select('*')


_26


.eq('id', data.user?.id)


_26


.single()


_26


_26


expect(profile).toBeTruthy()


_26


})


`


One test covers authentication, database triggers, and data access policies. That is efficient testing.


## Focus on expensive failures first#


Write tests for the areas where bugs cost you the most money or reputation. Authentication and authorization failures expose user data or lock people out of their accounts. Money calculations that are wrong by even a penny destroy trust. Data validation bugs let malicious users break your application.


Test that logged-out users cannot access protected endpoints. Verify that users can only see their own data under Row Level Security. Confirm that session refresh works correctly. For business logic, verify that totals and taxes calculate correctly, discounts do not create negative prices, and webhook handlers are idempotent so duplicate deliveries do not double-charge customers.


Check that email addresses, dates, and user IDs are validated properly. Ensure that dangerous input gets rejected on the server side, not just in the browser. Test your critical user flows like signup, onboarding, checkout, content creation, and file uploads.


A single test in these areas prevents entire categories of production incidents. Focus your testing time where failure hurts the most.


## Test Supabase-specific features#


Row Level Security is easy to forget during development, and forgetting it leaves your database wide open. Write tests that prove users cannot see each other's data:


`
_31


test('users cannot see each other's posts', async () => {


_31


const u1 = await supabase.auth.signUp({


_31


email: 'u1@test.com',


_31


password: 'pass'


_31


})


_31


const u2 = await supabase.auth.signUp({


_31


email: 'u2@test.com',


_31


password: 'pass'


_31


})


_31


_31


await supabase.auth.signInWithPassword({


_31


email: 'u1@test.com',


_31


password: 'pass'


_31


})


_31


const { data: post } = await supabase


_31


.from('posts')


_31


.insert({ title: 'secret' })


_31


.select()


_31


.single()


_31


_31


await supabase.auth.signInWithPassword({


_31


email: 'u2@test.com',


_31


password: 'pass'


_31


})


_31


const { data: rows } = await supabase


_31


.from('posts')


_31


.select()


_31


.eq('id', post!.id)


_31


_31


expect(rows?.length ?? 0).toBe(0)


_31


})


`


Test database triggers that create profile rows after user signup or update timestamps on data changes. If your app relies on these triggers, make sure they fire correctly.


For file storage, test that uploads work but unauthorized users cannot read or delete files:


`
_10


test('upload to avatars bucket works', async () => {


_10


const file = new File(\['test'\], 'avatar.jpg', { type: 'image/jpeg' })


_10


const { data, error } = await supabase.storage


_10


.from('avatars')


_10


.upload(\`avatar-${Date.now()}.jpg\`, file)


_10


_10


expect(error).toBeNull()


_10


expect(data?.path).toBeTruthy()


_10


})


`


If you use Supabase Realtime for collaborative features, write a test that subscribes to table changes and verifies that events arrive after you insert data.


## Generate test data that looks real#


Your first few tests work fine with hardcoded values like` test-${Date.now()}@example.com` . But eventually you need to test pagination, search results, or how your app handles varied user data. Writing 50 manual insert statements gets old fast.


Start with simple helper functions that create test records:


`
_27


export async function createTestUser(overrides = {}) {


_27


const email = \`user-${Date.now()}@example.com\`


_27


const { data, error } = await supabase.auth.signUp({


_27


email,


_27


password: 'TestPass123!',


_27


...overrides,


_27


})


_27


_27


if (error) throw error


_27


return data.user


_27


}


_27


_27


export async function createTestPost(userId: string, overrides = {}) {


_27


const { data, error } = await supabase


_27


.from('posts')


_27


.insert({


_27


user_id: userId,


_27


title: 'Test post',


_27


content: 'Test content',


_27


...overrides,


_27


})


_27


.select()


_27


.single()


_27


_27


if (error) throw error


_27


return data


_27


}


`


Now your tests are cleaner:


`
_10


test('search returns relevant posts', async () => {


_10


const user = await createTestUser()


_10


await createTestPost(user.id, { title: 'JavaScript tips' })


_10


await createTestPost(user.id, { title: 'Python tricks' })


_10


_10


const { data } = await supabase.from('posts').select().textSearch('title', 'JavaScript')


_10


_10


expect(data).toHaveLength(1)


_10


})


`


When you need realistic variety, use Faker.js:


`
_10


npm install @faker-js/faker --save-dev


`


`
_22


import { faker } from '@faker-js/faker'


_22


_22


export async function createTestUser(overrides = {}) {


_22


const { data, error } = await supabase.auth.signUp({


_22


email: faker.internet.email(),


_22


password: 'TestPass123!',


_22


...overrides,


_22


})


_22


_22


if (error) throw error


_22


_22


await supabase


_22


.from('profiles')


_22


.update({


_22


display_name: faker.person.fullName(),


_22


bio: faker.lorem.paragraph(),


_22


avatar_url: faker.image.avatar(),


_22


})


_22


.eq('id', data.user.id)


_22


_22


return data.user


_22


}


`


For tests that need volume, write a seed script that populates your database with realistic data:


`
_31


// tests/seed.ts


_31


import { faker } from '@faker-js/faker'


_31


import { createClient } from '@supabase/supabase-js'


_31


_31


const supabase = createClient('http://localhost:54321', process.env.SUPABASE_SERVICE_ROLE_KEY)


_31


_31


async function seed() {


_31


// Create 10 users with posts


_31


for (let i = 0; i < 10; i++) {


_31


const { data: user } = await supabase.auth.admin.createUser({


_31


email: faker.internet.email(),


_31


password: 'TestPass123!',


_31


email_confirm: true,


_31


})


_31


_31


// Each user gets 3-7 posts


_31


const postCount = faker.number.int({ min: 3, max: 7 })


_31


for (let j = 0; j < postCount; j++) {


_31


await supabase.from('posts').insert({


_31


user_id: user.user.id,


_31


title: faker.lorem.sentence(),


_31


content: faker.lorem.paragraphs(3),


_31


published_at: faker.date.recent({ days: 30 }),


_31


})


_31


}


_31


}


_31


_31


console.log('Seed complete')


_31


}


_31


_31


seed()


`


Run it with` npx tsx tests/seed.ts` when you need fresh data. Better yet, add it to your database reset flow:


`
_10


supabase db reset && npx tsx tests/seed.ts


`


This gives you a baseline dataset that looks like real usage. Your pagination tests work correctly, search returns varied results, and you catch UI bugs that only show up with different name lengths or content volumes.


Keep your seed data simple at first. Add complexity only when you actually need to test against it. Ten users with a few posts each covers most testing scenarios. You can always generate more data for specific performance tests.


## Keep authentication tests simple#


OAuth testing (for Login with Google, Login with Apple, etc.) on localhost is painful, so mix your approaches. Mock external provider calls in unit tests to verify your callback logic works. Use Supabase Admin APIs in integration tests to create confirmed users quickly without going through the full signup flow. Use Playwright for one or two complete OAuth flows with a dedicated test application and saved login state.


This gives you fast feedback during development and confidence that production flows work correctly.


## Make async tests reliable#


Flaky tests destroy team confidence in your test suite. Always await promises in your tests and explicitly test error conditions. Use fake timers instead of sleeping to make time-dependent tests deterministic. Reset your database state between tests so they do not interfere with each other. Retry network calls in tests the same way your production code does.


If a test fails only in continuous integration, capture logs and debugging artifacts. Fix flaky tests immediately or delete them. A reliable test suite that catches real bugs is better than a comprehensive suite that cries wolf.


## Run tests in continuous integration#


Set up GitHub Actions to run your tests on every pull request and merge to main. Start Supabase locally in CI with` supabase start` and point your tests at the local instance. Split fast unit and integration tests from slower end-to-end tests into separate jobs. Gate your deployments on fast tests passing, but let end-to-end tests run in parallel.


Keep your CI builds fast by running tests in parallel and caching dependencies. Developers stop running tests if they take too long.


## Test-driven development with AI coding assistants#


Testing becomes even more important when you are using AI to write code quickly. Large language models are creative assistants, but they make subtle mistakes. A test suite turns your AI pair programmer from a creative helper into a reliable co-pilot.


The workflow is simple. Write or update a test that describes what you want. Ask the AI to implement the feature. Run the tests and feed any failures back to the model. The test is your contract. If the AI goes off track, the test catches it immediately.


This works especially well for API contract tests that verify status codes and response shapes, Row Level Security policies that prevent users from seeing each other's data, money calculations that prevent rounding errors, and webhook handlers that need to be idempotent.


Done correctly, tests make AI-assisted development faster and more reliable. You can iterate quickly without accidentally breaking existing functionality.


## Build in a weekend, test forever#


If you have been coding your project for a while and haven't started to add tests, don't worry. It's not too late. Here is how to retrofit testing onto your existing application and maintain good habits going forward.


### Add tests to your existing project#


Start by installing your testing framework and setting up Supabase local development:


`
_10


npm install vitest @supabase/supabase-js --save-dev


_10


supabase init


_10


supabase link --project-ref YOUR_PROJECT_ID


_10


supabase db pull


_10


supabase start


`


This captures your existing database schema as migration files and starts a local Supabase instance that matches your production setup.


Create a simple test configuration in` vitest.config.js` :


`
_10


import { defineConfig } from 'vitest/config'


_10


_10


export default defineConfig({


_10


test: {


_10


environment: 'node',


_10


setupFiles: \['./tests/setup.ts'\],


_10


},


_10


})


`


Write your first integration test for the most critical feature in your app. If it is a social app, test that users can create posts and see their own posts but not other users' posts. If it is an e-commerce app, test that the checkout calculation is correct. If it is a content management system, test that publishing and unpublishing work properly.


Pick the one feature that would hurt the most if it broke, and write a test for it first. This gives you immediate confidence that your core functionality works correctly.


### Test your authentication system#


Most weekend projects have basic authentication but skip Row Level Security. Write a test that creates two users, has one create some data, and verifies the other cannot see it:


`
_38


import { createClient } from '@supabase/supabase-js'


_38


_38


const supabase = createClient('http://localhost:54321', process.env.SUPABASE_ANON_KEY)


_38


_38


test('users cannot access each other data', async () => {


_38


// Create two test users


_38


const user1 = await supabase.auth.signUp({


_38


email: 'user1@test.com',


_38


password: 'password123',


_38


})


_38


_38


const user2 = await supabase.auth.signUp({


_38


email: 'user2@test.com',


_38


password: 'password123',


_38


})


_38


_38


// User 1 creates some data


_38


await supabase.auth.signInWithPassword({


_38


email: 'user1@test.com',


_38


password: 'password123',


_38


})


_38


_38


const { data: created } = await supabase


_38


.from('posts')


_38


.insert({ title: 'Private post' })


_38


.select()


_38


.single()


_38


_38


// User 2 tries to access it


_38


await supabase.auth.signInWithPassword({


_38


email: 'user2@test.com',


_38


password: 'password123',


_38


})


_38


_38


const { data: accessed } = await supabase.from('posts').select().eq('id', created.id)


_38


_38


expect(accessed).toHaveLength(0)


_38


})


`


If this test fails, you need to add Row Level Security policies to your tables. If it passes, your data is properly isolated between users.


### Add tests as you build new features#


From now on, write a test before you add each new feature. This prevents regressions and gives you confidence that changes work correctly. The pattern is simple: describe what the feature should do in a test, implement the feature, and verify the test passes.


For a new feature like user profiles, write the test first:


`
_21


test('users can update their own profile', async () => {


_21


const { data: user } = await supabase.auth.signUp({


_21


email: 'profile@test.com',


_21


password: 'password123',


_21


})


_21


_21


const { error } = await supabase


_21


.from('profiles')


_21


.update({ display_name: 'New Name' })


_21


.eq('id', user.user.id)


_21


_21


expect(error).toBeNull()


_21


_21


const { data: profile } = await supabase


_21


.from('profiles')


_21


.select('display_name')


_21


.eq('id', user.user.id)


_21


.single()


_21


_21


expect(profile.display_name).toBe('New Name')


_21


})


`


Then implement the feature and verify the test passes. This workflow catches bugs before they reach users and documents how your features are supposed to work.


### Set up continuous integration#


Add a GitHub Actions workflow that runs your tests on every push:


`
_21


name: Tests


_21


on: \[push, pull_request\]


_21


_21


jobs:


_21


test:


_21


runs-on: ubuntu-latest


_21


steps:


_21


- uses: actions/checkout@v4


_21


- uses: actions/setup-node@v4


_21


with:


_21


node-version: '18'


_21


- uses: supabase/setup-cli@v1


_21


_21


- name: Install dependencies


_21


run: npm ci


_21


_21


- name: Start Supabase


_21


run: supabase start


_21


_21


- name: Run tests


_21


run: npm test


`


This ensures your tests run in a clean environment and catch issues before they reach production. Tests that pass locally but fail in CI usually indicate missing environment setup or flaky timing assumptions.


### Maintain good testing habits#


Make testing part of your daily workflow. Run tests in watch mode while developing so you get immediate feedback when something breaks. Reset your local database regularly with` supabase db reset` to ensure your tests work against a clean schema.


When you fix a bug, write a test that would have caught it. This prevents the same bug from coming back and gradually improves your test coverage in the most important areas.


Review your tests monthly and delete ones that no longer add value. Tests that are hard to maintain or frequently break for trivial reasons hurt more than they help. Keep your test suite focused on the functionality that matters most to your users.


The goal is not perfect test coverage but reliable protection against the bugs that would hurt your business. A small suite of well-targeted tests beats a comprehensive suite that breaks constantly and slows down development.


## Your testing workflow#


Make these habits automatic. During daily development, run tests in watch mode, reset your local database when things get messy, and write a test when you fix any bug. For each pull request, ensure your tests pass locally, run a quick end-to-end check on critical flows, and fix any flaky tests immediately.


Monthly, review your test suite and remove obsolete tests, refresh your seed data to match current usage patterns, and update testing dependencies to stay current with security patches.


## You do not need perfect coverage#


You need tests in the right places that run against your real schema and integrate into your daily development flow. Supabase makes this straightforward because you can run the entire stack locally, test your actual Postgres policies and triggers, and deploy the same migrations you test with.


Start with integration tests for your core features, add a few end-to-end tests for critical user flows, protect your authentication and business logic, and automate the rest. Your users will notice fewer bugs, and you will ship new features with confidence instead of anxiety.
