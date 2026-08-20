---
schema_version: "1.0.0"
document_id: "2d343bb62a56d8763dee5beb1b14ced89cc07ab53182d35fcebbf38d5a715f02"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/tutorial-end-to-end-hot-reload-style-previews-with-vercel-signadot/"
published_at: "2025-12-12T20:43:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:f4af2080cc5fb7dfb3e708b47a4c5ce70ccee27f2656ee1e39762ae24725fd6a"
---

# Tutorial: End-to-End Hot-Reload‑Style Previews with Vercel + Signadot

**Goal:** Get a hot‑reload‑style workflow where every backend change is previewable as quickly as frontend changes on Vercel.


Frontend developers are used to instant feedback: save a file, and the browser updates almost immediately. Vercel previews bring a similar experience to pull requests, where every push spins up a fresh frontend URL. But the backend usually lags behind: changes wait for a staging deploy, reviewers test against stale APIs, and full‑stack PRs become slow to validate.


This tutorial shows how to pair Vercel Preview Deployments with Signadot Sandboxes so that:


1. Every PR builds a fresh backend image and launches a PR‑scoped backend sandbox.
2. The frontend preview is wired to that sandbox via` NEXT_PUBLIC_API_URL` and a secure API proxy.
3. GitHub Actions posts both URLs to the PR so reviewers can exercise the feature end‑to‑end.
4. Pushing more commits to the PR updates both sides, giving a fast-feedback, hot‑reload‑like loop for backend and frontend together.


**Time required:** 45–60 minutes
**Repository:**[https://github.com/signadot/examples/tree/main/vercel-preview-signadot-sandoxes-cicd-connection](https://github.com/signadot/examples/tree/main/vercel-preview-signadot-sandoxes-cicd-connection)


**Stack note:** The sample app uses a Next.js frontend and a Node/Express backend, but the workflow applies to any framework that can read build‑time environment variables and expose a stable Kubernetes deployment for Signadot to clone.


## 1. Introduction


### 1.1 Problem: Fast Frontend, Slow Backend


- Vercel previews give instant frontend previews per PR, but they usually point at a static staging/production backend.
- When PRs span both frontend and backend, reviewers test UI changes against outdated APIs.
- If staging is broken, every frontend PR looks broken, even when the frontend code is fine.


### 1.2 Solution: End‑to‑End Hot Reload with Sandboxes


- Create a Signadot sandbox for each PR that clones the baseline backend deployment and swaps in the PR’s image.
- Inject the sandbox URL into the Next.js build via` NEXT_PUBLIC_API_URL` , and route calls through a server‑side proxy that keeps the Signadot API key private.
- Use GitHub Actions to orchestrate:


- backend image build & push,
- sandbox creation,
- Vercel preview deployment wired to the sandbox URL,
- and a PR comment with both preview links.


**Result:** a full‑stack, hot‑reload‑style workflow where every push to a PR spins up matching frontend and backend changes, with instant backend previews tied to the Vercel preview you already use.


## 2. Key Concepts


- **Vercel Preview Deployment (Frontend Hot Reload Experience)**
Every PR and push gets its own frontend URL. Frontend developers already rely on this for a fast feedback loop during code review.
- **Signadot Sandbox (Instant Backend Preview)**
A sandbox is a PR‑scoped backend environment that clones your baseline Kubernetes deployment and overrides just the parts you are changing (for example, the backend image tag). This brings a hot‑reload‑like loop to backend changes.
- **Full‑Stack Hot‑Reload‑Style Workflow**
By wiring the Vercel preview to the PR’s sandbox URL:


- Each PR gets a dedicated backend + frontend pair.
- Reviewers see the exact backend behavior that the frontend expects.
- Pushing another commit updates the backend image and redeploys the preview, preserving the fast‑feedback experience across the whole stack.


### 2.1 Architecture Overview


## 3. Prerequisites


- GitHub repositories - Separate frontend (` next.js` ) and backend (` express` ) repos, or a monorepo
- Vercel account - Project wired to the frontend repo, API token for GitHub Actions
- Signadot account - Organization name, API key, access to a Kubernetes cluster with the Signadot Operator installed
- Kubernetes cluster - AWS EKS or GKE Standard (Operator does not run on GKE Autopilot)
- Container registry - Docker Hub / GHCR / GCR for pushing backend images


**Tip:** Ensure the Signadot Operator is installed ahead of time. The frontend workflow only verifies the operator; it does not install it.


## 4. Baseline Environment (One‑Time Setup)


In this section you prepare the baseline backend environment that Signadot will clone for each PR. This is a one‑time setup per cluster.


### 4.1 Install the Signadot Operator


Install the operator on your target Kubernetes cluster (for example, an EKS cluster used by your backend):


**Checkpoint: Operator ready**


-


Run:


-


**Expected:** At least one` signadot-operator` pod is in` Running` state.


### 4.2 Deploy the Baseline Backend


The backend (` backend/` ) is a minimal Express server with Kubernetes manifests under` backend/k8s/` .


1. Update the deployment image to point at your registry:


` backend/k8s/deployment.yaml`


1. Apply the manifests to your cluster:


**Checkpoint: Baseline backend healthy**


-


Run:


-


**Expected:**` AVAILABLE` replicas is at least` 1` .


For a local quick check (optional):


You should see a JSON response with` status: "healthy"` .


### 4.3 Sandbox Blueprint (` backend/sandbox.yaml` )


` backend/sandbox.yaml` defines how Signadot creates a PR‑scoped backend sandbox from the baseline deployment:


Key ideas:


- ` name:` is replaced with a PR‑specific identifier (for example,` backend-pr-42` ).
- ` value:` is replaced with the actual image reference built by backend CI (for example,` docker.io/alice/vercel-signadot-backend:branch-sha` ).
- ` defaultRouteGroup` exposes an instant backend preview URL such as:


- ` https://backend-api--backend-pr-42.sb.signadot.com`


This is the URL we will wire into` NEXT_PUBLIC_API_URL` for a hot‑reload‑style backend experience.


## 5. Application Configuration (Frontend + Backend)


With the baseline environment ready, configure the sample app to take advantage of it.


### 5.1 Frontend: Hot‑Reload‑Style Backend URL via` NEXT_PUBLIC_API_URL`


The frontend (Next.js 13+) reads its backend URL from` NEXT_PUBLIC_API_URL` and automatically proxies sandbox calls through a server‑side route.


**` frontend/src/lib/config/api.ts`**


**` frontend/src/app/api/proxy/\[...path\]/route.ts`**


This route keeps the Signadot API key server‑side while still giving the frontend an instant backend preview:


All sandbox requests go through` /api/proxy/*` , so` SIGNADOT_API_KEY` is never exposed in client‑side bundles.


**Example component usage**


**Checkpoint: Local full‑stack dev**


- Start the backend locally on port` 8080` .
- Run the frontend with` NEXT_PUBLIC_API_URL=http://localhost:8080` .
- **Expected:** the sample page renders the backend health data successfully.


## 6. Integration with Sandboxes (GitHub Workflows)


Now that the baseline environment and app configuration are in place, wire them together using GitHub Actions. The goal is to automate the fast‑feedback loop for every PR.


### 6.1 Backend CI: Build and Push Images Only


File:` backend/.github/workflows/ci.yml`


Purpose: build, tag, and push backend Docker images so Signadot sandboxes can pull **PR‑specific artifacts** . This workflow **does not** deploy to the cluster or install the operator; those are one‑time baseline steps from the previous section.


Highlights:


- Logs into` REGISTRY` using` DOCKERHUB_USERNAME` /` DOCKERHUB_TOKEN` .
- Tags images with a sanitized branch prefix, short SHA, and` latest` on the default branch.
- Outputs a canonical image reference that matches the registry path used in` sandbox.yaml` .


Secrets required in the **backend repo** :


**Checkpoint: Image available for sandboxes**


- After pushing a commit, check your registry (for example, Docker Hub).
- **Expected:** a new image tag exists for` vercel-signadot-backend` , matching the branch and SHA for your PR.


### 6.2 Frontend Preview Workflow: Full‑Stack Hot‑Reload‑Style Preview


File:` frontend/.github/workflows/vercel-preview.yml`


Triggered on` pull_request` , this workflow creates a PR‑scoped backend sandbox and deploys a Vercel preview wired to it:


1. Check out the frontend repo and backend repo (to read` sandbox.yaml` ).
2. Authenticate to AWS and configure` kubectl` for the cluster where the baseline backend and Signadot operator run.
3. Verify the Signadot operator namespace/pods exist (no install).
4. Rewrite **` backend/sandbox.yaml`** with the cluster name and backend image reference.
5. Create the Signadot sandbox and extract its` backend-api` URL.
6. Deploy to Vercel with` NEXT_PUBLIC_API_URL` set to the sandbox endpoint and` SIGNADOT_API_KEY` as a server‑side secret.
7. Comment on the PR with both frontend and backend preview URLs.


Key excerpt:


Secrets required in the **frontend repo** :


**Note:** This workflow uses the Signadot CLI (` signadot sandbox apply/get` ). You can substitute` signadot/sandbox-action` if desired; the logic is equivalent.


**Checkpoint: Full‑stack preview workflow**


- Open a PR against the frontend repo.
- In GitHub Actions, you should see:


- Backend CI building and pushing an image.
- Frontend preview workflow creating a sandbox and deploying to Vercel.


- **Expected:** the PR gets a comment with a Frontend Preview URL and a Backend Sandbox URL.


If you need a visual reference for secrets and API keys, see:


## 7. See It Work: Experiencing the Hot‑Reload‑Style Loop


This section walks through what it feels like to use the workflow, not just configure it.


### 7.1 Create a Test PR


1. Make a small change in the frontend (for example, update the text rendered by the` BackendStatus` component).
2. Optionally, make a small change in the backend (for example, add a field to the` /health` response).
3. Push your branch and open a PR against` main` or` master` .


**Checkpoint: Workflows running**


- In the frontend repo, you should see the Deploy Full‑Stack Preview workflow running.
- In the backend repo, you should see the Build and Push Backend Image workflow running.


### 7.2 Inspect the PR Comment and URLs


Once the workflows complete:


1. Scroll to the bottom of the PR to find the comment created by the frontend workflow.
2. It should list:


- Frontend Preview: a` vercel.app` URL.
- Backend Sandbox: a` *.sb.signadot.com` or` *.preview.signadot.com` URL.


These two URLs represent your end‑to‑end instant preview for that PR.


### 7.3 Validate the End‑to‑End Flow


1. Open the Frontend Preview URL in your browser.
2. Open DevTools → Network tab.
3. Trigger the UI behavior that hits the backend (for example, load the page that reads` /health` ).
4. Inspect the network requests:


- Requests from the browser should go to` /api/proxy/…` on the Vercel domain.
- The proxy (on the server side) forwards to a URL like:


- ` https://backend-api--backend-pr-<PR_NUMBER>.sb.signadot.com/health`


**Checkpoint: Instant backend preview**


- **Expected:** the response body reflects the backend version built for this PR, not the shared staging backend. You are now effectively experiencing a hot‑reload‑style backend workflow tied to your PR.


### 7.4 Push Another Change and Feel the Loop


To feel the **fast‑feedback loop** :


1. Change something in the backend (for example, modify the` status` message or add a new field to` /health` ).
2. Commit and push to the same PR branch.
3. Wait for:


- Backend CI to build and push a new image.
- The frontend preview workflow to recreate or update the sandbox with that new image and redeploy the preview.


4. Refresh the same Vercel preview (or the new preview URL if it changed).


**Checkpoint: Hot‑reload‑style full‑stack update**


- **Expected:** the frontend now shows the updated backend behavior, without manually promoting anything to staging. Every push gives you an instant backend preview wired to your frontend preview.


## 8. Troubleshooting


To debug locally:


## 9. Conclusion & Next Steps


By pairing Vercel previews with Signadot sandboxes, you:


- Bring the hot‑reload experience to your backend with instant backend previews per PR.
- Give reviewers a fast-feedback, end‑to‑end preview they can trust for every full‑stack change.
- Automate the whole flow with GitHub Actions, so the loop stays fast without extra manual steps.


From here you can:


- Add more services to the same sandbox to preview multi‑service changes.
- Apply the same pattern to other frontend frameworks that support build‑time env vars.
- Extend the workflows with checks (for example, integration tests) that also run against the sandbox.


### Additional Resources


- [Signadot Documentation](https://www.signadot.com/docs)
- [Vercel Preview Deployments](https://vercel.com/docs/deployments/preview-deployments)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
