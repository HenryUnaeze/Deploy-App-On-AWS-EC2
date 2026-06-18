# Deploy-App-On-AWS-EC2

A complete beginner's documented journey deploying a live Node.js + Stripe payment app to a real AWS EC2 instance — including every mistake made along the way.

## What This Is

A real Node.js/Stripe payment application deployed to a public-facing AWS EC2 server (not a localhost demo). This repo/post documents the full process honestly, mistakes included, for other beginners learning DevOps.

**Credits:**
- Course: [DevOps Zero to Hero](https://www.youtube.com/@AbhishekVeeramalla) by Abhishek Veeramalla
- Starter code: [Kunal Verma](https://github.com/verma-kunal/AWS-Session)

## Tools Used

AWS EC2 · Node.js · Express · Stripe · PM2 · Git · WSL2 · Ubuntu

## Project Structure

```
Deploy-App-On-AWS-EC2/
├── public/              # Static frontend assets
├── views/               # App views/templates
├── server.js            # Express app entry point
├── package.json         # Node dependencies & scripts
├── Dockerfile           # Container build definition
├── .env                 # Stripe keys (not committed)
├── .gitignore           # Excludes .env, *.pem, node_modules
└── README.md
```

> Note: structure inferred from the tools/stack described (Node.js + Express + Stripe + Dockerfile). Adjust to match your actual repo layout.

## Key Lessons Learned

1. **Know which terminal you're in.** Laptop and server prompts look similar — always check before running a command.
2. **One command, one job.** `chmod` and `ssh` are separate commands; never combine them on one line.
   ```bash
   chmod 400 ~/first_server.pem
   ```
3. **Never commit secrets.** API keys belong in a `.env` file, excluded via `.gitignore` *before* the first commit — not after.
   ```bash
   echo ".env" >> .gitignore
   echo "*.pem" >> .gitignore
   ```
4. **Dependencies don't travel with your code.** Always run `npm install` fresh on any new machine.

## What's Next

This is post 1 of a "DevOps Journey in Public" series. Coming up:
- Dockerizing the app
- CI/CD with GitHub Actions
- Kubernetes orchestration

## Author

**Henry Unaeze** — Junior DevOps Engineer in training
📍 Lincoln, England
GitHub: [github.com/HenryUnaeze](https://github.com/HenryUnaeze)
