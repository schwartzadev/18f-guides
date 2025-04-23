---
title: Project setup
sidenav: true
sticky_sidenav: true
permalink: /engineering/tools/project-setup/
redirect_from:
  - engineering/project-setup/
tags: engineering
layout: layouts/page
eleventyNavigation:
  parent: engineering_tools
  key: Project setup
  order: 3
  title: Project setup
subnav:
  - text: Source code management
    href: "#source-code-management"
  - text: Repository checklist
    href: "#repository-checklist"
  - text: Branch protection
    href: "#branch-protection-requirement"
  - text: Project management tool
    href: "#project-management-tool"
  - text: Continuous Integration/Continuous Deployment
    href: "#continuous-integration-continuous-deployment"
  - text: Code coverage metrics
    href: "#code-coverage-metrics"
  - text: Static analysis for code quality
    href: "#static-analysis-for-code-quality"
  - text: Dependency management
    href: "#dependency-management"
  - text: Deployment infrastructure
    href: "#deployment-infrastructure"
  - text: Infrastructure as Code
    href: "#infrastructure-as-code"
---

While the specific setup for each 18F project varies widely, there are certain elements that should be present in all source code repositories. This document aims to detail those elements and suggest corresponding tools and resources.

## Source code management
TTS projects use GitHub for source code management, with their repositories under a [government-owned organization](https://handbook.tts.gsa.gov/github/#organizations).

- [GitHub](https://handbook.tts.gsa.gov/github/) {% include "engineering/tag-standard.html" %}

## Repository checklist
Below is an aspirational list of configuration and files for a source code repository. Not all of these elements will apply to every project (e.g. visual regression tests don't make sense for an API).

1. [Community profile](https://help.github.com/en/github/building-a-strong-community/about-community-profiles-for-public-repositories)
   - [`LICENSE.md`](https://github.com/18F/open-source-policy/blob/master/LICENSE.md) {% include "engineering/tag-standard.html" %}
   - [`CONTRIBUTING.md`](https://github.com/18F/open-source-policy/blob/master/CONTRIBUTING.md) {% include "engineering/tag-standard.html" %}
   - [`README.md`](https://github.com/18F/open-source-policy/blob/master/README_TEMPLATE.md) {% include "engineering/tag-default.html" %}
1. [`.gitignore`](https://github.com/github/gitignore) (though also consider a [global config](https://help.github.com/articles/ignoring-files/#create-a-global-gitignore))
1. Programming language version specifications (e.g., `.nvmrc`, `runtime.txt`, `Gemfile`)
1. Dependency version descriptions (e.g., `Gemfile`, `Pipfile`, `package.json`) — don't forget to
   [pin](https://pages.18f.gov/before-you-ship/infrastructure/pinning-dependencies/)
   them
1. Build scripts
1. Unit test setup for each programming language
1. Linter setup (e.g., [`flake8`](http://flake8.pycqa.org/en/latest/),
   [`rubocop`]({{ "/assets/engineering/dist/rubocop.yml" | url }}),
   [`eslint`](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc) {% include "engineering/tag-suggestion.html" %})
1. Docker
   - Dockerfiles
   - [Compose](https://docs.docker.com/compose/) files
   - [.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
1. cloud.gov/Cloud Foundry
   - [Manifests](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html), one per deployment environment (e.g., [18F/fec-cms](https://github.com/18F/fec-cms), [18F/culper](https://github.com/18F/culper/tree/develop/conf/manifests))
   - [`.cfignore`](https://docs.cloudfoundry.org/devguide/deploy-apps/prepare-to-deploy.html#-ignore-unnecessary-files-when-pushing) (can be a symlink to `.gitignore` to get started)
1. Deploy scripts
1. [Continuous Integration/Continuous Deployment](#continuous-integration-continuous-deployment)
1. [Code coverage metrics](#code-coverage-metrics)
1. [Static analysis for code quality](#static-analysis-for-code-quality)
1. [Static security analysis](https://pages.18f.gov/before-you-ship/security/static-analysis/)
1. [Accessibility scanning](https://18f-guides.netlify.app/engineering/accessibility-scanning/) (e.g., [Pa11y](https://pa11y.org/) {% include "engineering/tag-default.html" %})
1. Integration test setup (e.g., [Selenium](https://www.selenium.dev/) {% include "engineering/tag-suggestion.html" %})
1. Visual regression setup (e.g., [Backstop](https://github.com/garris/BackstopJS) {% include "engineering/tag-suggestion.html" %})
1. [Infrastructure as Code](#infrastructure-as-code) (e.g., [Terraform](https://www.terraform.io/) {% include "engineering/tag-default.html" %})

## Branch protection {% include "engineering/tag-requirement.html" %}
Set up branch protection rules for each repository. It's a good practice to prevent mistakes like an accidental force-push to main.

We recommend at the very least enabling:
* Require pull request reviews before merging
* Restrict who can push to your main branch

Please refer to GitHub's [branch protection documentation](https://docs.github.com/en/github/administering-a-repository/managing-a-branch-protection-rule) to help determine what other configuration settings are best for your project.

By default, protected branch rules do not apply to people with admin permissions to a repository, allowing admins to merge PRs without an external review when necessary.

## Project management tool
Every project, no matter the size, should use a project management tool to keep track of ongoing tasks and to do items. The project management tool should be linked to somewhere in the project's GitHub repository so that others can find it easily.

Choose a tool that will work for you and your partner. Remember, they will be the ones using it once the engagement is over!

- [GitHub Issues](https://guides.github.com/features/issues/) {% include "engineering/tag-default.html" %}
- [Trello](https://trello.com/) {% include "engineering/tag-suggestion.html" %}
- [Jira](https://www.atlassian.com/software/jira) {% include "engineering/tag-suggestion.html" %}

## Continuous Integration/Continuous Deployment
Developers don't always remember to run the test suite. That's why we have Continuous Integration services to run the tests automatically after each commit. CI also helps ensure there's nothing specific to the developer's machine that makes the tests pass.

A Continuous Deployment service allows a development team to receive rapid feedback on new features or bug fixes. CD also helps ensure deployment and infrastructure issues are identified earlier in a release process, and are scoped to a smaller number of changes.

Pick a CI/CD service with a GitHub integration so that the build status can be seen for each pull request.

- [CircleCI](https://circleci.com/) {% include "engineering/tag-default.html" %}
- [GitHub Actions](https://github.com/features/actions) {% include "engineering/tag-suggestion.html" %}

## Code coverage metrics
Aim for more than 90% of your source code to be covered by tests; worry if coverage drops below 80%. For repositories with multiple programming languages/components (e.g., front and back ends), ensure that coverage reports are aggregated and reported on the entire project, in addition to reports on individual components.

- [Code Climate Quality](https://codeclimate.com/quality/) {% include "engineering/tag-suggestion.html" %}

## Static analysis for code quality
A good [code review process]({{ "/engineering/our-approach/code-review/" | url}}) is essential to writing good code. But certain code problems are difficult for humans to spot. Duplication, for example: if new code is an exact duplicate of code from an existing file in a project, that might not be caught in a code review. Static analysis tools catch duplication, security concerns, and more. Also see [Static Security Analysis](https://before-you-ship.18f.gov/security/static-analysis/).

- [CodeQL on GitHub](https://docs.github.com/en/code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/setting-up-code-scanning-for-a-repository) (for security analysis) {% include "engineering/tag-default.html" %}
- [Code Climate Quality](https://codeclimate.com/quality/) (for maintainability metrics) {% include "engineering/tag-suggestion.html" %}

## Dependency management
Applications require specific versions of programming languages, libraries, databases, services, and configuration to execute. A dependency management solution helps abstract that complexity and makes it easier for cross-functional teams to create consistent, reproducible, local development environments.

- [Docker](https://www.docker.com/why-docker) {% include "engineering/tag-suggestion.html" %}<br>
  See our [Docker for Development](../docker/) recommendations.

## Deployment infrastructure
Developers needing to deploy their code beyond their local environment should use either:

- a [cloud.gov sandbox](https://cloud.gov/docs/pricing/free-limited-sandbox/) {% include "engineering/tag-standard.html" %}
- an 18F-managed [AWS sandbox account](https://before-you-ship.18f.gov/infrastructure/sandbox/#aws-sandbox-accounts) {% include "engineering/tag-standard.html" %}
- or [Federalist](https://handbook.tts.gsa.gov/federalist/) (for static web sites) {% include "engineering/tag-standard.html" %}

{% include "engineering/tag-caution.html" %} The use of tools such as `localtunnel` and `ngrok`, which make your locally running services visible to the internet, are not allowed because they present a large security concern. Consult [`#infrastructure`](https://gsa-tts.slack.com/archives/C039MHHF8) on Slack for any questions.

## Infrastructure as Code

When developers are responsible for setting up or configuring infrastructure, it is best to record the steps or configuration as reproducible code.

A shell script is better than nothing and highly encouraged if infrastructure is simple. ([Example](https://github.com/18F/revampd/blob/develop/bin/setup-cloudgov)).

Many cloud providers like Azure or AWS provide APIs for controlling their resources. [Terraform](https://www.terraform.io/) {% include "engineering/tag-default.html" %} is a wrapper over these APIs. You can write source-controlled configuration files (or even let Terraform generate them from existing deployments) and then use Terraform to create, modify, or delete resources. ([Example](https://github.com/18F/dns)). This is particularly handy when you need multiple environments (dev, staging, prod, etc) that differ only in small details.

Be aware that you might encounter difficulty in managing user roles and permissions with Terraform and cloud.gov. See [this issue](https://github.com/GSA/data.gov/issues/1556) for context.

{% include "engineering/tag-caution.html" %} As of 2023, Terraform is no longer open source. A fork, [OpenTofu](https://opentofu.org/), exists, which is stewarded by the Linux Foundation. You are welcome to use OpenTofu if it works for you and your partner; it may eventually become the default at 18F.

Finally, although in many cases it is good to use containerized images, if it is necessary to do system configuration tasks on a physical server or a virtual machine, [Ansible](https://docs.ansible.com/) or [Chef](https://github.com/chef/chef) are tools to write down these tasks as playbooks or recipes and re-run them whenever needed.