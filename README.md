# Living Brain: Professional Case Study

**Company:** Living Brain GmbH | **Role:** Unity Developer -> Senior Software Engineer -> Head of Development | **Timeframe:** 2020-2026 | **Note:** proprietary/confidential codebase, not included; this is a writeup of the work, not the code itself

I spent six years at Living Brain GmbH, a small medical VR company, growing from an intern into Head of Development. This writeup covers what I actually built and what changed about my work as the role grew.

## What Living Brain builds

Living Brain's main product, teora mind, is a VR-based cognitive training application used in therapeutic settings, both in-clinic during a supervised session and at home for aftercare, with a doctor able to follow a patient's progress either way. It is classified as a Class IIa medical device in Germany. Patients wear a VR headset and work through therapy exercises; an optional tablet companion app lets a therapist watch the session in real time and step in to guide the patient when needed. Three related products grew out of it: teora body (a hand-tracking variant for upper-extremity rehabilitation), teora platform (the clinician-facing web backend where training plans get managed and results reviewed), and teora supervisor (a tablet app that lets a doctor or family member watch a patient's session live).

## Tech stack

Unity / C# (teora mind, teora body); Android / Java (the device-interface library, teora supervisor); WebRTC / Unity Render Streaming (teora supervisor's live view); Plastic SCM, Jira, Confluence.

teora platform itself runs on Java 17 / Spring Boot microservices, Keycloak, PostgreSQL with Liquibase, and a React/TypeScript frontend. Listed here for completeness: I only worked with this stack superficially - it was built and is maintained primarily by others, with outside technical support brought in as needed.

## My role

I joined in January 2020 as an intern and stayed through several role changes: Junior Unity Developer, Unity Developer, Senior Software Engineer, and finally Head of Development from April 2025. As the company's programming team never grew much past three people, my day-to-day work spanned nearly everything - across all of Living Brain's products, plus regulatory work and, eventually, leading the team (both covered below).

### teora mind
Living Brain's flagship product, and the one I was involved with the longest - close to its start, and continuously through all six years. I estimate I personally implemented 40-50% of its codebase, and was conceptually involved in nearly all of it, either as the one who designed a system or the one who reviewed and understood it afterward.

### teora body
A hand-tracking fork of teora mind for upper-extremity training, built over about six to eight months by a small team of two programmers and two artists. My role matched my usual one on teora mind - planning, implementation, and design decisions - plus a recurring extra task: making movement-based interactions actually feel good, like a rocket's flight or a navigation device with realistic weight and drag.

### teora platform
My own hands-on involvement was limited: coordinating the protocol between it and teora mind early on, then in 2026, supervising a short focused effort with an external team to close out a paused batch of pending changes. The platform itself was built and is maintained primarily by others (see Tech stack).

### teora supervisor
The tablet app that lets a doctor or family member watch a patient's teora mind session live. I built its custom handshake protocol (see Technical Highlights) and the UI around it, implemented on both sides of the connection - Android Studio on the tablet, Unity on the headset. The initial setup and the streaming itself were built by others.

### teora device interface library
An Android library connecting the VR headset to the cloud backend - entirely my own project, planned and built solo. It's also where the device-identity work described under Technical Highlights lives.

### Regulatory and compliance work

teora mind's status as a medical device meant maintaining a quality management system (QMS) and technical documentation (TD). Neither of these was something I built; both existed before my involvement, and my role was maintaining and extending them, not designing them from scratch.

I took on a larger share of regulatory work once I became Head of Development, helping turn it into a properly shared team effort. I've since handed off day-to-day QMS ownership, keeping the TD myself.

On the TD side, I ran regularly recurring cybersecurity screenings against public vulnerability databases and wrote up concrete CVE impact assessments (for example, ruling out exposure to a widely-reported React vulnerability and a PostgreSQL CVE), with the actual verification steps documented, not just a conclusion.

## Leading a small, experienced team

Becoming Head of Development in 2025 changed less about day-to-day technical decisions than it changed about process and responsibility. I had already been the one setting architectural direction informally for years; what was new was owning the outcome, including on the strategic side: not just how something got built, but whether it should get built at all. I wrote design documents ahead of implementation for anything non-trivial (problem, options, risks, a rough plan), a habit that consistently surfaced inconsistencies the team hadn't caught in conversation alone.

A recurring part of that shift was prioritizing based on direct, concrete feedback over what was already underway or seemed technically interesting on its own - including being willing to pause work in progress once a clearer priority surfaced.

## Technical highlights

### A handshake protocol for a secure, patient-protecting connection
teora supervisor lets a doctor watch a patient's VR session live on a tablet, so the pairing protocol connecting tablet and headset had to handle the connection reliably without either side needing technical know-how to recover it. It includes a heartbeat timeout to catch a dropped WiFi connection and a same-device filter to prevent cross-talk between headsets, and had to be implemented twice, independently, in Java on the tablet and C#/Unity on the headset, with the exact same message format kept in sync by hand across both.

### Making a device's identity un-copyable
Each VR headset's identity was stored in a local file that could simply be copied onto another headset to fake its training history. I fixed this by binding that file to a hardware-backed key from Android's Keystore, written into the file once at setup - checking authenticity from then on is a purely local, on-device comparison against that key, no server round-trip needed.

### A configurable, general-purpose object pooler
A general-purpose, fully generic object pool for VR performance-critical reuse (sound effects, projectiles, UI popups): minimal API, auto-creates a pool on first use, and supports per-object configuration plus lifecycle hooks for extending behavior without touching the pool itself. Small, clean, and still held up unchanged years later.

### Using the Elo system to recommend the right difficulty
Patient difficulty recommendations needed to self-adjust from feedback, without hand-tuned rules for every case, so I borrowed the Elo rating algorithm chess uses: exercise difficulty and patient rating play the two competitors, feedback becomes the match outcome. A handful of lines of code replaced what could have been a sprawling set of hand-tuned rules.

### A reflection-based tool for safe bulk refactors
Adding Arabic support meant swapping every TextMeshPro component project-wide for a right-to-left-capable one, without breaking the many other references pointing at them. I wrote a reflection-based editor tool that found, tagged, swapped, and automatically reapplied every reference, turning a risky multi-week manual migration into an automated run that took about four hours.

---

See the rest of my portfolio for personal projects built outside of Living Brain.
