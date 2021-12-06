<h1 class="submenu-hidden">What is webapp.io?</h1>

# Introduction

Welcome to the webapp.io docs. If you’d like to make any improvements, you can <a href="https://github.com/webappio/docs/tree/master/home">edit these docs</a> directly. Happy coding!

<div class="section-spacing"></div>

## What is webapp.io?

Webapp.io enables you to review changes to your projects within minutes instead of hours. We integrate with an existing repository on GitHub, GitLab, or BitBucket to provide customizable full-stack review environments directly into every pull request.

<div class="Video--Parent">
  <video class="Video--Child" controls autoplay muted>
    <source src="/static/assets/what-is-webapp-arrows.mp4" type="video/mp4">
  </video>
</div>

The first step is adding the Layerfile to the existing code on your repository. Once added, the next time you make a commit, webapp.io spins up a review environment where you can view your project and share the link with your team, designers, or key stakeholders. Similarly, if you or another developer makes a commit you’ll get another review environment which can be shared across your team (or external stakeholders).

Every time you push a new change, webapp.io leverages snapshots to create new copies of virtual machines (not containers!) in seconds by re-using instructions that have not been updated. 

As an example, instead of running “npm install” for every push you make, webapp.io takes a snapshot so that on a subsequent push we can skip the “npm install” step (unless your dependencies have changed).

<div class="section-spacing"></div>

## Beyond Review Environments

Webapp.io is well suited for creating full-stack review environment for every commit, but that’s far from the only thing it can do! We also support the following:

- Pull Request Template Builder (create a template to access important information for your webapp directly from your pull request)
- Polyrepositories (if you have multiple repo’s running your project, we support it)
- Review Environments on your Domain
- OAuth (logging in with external sites)

And much more! Check out our <a href="/docs/advanced-workflows">Advanced Workflows</a> documentation for more information.

<div class="section-spacing"></div>