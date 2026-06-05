
#Copilot
#Teamwork

What are the fields that can be called from the Teamwork API, e.g., a user's projects and tasks in a single day? Can they be called into an Excel worksheet with Python?

Key Fields Available via Teamwork API
1. Projects
•  ID, Name, Description, Start/End Dates, Status, Category, Tags, Custom Fields
•  Company, Owner, Privacy, Notify, Start Page, Harvest Timers, Substatus
•  Project Updates, Messages, Files, Notebooks, Links, Tasks, Milestones, Time Entries, People, Custom Fields  ​⁠.
2. Tasks
•  ID, Name, Description, Start/End Dates, Due Date, Priority, Progress, Status, Tags
•  Assignees (User IDs), Creator, Time Estimates, Time Spent, Custom Fields
•  Task List ID, Project ID, Parent Task ID, Subtasks, Followers, Reminders  ​⁠.
3. Users
•  ID, Name, Email, Role, Avatar, Last Activity, Company, Department, Skills, Custom Fields  ​⁠.
4. Time Entries
•  ID, Description, Date, Time Spent, User ID, Project ID, Task ID, Billable Status, Tags  ​⁠.
Filtering for a Single Day
To retrieve tasks for a specific day, use the startAt and dueAt fields with date filters in the API request. For example:
•  /projects/api/v3/tasks.json?filter=startAt:2026-05-01 (for tasks starting on May 1, 2026)
•  /projects/api/v3/tasks.json?filter=dueAt:2026-05-01 (for tasks due on May 1, 2026)
•  You can also filter by responsible-party-ids to get tasks assigned to a specific user  ​⁠.
Exporting to Excel with Python

Here’s a basic workflow to fetch data from the Teamwork API and export it to an Excel file using Python:

pip install requests pandas openpyxl

1. Install Required Libraries
pip install requests pandas openpyxl
2. Python Script Example
import requests
import pandas as pd
import base64
# Replace with your Teamwork site name and API key
site_name = "yourSiteName"
api_key = "yourAPIKey"
user_email = "yourEmail"
# Encode credentials for Basic Auth
credentials = f"{api_key}:xxx"  # Some APIs use API key as username, leave password empty or use a token
encoded_credentials = base64.b64encode(credentials.encode()).decode()
# Headers for API request
headers = {
    "Authorization": f"Basic {encoded_credentials}",
    "Content-Type": "application/json"
}
# API endpoint to get tasks for a specific day
url = f"https://{site_name}.teamwork.com/projects/api/v3/tasks.json"
params = {
    "filter": "startAt:2026-05-01",  # Filter for tasks starting on May 1, 2026
    "includeCustomFields": "true"
}
# Make the API request
response = requests.get(url, headers=headers, params=params)
data = response.json()
# Extract tasks
tasks = data.get("tasks", [])
# Convert to DataFrame
df = pd.DataFrame(tasks)
# Select relevant columns (customize as needed)
df = df[["id", "name", "description", "startAt", "dueAt", "assignees", "status"]]
# Export to Excel
df.to_excel("teamwork_tasks_2026_05_01.xlsx", index=False)
print("Data exported to Excel!")
3. Notes
•  Authentication: Teamwork API typically uses Basic Auth (API key + email) or OAuth2. Check your API settings for the correct method  ​⁠.
•  Pagination: If you have many tasks, use pagination (?page=1&pageSize=500) to fetch all results  ​⁠.
•  Custom Fields: Use includeCustomFields=true to include custom fields in the response  ​⁠.
•  Sparse Fieldsets: Use ?include=... to specify only the fields you need, improving performance  ​⁠.

Documentation References
•  Teamwork API V3 Documentation
•  Tasks Endpoint
•  Custom Fields and Sparse Fieldsets   ​⁠

https://apidocs.teamwork.com/docs/teamwork/v3/projects/get-projects-api-v3-projects-json

