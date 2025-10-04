<img width="707" height="245" alt="image" src="https://github.com/user-attachments/assets/9c27edc2-7dd1-45e9-a31b-e14623b25020" />

# Google Maps Business Scraper (n8n Workflow)

This n8n workflow automates the process of scraping business phone numbers and related information from **Google Maps** using the **Bright Data API**, and saves the results into **Google Sheets**.

---

## 📌 Workflow Overview

This automation performs the following:

1. Accepts a location and keyword input from a user form.
2. Triggers a data scrape using Bright Data API.
3. Monitors the scraping status.
4. Retrieves business data (e.g., name, address, phone number).
5. Saves the results into a Google Sheet.

---

## 🔧 Workflow Components

### 1. **Form Trigger – Submit Location and Keywords**
- **Type:** Form Trigger
- **Webhook ID:** `8b72dcdf-25a1-4b63-bb44-f918f7095d5d`
- **Form Title:** `GMB`
- **Fields:**
  - `Location` (required)
  - `Keywords` (required)

### 2. **Bright Data API – Request Business Data**
- **Type:** HTTP Request (POST)
- **Headers:**
  - `Authorization: Bearer BRIGHT_DATA_API_KEY`
- **Query Parameters:**
  - `dataset_id: gd_m8ebnr0q2qlklc02fz`
  - `include_errors: true`
  - `type: discover_new`
  - `discover_by: location`
  - `limit_per_input: 2`
3. Check Scraping Status

Type: HTTP Request (GET)

URL: https://api.brightdata.com/datasets/v3/progress/{{ $json.snapshot_id }}

Headers:

Authorization: Bearer BRIGHT_DATA_API_KEY

Query Parameters:

format=json

4. Check If Status Ready

Type: IF (Conditional)

Condition: {{ $json.status }} equals "ready"

5. Wait Before Retry

Type: Wait

Duration: 1 minute

Webhook ID: 7047efad-de41-4608-b95c-d3e0203ef620

6. Check Records Exist

Type: IF (Conditional)

Condition: {{ $json.records }} not equals 0

7. Fetch Business Data

Type: HTTP Request (GET)

Headers:

Authorization: Bearer BRIGHT_DATA_API_KEY

Query Parameters:

format=json

8. Save to Google Sheets

Type: Google Sheets (Append)

Document ID: YOUR_GOOGLE_SHEET_ID

Sheet Name: GMB

Column Mapping:

Name: {{ $json.name }}

Address: {{ $json.address }}

Rating: {{ $json.rating }}

Phone Number: {{ $json.phone_number }}

URL: {{ $json.url }}

📂 Workflow Flow Summary

Start: User submits the form (Location + Keywords).

Request: Bright Data API is triggered for scraping.

Monitor: Check status every 1 minute until complete.

Validate: Proceed only if data is returned.

Fetch: Retrieve business data from snapshot.

Save: Append results to Google Sheets.

✅ Setup Requirements
API Keys & Credentials

Bright Data API Key: Replace BRIGHT_DATA_API_KEY in HTTP request headers.

Google Sheets: Set up OAuth2 credentials in n8n for access.

Google Sheet ID: Replace YOUR_GOOGLE_SHEET_ID with your actual sheet ID.

Google Sheet Setup

Create a Google Sheet with a tab named GMB

Add the following column headers:

Name

Address

Rating

Phone Number

URL

⚙️ Workflow Info

Workflow ID: Hm7iTSgpu2of6gz4

Version ID: 0bed9bf1-00a3-4eb6-bf7c-cf07bee006a2

Execution Order: v1

Status: Inactive (Currently)

📌 Notes

A built-in retry mechanism waits 1 minute if the scraping process isn't finished.

Data validation ensures only completed and non-empty datasets are saved.

Supports easy submission of jobs via a form.

🪪 License

MIT License
