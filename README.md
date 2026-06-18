# Deploy-App-On-AWS-EC2

*How a complete beginner went from terrified of the terminal to running a live payment app on AWS EC2 — mistakes, panic, and all*

---

> *"The fastest way to learn DevOps is to break things in production and survive it."*
> — Every senior DevOps engineer, ever

I did not plan to learn that lesson on day one.

But here we are.

---

## The Moment That Changed Everything

I want to take you back to a bootcamp classroom. Data Engineering. A instructor says the words nobody warned me about:

*"Open your terminal."*

My stomach dropped.

I had avoided the terminal my entire life. It felt like hacking. Like one wrong keystroke would delete everything. Like it was built for people smarter than me.

Then we typed a single command to spin up Apache Airflow — an entire data pipeline tool — and it just... worked.

I sat back in my chair and thought: *what else can this thing do?*

That was the moment. That was when DevOps stopped being something other people did and became something I desperately wanted to understand.

And if I am being completely honest with you — I also hate repetitive tasks with a passion. The idea that I could automate the boring stuff and never do it manually again? That felt like a superpower made specifically for lazy people like me 😂

So here I am. Documenting my very first DevOps project publicly — every command, every error, every moment of panic — so you do not have to figure it out alone.

---

## What I Actually Built

A live Node.js payment application using Stripe, deployed to a real AWS EC2 server on the internet.

Not a tutorial app running on localhost that disappears when you close the laptop.

A real server. A real IP address. A real website anyone in the world could visit.

*📸 [Screenshot: AWS EC2 console showing the instance with green "running" status]*

Before I go any further — full transparency. This project followed the **DevOps Zero to Hero** course by **Abhishek Veeramalla** on YouTube, using starter code by **Kunal Verma**. All credit to them for the foundation. What I am documenting here is not the code — it is the chaos of deploying it as a complete beginner. That part? Entirely mine.

---

## The Part Nobody Warns You About

Every YouTube tutorial shows you the happy path.

The commands work. The server starts. The website appears. The creator smiles at the camera.

Nobody shows you what happens when you accidentally add the SSH server address to your file permission command. Nobody shows you the moment you realise your secret API keys are now visible on GitHub to potentially anyone in the world.

I am going to show you all of that.

---

## Mistake #1 — I Could Not Tell Where I Was

> *I was typing commands on the wrong machine for twenty minutes before I realised.*

When you work with cloud servers, you have two terminals open that look almost identical:

```
henry_unaeze@LAPTOP-HLAU5O8V   ← my laptop
ubuntu@ip-172-31-42-166        ← my AWS server in the cloud
```

I kept running laptop commands on the server and server commands on the laptop. Nothing worked. I had no idea why. I felt genuinely stupid.

*📸 [Screenshot: Terminal showing both prompts]*

The fix was embarrassingly simple — read the prompt before every single command.

**What I learned:** Your terminal prompt is your compass. Never run a command without checking it first.

---

## Mistake #2 — SSH Refused to Let Me In

My first attempt to connect to my server was met with this:

```
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permissions 0744 for 'first_server.pem' are too open.
Permission denied (publickey).
```

SSH is paranoid about security — rightly so. It refused to use my key because other users on my machine could theoretically read it. The fix was one command:

```bash
chmod 400 ~/first_server.pem
```

It took me twenty minutes to figure this out because I kept writing the command like this:

```bash
chmod 400 first_server.pem ubuntu@myserver.com
```

I was mixing two completely separate commands into one. chmod changes file permissions. ssh connects to servers. They should never share a line.

**What I learned:** In the terminal, every command does exactly one job. Never combine unrelated commands on the same line.

---

## Mistake #3 — The One That Made My Heart Race

> *I pushed my Stripe secret keys to GitHub. GitHub caught it. My hands went cold.*

This is the mistake I am most embarrassed about — and the one I learned the most from.

I had hardcoded my Stripe API keys directly inside my Dockerfile:

```dockerfile
ENV SECRET_KEY="sk_test_51L5As..."
ENV PUBLISHABLE_KEY="pk_test_51L5As..."
```

I committed the file. I pushed to GitHub. GitHub's secret scanning caught it instantly and blocked the push — but the keys had already been exposed in my commit history.

What followed was the most stressful hour of this entire project:

- Immediately rotated both keys on the Stripe dashboard
- Rewrote my entire Git history using git filter-repo
- Deleted the repository and started completely fresh
- Created a proper .env file and added it to .gitignore

```bash
# What I should have done from the very beginning
echo ".env" >> .gitignore
echo "*.pem" >> .gitignore
```

**What I learned:** Secrets never belong in code. Not even test keys. Use a .env file. Add it to .gitignore before your very first commit. Not after.

---

## Mistake #4 — My App Refused to Start on the Server

I had installed everything perfectly on my laptop. I copied the project to the server. I ran the app.

```
Error: Cannot find module 'express'
```

The server had no idea what express was. Because node_modules — the folder containing all installed packages — does not travel between machines. It is operating system specific and can be over 200MB in size.

The fix:

```bash
npm install
```

Run this on every new machine. Every time. No exceptions.

**What I learned:** Your code travels. Your dependencies do not. npm install is always the first thing you run on a new machine.

---

## The Moment It All Clicked

After hours of debugging across two machines, three terminals, and more error messages than I can count — I typed this into my browser:

```
http://13.51.163.1:3000
```

And my application loaded.

A real website. On a real server. In the real internet.

*📸 [Screenshot: Terminal showing PM2 status with stripe-app "online"]*

*📸 [Screenshot: The live Stripe payment website loading in the browser]*

I genuinely sat back and did nothing for about thirty seconds. Just stared at it.

Nobody tells you about that feeling. The quiet satisfaction of something working that was completely broken an hour ago.

---

## The Bigger Picture

Here is what I understand now that I did not understand at the start of today:

DevOps is not a set of commands to memorise. It is a way of thinking about systems — how pieces talk to each other, where things can break, how to make processes repeatable and automatic so humans do not have to do them manually every time.

That last part is what drew me here from the beginning. Automation. The idea that good engineers are not the ones who work the hardest — they are the ones who build systems that work hard for them.

I am nowhere near that yet. But I took a real step today.

---

## What Is Coming Next

This is the first post in a series I am calling **My DevOps Journey in Public**. Next up:

- **Dockerizing this application** — packaging it into a container that runs anywhere
- **CI/CD with GitHub Actions** — automating deployment so pushing code triggers everything automatically
- **Kubernetes** — orchestrating containers at scale

Every post will be this honest. Every mistake will be documented. Every error message will be shown in full.

If you are a beginner, follow along. If you are experienced, I welcome your feedback in the comments — this is how I learn.

---

## Credits & Resources

Everything I built today stood on the work of people who shared their knowledge freely:

- 🎓 **Course:** DevOps Zero to Hero by **Abhishek Veeramalla** — [YouTube Channel](https://www.youtube.com/@AbhishekVeeramalla)
- 💻 **Original Project Code:** **Kunal Verma** — [GitHub](https://github.com/verma-kunal/AWS-Session)
- 🛠️ **Tools Used:** AWS EC2, Node.js, Express, Stripe, PM2, Git, WSL2, Ubuntu

Thank you to both of them for making world class DevOps education accessible to complete beginners like me.

---

*Henry Unaeze — Junior DevOps Engineer in training*
*📍 Lincoln, England*
*Connect with me on LinkedIn | GitHub: github.com/HenryUnaeze*

---

*If this article helped you or made you feel less alone in your learning journey — leave a clap. It genuinely keeps me writing. 👏*
