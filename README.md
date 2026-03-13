# SRE & Incident Response - GCP Guide

Welcome to the **SRE & Incident Response - GCP Guide**!  

This guide provides a practical, hands-on approach to setting up a Next.js application, deploying it on **Google Cloud Platform (GCP)** using **Cloud Run**, and simulating a backend failure to demonstrate the core **Incident Response lifecycle** in a **Site Reliability Engineering (SRE)** context.  

---

## Introduction

**Site Reliability Engineering (SRE)** focuses on ensuring the reliability, scalability, and availability of systems in production environments.  
**Incident Response** is a structured process to detect, investigate, mitigate, recover from, and learn from failures to minimize downtime and impact on users.  

By following this guide, you will:  
- Deploy a Next.js app to GCP Cloud Run  
- Simulate backend failures  
- Apply the SRE Incident Response framework: **Detect → Investigate → Mitigate → Recover → Learn**  

---

## Steps

### 1. Create Next.js Project
```bash
npx create-next-app@latest sre
```
*Initialize a new Next.js application named `sre`.*

```bash
cd sre
```
*Navigate into the project directory.*

```bash
npm run dev
```
*Run the development server locally to ensure everything works.*

---

### 2. Create Dockerfile
```bash
FROM node:22-alpine
```
*Use Node.js 22 Alpine as the base image for lightweight container deployment.*

```bash
WORKDIR /app
```
*Set working directory inside the container.*

```bash
COPY package*.json ./
```
*Copy package.json and package-lock.json to the container.*

```bash
RUN npm install
```
*Install all project dependencies inside the container.*

```bash
COPY . .
```
*Copy the entire project into the container.*

```bash
RUN npm run build
```
*Build the Next.js application for production.*

```bash
ENV PORT=8080
EXPOSE 8080
```
*Set the port environment variable and expose it for Cloud Run.*

```bash
CMD ["npm","start"]
```
*Start the production server when the container runs.*

---

### 3. Authenticate & Configure GCP
```bash
gcloud auth login
```
*Login to your Google Cloud account.*

```bash
gcloud config set project YOUR_PROJECT_ID
```
*Set your GCP project to deploy the application.*

---

### 4. Set IAM Permissions
```bash
gcloud projects add-iam-policy-binding reliable-brace-483115-r0 --member="serviceAccount:734096496616-compute@developer.gserviceaccount.com" --role="roles/storage.objectViewer"
```
*Grant object viewing permissions to the service account.*

```bash
gcloud projects add-iam-policy-binding reliable-brace-483115-r0 --member="serviceAccount:734096496616-compute@developer.gserviceaccount.com" --role="roles/storage.objectCreator"
```
*Grant object creation permissions to the service account.*

---

### 5. Deploy to Cloud Run
```bash
gcloud run deploy sre-demo --source . --region us-central1 --allow-unauthenticated
```
*Deploy your Next.js application to **Cloud Run** with public access.*

---

### 6. Simulate a Backend Failure
Create a route file:  
`api/test/route.ts`
```ts
export async function GET() {
  throw new Error("Simulated Backend Failure");
}
```
*This simulates a 500 Internal Server Error to practice incident handling.*

---

## Incident Response Steps

Whenever a failure occurs, follow these SRE steps:

1. **Detect**  
   *Monitor your system to detect anomalies or errors like 500 responses.*

2. **Investigate**  
   *Identify the root cause by reviewing logs, error messages, and stack traces.*

3. **Mitigate**  
   *Apply temporary fixes or rollbacks to reduce user impact.*

4. **Recover**  
   *Restore full service by deploying patches, fixes, or configuration changes.*

5. **Learn**  
   *Document the incident, analyze failures, and improve monitoring or automation to prevent recurrence.*

Made with ❤️ by **The Techzeen**

