# Automated AI Chatbot

A production ready chatbot backend that I built using n8n for multiple tenants to use at once. This system is designed to be completely automated and run without my intervention and without using code. This means that I can onboard new users without ever having to change code!.

# The Architecture

I buiilt this such that it was easy to scale up and add more users. I store all data externally in Google Sheets, which means that this workflow uses the incoming request's credentials in order to dynamically pull specific instructions and information when running.

# Core features

+ External Config: Since this workflow pulls the data from Google Sheets dynamically all of the configuration is done there without me ever having to change code.
+ Authentication: Each client has their own unique token. Each request is validated with that token which is also used for data matching.
+ Infrastructure Safety: This system has session based rate limiting to report abuse and to ensure the system is stable even under heavy use.
+ Content Moderation: Built in moderation layer that detects profanity and inappropriate language. If self harm is detected it directs users to contact 988.
+ Observability: Every part of the workflow is logged and tracked, each path has its own error code and every error is logged.

# Logic Flow

1. Auth: Incoming webhook verifies "clientID".
2. Lookup: Looks up the client ID in the Google Sheets and fetches the specific identity and information of the associated brand.
3. Screening: The user message is passed through the moderation node and checked for profanity or self harm.
4. Response: If cleared, the question is sent to the AI model and the response is normalized and returned to the user.
5. Logging: Each run is logged at the end unless there is an error in which case it is logged when the error happens.

# Tech Stack

+ Logic Engine: n8n
+ Config Database: Google Sheets
+ Inference Engine: Llama 3.1 8B via Groq
+ Messaging Protocol: Webhook / JSON

# Installation and Use

This workflow uses n8n internal credentials. To deploy, you will need to create your own credential objects for:

+ Google Sheets API (OAuth2 or Service Account)
+ Groq API (Header Auth)
