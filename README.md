


# Living Brain: Professional Case Study

**Company:** Living Brain GmbH | **Role:** Unity Developer -> Senior Software Engineer -> Head of Development | **Timeframe:** 2020-2026 | **Note:** proprietary/confidential codebase, not included. This is a writeup of the work, not the code itself

I joined Living Brain GmbH, a small medical VR company, in 2020 and spent six years there, progressing from a programmer role to Head of Development. This writeup covers what I actually built and what changed about my work as the role grew.

## What Living Brain builds

https://github.com/user-attachments/assets/175d2977-611f-4e7c-892f-c5bb15140f57

Living Brain's main product, teora mind, is designed to help people with neurological conditions train memory, attention, and concentration. Instead of relying on abstract cognitive exercises, teora mind uses immersive virtual environments to recreate familiar real-world situations - shopping for groceries, cooking in a kitchen, pouring a cup of coffee - in a setting where a mistake costs nothing: no burns, no broken dishes, no real consequences, just another try.

It is a VR-based cognitive training application used in therapeutic settings, both in-clinic during a supervised session and at home for aftercare, and is classified as a Class IIa medical device in Germany. Patients wear a VR headset and work through therapy exercises, while an optional tablet companion app lets a therapist watch the session in real time and step in to guide the patient when needed. Three related products grew out of it: teora body (a hand-tracking variant for upper-extremity rehabilitation), teora platform (the clinician-facing web backend where patients' training plans get managed and results reviewed), and teora supervisor (a tablet app that lets a doctor or family member watch a patient's session live).

## Tech stack

Unity / C# (teora mind, teora body). Android / Java (the device-interface library, teora supervisor). WebRTC / Unity Render Streaming (teora supervisor's live view). Plastic SCM, GitHub, Jira, Confluence, Scrum.

teora platform itself runs on Java 17 / Spring Boot microservices, Keycloak, PostgreSQL with Liquibase, and a React/TypeScript frontend. Listed here for completeness: I only worked with this stack superficially - it was built and maintained primarily by others, with outside technical support brought in as needed (my own involvement is described under [teora platform](#teora-platform)).

## My role

I joined the team in January 2020 as an intern. Over the next six years I moved through several roles: Junior Unity Developer, Unity Developer, Senior Software Engineer, and Head of Development from April 2025. My responsibilities grew with each step. I started as a regular programmer, the first one hired next to my boss. The longer I was there, the more my responsibility naturally grew: I began making design decisions myself, then took ownership of the codebase, and later took on responsibility for the team.

Content-wise, I was involved in all four of Living Brain's products and did the usual programming work: implementing features, designing systems, and fixing bugs. Because the team was small, I also regularly took on side tasks outside my main domain that still had to be done, such as regulatory work, support, and device management for the VR headsets and tablets. The sections below cover [each product](#teora-mind), the [regulatory work](#regulatory-and-compliance-work), and [leading the team](#leading-a-small-experienced-team).

### teora mind
**Longest involvement, roughly half the codebase.** Living Brain's flagship product, and the one I was involved with the longest - close to its start, and continuously through all six years. I estimate I personally implemented roughly half of its codebase, and was conceptually involved in nearly all of it, either as the one who designed a system or the one who reviewed and understood it afterward.

Around 2022-2024, as my manager's attention shifted more toward regulatory work, I proactively took on more responsibility for the codebase. This meant making more of the technical decisions myself, extending the team's existing coding conventions, and starting to plan further ahead. That responsibility was later formalized in the title Code Lead, awarded before Head of Development and distinct from it - Code Lead was specifically about owning the codebase, while Head of Development (see [Leading a small, experienced team](#leading-a-small-experienced-team), below) was about the team and process more broadly. Taking on that kind of ownership changed how I see the code, in much the same way leading the team later changed how I see the product.

### teora body

**A hand-tracking fork of teora mind.** It applies the same VR training approach to the rehabilitation of the upper extremity joints, such as shoulders and elbows. We saw a market for it, so we did it in between work on teora mind. My role was the same as on teora mind: planning, implementation, and design decisions. On top of that, our Head of Development was already preparing to leave the company and was largely not involved. So this was the first project where we made most of the decisions ourselves.

### teora platform
**Limited involvement, mostly coordination.** The platform was built and maintained primarily by others (see [Tech stack](#tech-stack)). I was involved in two ways. First, I prepared the data in Unity and sent it to the platform via REST calls. Second, in 2026 customer feedback led to new changes for the platform, and I supervised a short, focused effort with an external team to implement them.

### teora supervisor
**Built the connection layer end to end.** The tablet app that lets a doctor or family member watch a patient's teora mind session live. I built its custom handshake protocol (see [Technical highlights](#technical-highlights)) and the UI around it, implemented on both sides of the connection - Android Studio on the tablet, Unity on the headset. The initial setup and the streaming itself were built by others.

### teora device interface library
**Entirely solo, start to finish.** The training itself runs in Unity, but a VR headset in the field needs more than that. It has to log in to the platform, download the training plans that clinicians assign, and upload the results afterwards, even when the connection is unreliable. It also has to enforce time-limited licenses and protect the patient data on the device against tampering. None of this belongs in the game code, so I built it as a separate Android library that connects teora mind to the platform. The device-identity work described under [Technical highlights](#technical-highlights) is part of this library.

### Regulatory and compliance work

**Maintained an existing system, not designed from scratch.** teora mind's status as a medical device meant maintaining a quality management system (QMS) and technical documentation (TD). When I became Head of Development, this work became part of my role. Neither the QMS nor the TD was something I built. Both existed before my involvement. I maintained and extended them instead of designing them from scratch.

Later I handed off day-to-day QMS ownership to a coworker. I kept the TD, since it covers the development side of the product. My work on the TD was regular maintenance for the most part. In practice, I brought it up to date with the changes in the app for each new release. This included things like revising the specifications of changed features, documenting every change with an assessment of its risk impact, and updating the tests. Also, as Head of Development, I signed off the complete TD and with it the release.

Another important part of my responsibility was the cybersecurity of all our apps. I ran recurring screenings against public vulnerability databases, and I read security news regularly to spot major vulnerabilities early. For example, when React2Shell was reported, I wrote a dedicated assessment of whether we were affected and added it to the TD. The assessment ruled out any exposure and documented the verification steps, not just the conclusion.

## Leading a small, experienced team

After the previous Head of Development left the company, I took over the role in 2025 as the most suitable successor from within the team. This changed less about day-to-day technical decisions than it changed about process and responsibility. I had already been the one setting architectural direction informally for years. What was new was owning the outcome. Until then, my day-to-day work ended with the implementation. From then on, I was involved in all the steps before it and had to make many decisions about the future of the products, instead of only the technical side. That changed how I look at the products and at development as a whole, and with it how I approach my work.

Since the team was small and experienced, I kept the way of working that already existed. The roles had already been established and did not need to change. So I concentrated on making sure that whoever worked on a task knew exactly what the result had to be. For that I wrote design documents before implementing bigger things. They included things like the goal, the requirements, the expected effort, and a priority. Where it mattered, they also covered risks, alternative solutions, and open questions.

I learned fairly quickly that there are many things to consider when deciding what to work on. This includes things like planning for the next five years instead of only the next one, keeping our existing customers in mind, taking the regulatory effort of a change into account, and protecting what already works for patients and therapists. I applied these points, for example, when new content for a new market was about 80 percent done. Feedback from our existing customers showed that the app itself still had UX problems, and new customers would have run into the same ones. So I argued for pausing the new content until the UX problems were fixed, and the team agreed.

## Technical highlights

The five pieces below aren't necessarily my single biggest or most impressive pieces of work - they're meant to give a reasonably representative picture of the kinds of problems I ended up solving.

### A handshake protocol for a secure, patient-protecting connection
teora supervisor lets a doctor watch a patient's VR session live on a tablet, so the pairing protocol connecting tablet and headset had to handle the connection reliably without either side needing technical know-how to recover it. The doctor picks a headset from a list on the tablet. The patient then accepts the request in the headset after checking a code the doctor tells them. The protocol handles the rest. It includes a heartbeat timeout to catch a dropped WiFi connection and a same-device filter to prevent cross-talk between headsets, and had to be implemented twice, independently, in Java on the tablet and C#/Unity on the headset, with the exact same message format kept in sync by hand across both. Being trusted with the one piece of the system that directly touches a live patient session is the kind of responsibility that stuck with me.

### Making a device's identity un-copyable
Each VR headset's identity was stored in a local file that could simply be copied onto another headset to fake its training history. I fixed this by binding that file to a hardware-backed key from Android's Keystore, written into the file once at setup - checking authenticity from then on is a purely local, on-device comparison against that key, no server round-trip needed. A small example of closing a real security gap, not just a theoretical one.

### A configurable, general-purpose object pooler
A general-purpose, fully generic object pool for VR performance-critical reuse (sound effects, projectiles, UI popups): minimal API, auto-creates a pool on first use, and supports per-object configuration plus lifecycle hooks for extending behavior without touching the pool itself. Small, clean, and still held up unchanged years later - the kind of generic, reusable infrastructure work I kept gravitating toward.

### Using the Elo system to recommend the right difficulty
Patient difficulty recommendations needed to self-adjust from feedback, without hand-tuned rules for every case, so I borrowed the Elo rating algorithm chess uses: exercise difficulty and patient rating play the two competitors, feedback becomes the match outcome. A handful of lines of code replaced what could have been a sprawling set of hand-tuned rules - reaching for an existing, proven idea from a completely different domain instead of building something bespoke.

### A reflection-based tool for safe bulk refactors
Adding Arabic support meant swapping every TextMeshPro component project-wide for a right-to-left-capable one, without breaking the many other references pointing at them. I wrote a reflection-based editor tool that found, tagged, swapped, and automatically reapplied every reference, turning a risky multi-week manual migration into an automated run that took about four hours. Building a background tool to solve a slow, risky problem properly, instead of grinding through it by hand, is something I ended up doing more than once.

---

See the rest of my portfolio for personal projects built outside of Living Brain.
