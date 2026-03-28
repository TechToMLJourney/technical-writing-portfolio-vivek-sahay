# AI-Assisted Release Notes Generator

## Overview

Creating release notes is often a manual and repetitive task. Technical writers need to review multiple Jira issues, identify user-facing changes, group them, and rewrite them in a consistent format.

This project demonstrates a prompt-driven approach to automate release note generation using AI while maintaining quality through structured inputs and review.

---

## Problem Statement

Manual release note creation:
- Takes significant time  
- Can miss important updates  
- Depends on how well Jira tickets are written  
- Lacks consistency across releases  

---

## Solution

This project uses AI to generate structured, customer-facing release notes from Jira issues.

### Workflow

1. Extract closed Jira issues labeled **"user-facing"**  
2. Prepare structured input (title, description, labels, version)  
3. Use a controlled prompt to generate release notes  
4. Review and refine the output before publishing  

---

## Prompt Design
You are a technical writer generating customer-facing release notes for an enterprise SaaS application.

Context:
The input consists of Jira issues marked as "user-facing" and closed for a specific release version.

Objective:
Generate clear and structured release notes in Markdown format.

Guidelines:

Use concise and user-focused language
Avoid internal or technical implementation details
Focus on what has changed and how it impacts users
Maintain consistent terminology

Input:
A list of Jira issues with:

Title
Description
Labels (Feature, Enhancement, Bug Fix)
Fix version

Output:

Markdown format
Group by change type
Include release version
Convert issues into user-friendly summaries
