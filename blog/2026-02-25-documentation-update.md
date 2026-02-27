---
slug: documentation-update
title: Documentation Update
authors: thane
---

![screenshot of our documentation site](/img/documentation-capture.png)

The Ocean Pi Learning Platform (learn.oceanpi.org) was built to transform our internal technical knowledge into a structured, scalable, and publicly accessible education system. What began as scattered Google Docs has evolved into a modern documentation and publishing platform designed to support students, collaborators, and future contributors to the Ocean Pi ecosystem.

Below is a summary of the major milestones in bringing learn.oceanpi.org online.

<!-- truncate -->

## From Google Docs to Structured Knowledge

Ocean Pi tutorials originally lived inside our “You Need A Wiki” Google Doc system — effective for drafting, but not scalable for long-term technical documentation.

We migrated that content into Docusaurus, restructuring tutorials into version-controlled Markdown files, organizing them into a coherent documentation architecture, and standardizing formatting for readability and future expansion. This transition marked a shift from static documents to a true learning platform.


## Building the Documentation Engine with Docusaurus

We initialized a dedicated Docusaurus project, configured the site structure, navigation, and metadata, and established a clean Git-based workflow for long-term maintainability.

The site now supports:
	•	Structured technical documentation
	•	Modular tutorial pathways
	•	Version control and change tracking
	•	Clean local development with hot-reload for rapid iteration

This created the foundation for Ocean Pi’s long-term knowledge infrastructure.


## Deployment Pipeline & Vercel Integration

To ensure reliability and scalability, the site was deployed to Vercel, enabling:
	•	Automatic builds on every GitHub push
	•	Global CDN distribution
	•	Continuous deployment
	•	Zero-downtime updates

The custom subdomain learn.oceanpi.org was configured and pointed to Vercel, completing the transition from local prototype to production-ready educational platform.

This ensures that every documentation improvement is instantly reflected in the live environment.


## Blog Activation

Beyond tutorials, we activated and configured the Docusaurus blog system to support ongoing technical updates, build logs, and Ocean Pi development notes.

We:
	•	Configured blog metadata and authors
	•	Cleaned up routing and author behavior
	•	Updated blog content structure
	•	Established a publishing workflow aligned with Git-based version control

The blog now serves as a public-facing development journal — documenting the evolution of Ocean Pi in real time.


## Clean Multi-Machine Workflow & Repository Realignment

As development expanded across multiple machines, we refined the Git workflow to ensure stability and clarity:
	•	Resolved remote history conflicts
	•	Standardized fetch/rebase practices
	•	Re-cloned the repository after renaming to maintain alignment
	•	Eliminated risky iCloud-based repo syncing in favor of GitHub as the single source of truth

This ensured long-term integrity of the codebase and deployment pipeline.

## Current Status

The Ocean Pi Learning Platform is now:
	•	Fully live at learn.oceanpi.org
	•	Deployed via Vercel with continuous integration
	•	Structured for scalable curriculum expansion
	•	Migrated off Google Docs into version-controlled documentation
	•	Equipped with a live blog to document ongoing system development

This platform represents a foundational step toward Ocean Pi becoming not just a system, but an open, living body of technical knowledge.