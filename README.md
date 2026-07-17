# aperture-Image-Sharing-social-app
Aperture is a front-end web application for an image-sharing social platform where users upload, like, and comment on photos, with a simulated backend pipeline illustrating how AWS services (S3, CloudFront, API Gateway, Lambda, DynamoDB, and Rekognition) would handle storage, delivery, and automated content tagging in production.
# Aperture — Image Sharing Social App

Aperture is a front-end web application for an image-sharing social platform where users upload, like, and comment on photos. It's built as a single-page demo with a simulated backend pipeline that illustrates how a set of AWS services would handle storage, delivery, and automated content tagging in production.


## Features

- **Photo feed** — browse uploaded photos with captions, auto-generated tags, and like/comment counts
- **Upload flow** — drag-and-drop or file picker, live preview, and caption input
- **Likes & comments** — fully interactive, persisted locally so state survives a page refresh
- **Simulated AWS pipeline** — visual step-through of Upload → S3 → API Gateway → Lambda → Rekognition → DynamoDB → CloudFront on every publish
- **Architecture walkthrough** — a plain-language section mapping each AWS service to the job it does in the flow
- **Responsive design** — works down to mobile, with visible focus states and reduced-motion support

## Tech Stack

| Layer | Choice |
|---|---|
| Structure/Styling | HTML5, CSS (custom properties, Grid/Flexbox) |
| Interactivity | Vanilla JavaScript (no build step, no dependencies) |
| Persistence | Browser `localStorage` (demo only — see [Architecture](#aws-architecture) for the production design) |
| Fonts | Space Grotesk, Inter, IBM Plex Mono (Google Fonts) |

No frameworks, no bundler, no `npm install` — it's a single `index.html` file.

## AWS Architecture

This app is designed to run on the following managed services:

| # | Service | Role |
|---|---|---|
| 1 | **Amazon API Gateway** | Front door for every request — upload, like, comment — routed to the right Lambda function |
| 2 | **AWS Lambda** | Stateless functions that handle uploads, extract metadata, and update records |
| 3 | **Amazon Rekognition** | Scans each image on upload — auto-tags content and flags anything unsuitable |
| 4 | **Amazon S3** | Durable storage for original image files |
| 5 | **Amazon CloudFront** | CDN that caches and serves images from edge locations close to each viewer |
| 6 | **Amazon DynamoDB** | Stores photo metadata — captions, tags, like counts, comment threads |

**Request flow:**

```
Upload tap → API Gateway → Lambda → S3 (store) + Rekognition (scan) → DynamoDB (record) → CloudFront (serve)
```

> **Note:** This repository contains the front end only. Uploads, likes, and comments currently run client-side (`localStorage`) so the app is fully functional without any backend setup. The pipeline steps shown during upload are a visual simulation of the flow above — wiring it to real AWS resources is the natural next step (see [Roadmap](#roadmap)).

## Getting Started




## Project Structure

```
aperture/
├── index.html      # entire app — markup, styles, and logic
└── README.md
```

## Roadmap

- [ ] Replace `localStorage` with real API calls to API Gateway
- [ ] Provision S3 bucket + CloudFront distribution for image storage/delivery
- [ ] Lambda functions for upload handling, like/comment writes
- [ ] Wire Rekognition for real content moderation and auto-tagging
- [ ] DynamoDB table design for photos, likes, and comments
- [ ] User authentication (e.g. Amazon Cognito)
