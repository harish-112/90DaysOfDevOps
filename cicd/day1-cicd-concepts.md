*Why do companies use Continuous Delivery instead of Continuous Deployment?*

Most companies (especially in banking, healthcare, or e-commerce) prefer Continuous Delivery because:

They want to run manual QA testing or business checks in staging first.

They want to control when code goes live (e.g., waiting for midnight or a low-traffic window instead of releasing in the middle of peak hours).

They need compliance, security, or managerial sign-offs before pushing updates to real users.

1. Pipeline
A Pipeline is the overall automated workflow process that takes your application code from a developer's machine to production. It defines the entire sequence of steps required to build, test, scan, package, and deploy software.

Analogy: Think of a pipeline like a factory assembly line. Raw materials (code) go in one end, pass through multiple automated stations (testing, packaging), and come out as a finished product (deployed application) on the other end.

2. Workflow
A Workflow is an automated procedure that you configure in your repository using a YAML file (e.g., in GitHub Actions, stored under .github/workflows/). It defines when to run (via triggers like push, pull_request, or a schedule) and what jobs to execute. A single repository can have multiple workflows (e.g., one for running unit tests, another for publishing release notes).

Analogy: The blueprint or recipe for a specific task on the assembly line.

3. Job
A Job is a set of steps executed sequentially on the same build machine (called a Runner).

By default, multiple jobs within a workflow run in parallel (at the same time).

You can configure dependencies between jobs so that Job B only runs if Job A succeeds (e.g., Deploy only if Build & Test succeeds).

Analogy: A specific workstation on the assembly line with its own dedicated worker machine.

4. Step
A Step is an individual task or command executed inside a job. Steps inside the same job run sequentially, one after another, on the exact same runner machine, allowing them to share data and file systems. A step can either be:

A shell command / script (e.g., npm test, docker build).

An Action (a reusable module).

Analogy: A single action item on a worker's checklist at a workstation.

5. Action
An Action is a reusable, pre-written extension or building block that performs a complex or repetitive task inside a step. Instead of writing custom scripts for common tasks (like checking out a git repository, setting up Node.js, or uploading files to AWS S3), you can use community or official actions.

Analogy: A specialized power tool given to a worker to complete a specific task quickly without building the tool from scratch.


Task 1: The Problem
What can go wrong?

Merge Conflicts & Code Overwrites: Without automated testing/merging, developers can accidentally overwrite each other's work or push conflicting code.

Production Downtime: Pushing untested code manually directly to production leads to broken features, app crashes, and poor user experience.

Lack of Visibility: It's hard to trace which developer introduced a specific bug or breaking change.

What does "It works on my machine" mean and why is it a real problem?

It means an app runs perfectly on a developer's local laptop, but fails in staging or production.

Why it's a problem: Differences in OS versions, environment variables, dependencies, or database schemas cause unexpected failures. Developers waste valuable time troubleshooting configuration and dependency issues instead of writing features or fixing real bugs.

How many times a day can a team safely deploy manually?

1 time (or even less—often once a week/month). Manual deployments require high coordination, stressful manual regression testing, and carry a high risk of failure.

Task 2: CI vs CD vs CD
1. Continuous Integration (CI)
Definition: A practice where developers frequently merge code changes into a central repository. Once pushed, an automated CI server immediately builds the app and runs automated tests to catch bugs early and keep the main branch stable.

Real-World Example: Every time a developer opens a Pull Request on GitHub, GitHub Actions automatically runs unit tests and linter checks to ensure the code doesn't break existing features before it can be merged.

2. Continuous Delivery (CD)
Definition: Automatically builds, tests, and prepares code changes for release to a testing/staging environment. The code is always in a deployable state, but manual approval from a human (like a QA lead or manager) is required before pushing to production.

Real-World Example: A fintech app automatically runs tests and deploys code to a Staging server. Once verified, a Release Manager clicks a "Deploy to Production" button in Jenkins.

3. Continuous Deployment (CD)
Definition: Extends Continuous Delivery by automatically deploying every change that passes the automated pipeline directly to production with zero manual human intervention.

Real-World Example: Netflix or Spotify pushing hundreds of small code changes and microservice updates directly to live users every single day through fully automated pipeline checks.