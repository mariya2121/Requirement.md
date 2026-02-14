1. System Architecture Overview
​The system follows a Serverless Event-Driven Architecture to handle heavy media processing without maintaining idle servers.
​Frontend: Next.js (React) hosted on AWS Amplify.
​API Layer: Amazon API Gateway with AWS Lambda (Node.js/Python).
​AI Orchestration: Amazon Bedrock (for metadata/NLP) and AWS Elemental (for video transcoding).
​Database: PostgreSQL (via Amazon Aurora) for relational metadata and Pinecone for vector search (personalization).
​2. Component Breakdown
​A. Media Ingestion Pipeline
​S3 Trigger: User uploads to uploads/ bucket.
​Lambda Processor: Extracts frames and audio samples.
​AI Analysis: Sends samples to Bedrock to generate descriptive tags and "highlight" timestamps.
​Database Update: Stores metadata in the MediaAssets table.
​B. Personalization Engine (Digital Experience)
​Vector Embeddings: User preferences are converted into vectors using Titan Text Embeddings.
​Matching: A Lambda function queries the vector database to find the closest media matches for the user's "Discovery Feed."
 3.Data scheme.
Table Key Fields Description 
Users id, preferences_vector, tier Stores user profile and AI-derived interests.
MediaAssets id, s3_url, tags, status Core media storage references.
Interactions user_id, media_id, watch_time Feedback loop for the AI recommendation engine.
4. Sequence Diagram: Content Generation
User requests "AI Highlight Reel."
API validates permissions and fetches raw asset URI.
Bedrock analyzes timestamps for "High Interest" moments.
FFmpeg (Lambda Layer) stitches segments together.
S3 stores the final output; CloudFront serves it to the user.
5. Security & Compliance
Authentication: Amazon Cognito for JWT-based user sessions.
Content Safety: AWS Rekognition to filter explicit or non-compliant media before it reaches the "Public" state.
Data Encryption: AES-256 at rest in S3 and RDS.
