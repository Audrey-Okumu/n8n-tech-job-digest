# n8n-tech-job-digest
Daily email of remote tech jobs ranked by AI fit score. Built with n8n, Gemini and Gmail, pulling from Remotive , RemoteOK and We Work Remotely with filtering and duplicate protection.

# Tech Job Digest (n8n + Gemini)

A daily email of remote tech jobs that match my skills, ranked by an AI fit score.

![Workflow](images/workflow.png)
![Sample email](images/email.png)

## The problem
Checking several job boards every day is slow, and most listings don't fit a junior Android/Kotlin/Python developer.

## How it works
1. A schedule trigger runs every day at 7:00 AM (Africa/Nairobi).
2. It pulls jobs from Remotive and We Work Remotely and merges them.
3. Keyword filters keep relevant roles and drop senior ones.
4. Remove Duplicates blocks jobs I've already been sent.
5. Gemini scores each job from 1 to 10 against my skills, in one batched request.
6. A Code node drops low scores, sorts best first, and Gmail sends the digest.

## What broke and how I fixed it
- **Rate limit (429):** scoring one job per request exceeded Gemini's free tier of 5 requests per minute. I fixed it by sending all jobs in a single prompt.
- **Disconnected node:** the Filter showed "No input connected" and processed nothing. I reconnected it to the previous node.
- **Deactivated node:** a deactivated Filter silently passed all 13 jobs through instead of 3. I reactivated it and checked the item counts at every step.
- **Duplicates:** the same jobs would be emailed daily. I added Remove Duplicates, then tested it by running twice and confirming the second run sent nothing.
- **Retired model (404):** the model I picked was no longer available, so I switched to the current Gemini Flash model.

## Stack
n8n Cloud, Gemini API, Gmail, Remotive API, We Work Remotely RSS

## Possible next steps
RemoteOK as a third source, and an error alert email if a run fails.
