# 🚀 DevOps Daily Learning Journey

Welcome to my **DevOps Learning Repository** — a living roadmap that documents my journey from absolute foundations to full-stack DevOps proficiency.  
Every `.md` file in this repo is designed to teach, clarify, and reinforce concepts — so anyone can learn alongside me, from **Linux basics to Kubernetes orchestration**.

---

## 🌍 Why This Repository Exists
Most people chase tools before they understand how systems actually work.  
This repository flips that mindset — learning from the **inside out**.

---

<details open>
<summary><strong>🐧 Linux & Bash</strong></summary>

Start with the OS basics — boot process, file system, permissions — and build up to writing shell scripts that automate tasks.

Includes:
- Boot process & file system  
- Shell navigation, permissions, and process management  
- Package management (`apt`, `yum`, `dnf`)  
- `systemd` & service management  
- Shell scripting fundamentals  
- Networking basics (DNS, TCP/IP, ports, `ping`, `traceroute`)  
- Monitoring tools (`top`, `htop`, `vmstat`, `netstat`)

**Goal:** Become fully comfortable in Linux CLI — your DevOps command center.

#### 🙏 Credits & References
- [**Udemy: The Linux Command Line for Beginners**](https://www.udemy.com/share/10cLbb3@rAHtXB-sYBJF-tmDzjkPKvPwn9nima7AgoYxGcRQxJnAVIuFDTAzlajAQ1GqjYDjGQ==/) – by **Jason Cannon** — for foundational Linux command structure and clear, beginner-friendly explanations.  
- [**W3Schools Linux Reference**](https://www.w3schools.io/terminal/) – for additional command syntax and quick reference lookups.  

*(All notes are rewritten, extended, and restructured for the Inside-Out Architecture learning framework by Akhil Teja Doosari.)*

</details>

---

<details>
<summary><strong>⚙️ Bash & Automation Scripts</strong></summary>

Move from typing commands to automating workflows.

Includes:
- Variables, loops, and conditionals  
- Functions & arguments  
- Environment variables  
- Cron jobs (task scheduling)  
- Log parsing with `awk`, `sed`, and redirection  
- Writing reusable DevOps helper scripts

**Goal:** Automate repetitive tasks and build confidence in scripting logic.

</details>

---

<details>
<summary><strong>🌿 Git & Version Control</strong></summary>

> From Local Control to Complete Recovery

This five-part Git learning sequence follows my **Inside-Out Architecture** system — designed to build mastery layer by layer, from solo control to full collaboration and recovery.

| Phase | File | Core Question | Focus |
|-------|------|----------------|-------|
| 1️⃣ Foundations | [01. Git Foundations](./Git/01.%20Git%20Foundations.md) | “How do I start and save work?” | Local setup, config, commit, push, pull. |
| 2️⃣ Work in Progress | [02. Git Stash & Tags](./Git/02.%20Git%20Stash%20&%20Tag.md) | “How do I pause and mark progress?” | Stash, tag, restore. |
| 3️⃣ Parallel Work | [03. Git History & Branching](./Git/03.%20Git%20History%20&%20Branching.md) | “How do I work on multiple versions safely?” | History, diff, branches, merges. |
| 4️⃣ Collaboration | [04. Git Contribute](./Git/04.%20Git%20Contribute.md) | “How do I collaborate and contribute?” | Forking, cloning, pull requests. |
| 5️⃣ Recovery | [05. Git Undo & Recovery](./Git/05.%20Git%20Undo%20&%20Recovery.md) | “What if something goes wrong?” | Revert, amend, reset, reflog. |

**Flow Summary**
```

Foundations → Stash → Branch → Contribute → Recover

```

**Mentor Insight**
> Git is more than version control — it’s time control.  
> Once you understand commits, branches, and recovery, you stop fearing mistakes and start experimenting freely.

**Credits**
- [W3Schools Git Tutorial](https://www.w3schools.com/git/)  
- [Hitesh Choudhary – ChaiCode Docs & Git Playlist](https://docs.chaicode.com/youtube/getting-started/)  
  🎥 [Watch Reference Lesson →](https://youtu.be/zTjRZNkhiEU?si=IL8kiu3lsCeZN0pA)

</details>

---

<details>
<summary><strong>☁️ AWS (Inside-Out Architect Approach)</strong></summary>

Learn AWS from the **inside out** — not by memorization, but by understanding how services actually connect.

**Phases**
1️⃣ IAM → Users, Roles, Policies  
2️⃣ VPC + Subnets → Network layout & security  
3️⃣ EBS, EFS, Snapshots → Block & shared storage  
4️⃣ S3 → Object storage & hosting  
5️⃣ EC2 → Virtual machines & compute  
6️⃣ RDS → Managed databases  
7️⃣ Load Balancer + Auto Scaling → Traffic & resilience  
8️⃣ CloudWatch, Lambda, Beanstalk → Monitoring & automation  
9️⃣ Route 53 + CloudFormation → DNS & Infrastructure as Code  

**Flow**
```

IAM → VPC → EBS → S3 → EC2 → RDS → Load Balancer → Auto Scaling → CloudWatch → Lambda → Route 53 → CloudFormation

```

**Goal:** Understand AWS as a living architecture, not a checklist of services.

</details>

---

<details>
<summary><strong>🐳 Containers & Orchestration</strong></summary>

Docker and Kubernetes — build, ship, and run applications anywhere.

Includes:
- Docker: Images, containers, volumes, networking, Dockerfiles  
- Kubernetes: Pods, Deployments, Services, ReplicaSets, Ingress, ConfigMaps  
- Helm basics: Simplify deployment management  

**Goal:** Learn containerization and orchestration to scale modern apps.

</details>

---

<details>
<summary><strong>🧱 Infrastructure as Code (IaC)</strong></summary>

Define and deploy infrastructure through code using Terraform and Ansible.

Includes:
- Terraform: Providers, state files, variables, modules  
- Ansible: Playbooks, inventory, roles, YAML syntax  
- AWS Integration: Deploy EC2, S3, networking  
- CI/CD pipelines for IaC validation & deployment  

**Goal:** Manage servers, networks, and apps declaratively — not manually.

</details>

---

<details>
<summary><strong>🔁 CI/CD & Automation</strong></summary>

Integrate everything together for smooth deployment pipelines.

Includes:
- Jenkins, GitHub Actions, GitLab CI  
- Build → Test → Deploy workflows  
- Integrating Docker, Terraform, AWS  
- Blue-Green & Canary deployments  

**Goal:** Achieve automated, repeatable, and reliable software delivery.

</details>

---

<details>
<summary><strong>🔍 Observability & Maintenance</strong></summary>

Monitor, log, and alert for proactive infrastructure management.

Includes:
- CloudWatch, Prometheus, Grafana  
- Log aggregation and alerting  
- Incident response strategies  

**Goal:** Build visibility into every layer of your stack.

</details>

---

## 🎯 What This Roadmap Offers
- 🧠 Conceptual clarity — from fundamentals to architecture  
- 🧩 Real-world analogies and examples  
- 💻 Hands-on labs for every major topic  
- 🗂️ Organized Markdown notes for easy review  
- 📚 Ideal reference for interviews & project building

---

## 🤝 Join the Journey
⭐ **Star this repo** to support the project  
🍴 **Fork it** to start your own journey  
💬 **Share insights or corrections**

📧 **Email:** [doosariakhilteja@gmail.com](mailto:doosariakhilteja@gmail.com)  
🔗 **LinkedIn:** [linkedin.com/in/akhiltejadoosari2001](https://linkedin.com/in/akhiltejadoosari2001)  
💻 **GitHub:** [github.com/AkhilTejaDoosari](https://github.com/AkhilTejaDoosari)

---

> “Certifications prove what you studied.  
> **This roadmap proves what you understand.**”

---

### 🤖 Special Acknowledgment

> This entire DevOps Learning Journal was structured, refined, and mentored with guidance from **ChatGPT (OpenAI GPT-5)** — assisting in architecture design, concept organization, and educational clarity across all modules under the Inside-Out Learning System.