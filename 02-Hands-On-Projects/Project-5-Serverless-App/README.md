# Project 5: Serverless Full-Stack Application 🏗️

## Overview
Build a complete serverless application combining multiple AWS services: API Gateway, Lambda, DynamoDB, S3, and CloudWatch.

**Time**: 3-4 hours
**Cost**: FREE (within Free Tier)
**Difficulty**: 🟡 Intermediate

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User's Browser                            │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│          S3 (Static Website Hosting)                         │
│  ├── index.html                                              │
│  └── styles.css                                              │
└─────────────────────────────────────────────────────────────┘
                          ↓
                    API Requests
                          ↓
┌─────────────────────────────────────────────────────────────┐
│          API Gateway (REST API)                              │
│  ├── POST /notes (create)                                   │
│  ├── GET /notes (list)                                      │
│  ├── GET /notes/{id} (get)                                  │
│  └── DELETE /notes/{id} (delete)                            │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│          Lambda Functions                                    │
│  ├── CreateNote                                              │
│  ├── ListNotes                                               │
│  ├── GetNote                                                 │
│  └── DeleteNote                                              │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│          DynamoDB (Notes Table)                              │
│  ├── note_id (Primary Key)                                  │
│  ├── title                                                   │
│  ├── content                                                 │
│  └── created_at                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Project: Build a Notes App

Create a simple notes application (like Google Keep) entirely on AWS serverless.

### Step 1: Create DynamoDB Table

```bash
aws dynamodb create-table \
  --table-name Notes \
  --attribute-definitions \
    AttributeName=note_id,AttributeType=S \
  --key-schema AttributeName=note_id,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

Or via Console:
- Table name: Notes
- Partition key: note_id (String)
- Billing: On-demand

---

### Step 2: Create Lambda Functions

#### Lambda 1: Create Note

```python
import json
import boto3
import uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Notes')

def lambda_handler(event, context):
    """
    Create a new note
    """
    try:
        body = json.loads(event.get('body', '{}'))
        
        note_id = str(uuid.uuid4())
        title = body.get('title', 'Untitled')
        content = body.get('content', '')
        
        # Save to DynamoDB
        table.put_item(
            Item={
                'note_id': note_id,
                'title': title,
                'content': content,
                'created_at': datetime.now().isoformat()
            }
        )
        
        return {
            'statusCode': 201,
            'body': json.dumps({
                'message': 'Note created',
                'note_id': note_id
            }),
            'headers': {'Content-Type': 'application/json'}
        }
    
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)}),
            'headers': {'Content-Type': 'application/json'}
        }
```

#### Lambda 2: List Notes

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Notes')

def lambda_handler(event, context):
    """
    List all notes
    """
    try:
        response = table.scan()
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'notes': response.get('Items', [])
            }),
            'headers': {'Content-Type': 'application/json'}
        }
    
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)}),
            'headers': {'Content-Type': 'application/json'}
        }
```

#### Lambda 3: Get Single Note

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Notes')

def lambda_handler(event, context):
    """
    Get a single note by ID
    """
    try:
        note_id = event['pathParameters']['id']
        
        response = table.get_item(Key={'note_id': note_id})
        
        if 'Item' not in response:
            return {
                'statusCode': 404,
                'body': json.dumps({'error': 'Note not found'}),
                'headers': {'Content-Type': 'application/json'}
            }
        
        return {
            'statusCode': 200,
            'body': json.dumps(response['Item']),
            'headers': {'Content-Type': 'application/json'}
        }
    
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)}),
            'headers': {'Content-Type': 'application/json'}
        }
```

#### Lambda 4: Delete Note

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Notes')

def lambda_handler(event, context):
    """
    Delete a note
    """
    try:
        note_id = event['pathParameters']['id']
        
        table.delete_item(Key={'note_id': note_id})
        
        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Note deleted'}),
            'headers': {'Content-Type': 'application/json'}
        }
    
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)}),
            'headers': {'Content-Type': 'application/json'}
        }
```

---

### Step 3: Create API Gateway

1. **Go to API Gateway** → **Create API**
2. **Choose REST API**
3. **API name**: NotesAPI
4. **Create API**

#### Create Resources & Methods

```
API Structure:
/
└── /notes
    ├── POST (CreateNote Lambda)
    ├── GET (ListNotes Lambda)
    └── /{id}
        ├── GET (GetNote Lambda)
        └── DELETE (DeleteNote Lambda)
```

**Create /notes resource**:
1. Click "/"
2. "Create resource"
3. Name: notes
4. Create

**Create POST method**:
1. Click /notes
2. "Create method" → POST
3. Lambda Function: CreateNote-function
4. Save

**Create GET method**:
1. Click /notes
2. "Create method" → GET
3. Lambda Function: ListNotes-function
4. Save

**Create /{id} resource**:
1. Click /notes
2. "Create resource"
3. Name: {id}
4. Create

**Create GET for /{id}**:
1. Click /notes/{id}
2. "Create method" → GET
3. Lambda Function: GetNote-function
4. Save

**Create DELETE for /{id}**:
1. Click /notes/{id}
2. "Create method" → DELETE
3. Lambda Function: DeleteNote-function
4. Save

#### Deploy API

1. **Click "Deploy API"**
2. **Stage**: prod
3. **Create**
4. **Get Invoke URL**: https://abc123.execute-api.us-east-1.amazonaws.com/prod

---

### Step 4: Create Frontend (S3 + HTML)

1. **Create S3 bucket**: my-notes-app-frontend
2. **Create index.html**:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Notes App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background: #f5f5f5;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        h1 {
            color: #FF9900;
            margin-bottom: 20px;
        }
        
        .input-section {
            margin-bottom: 20px;
        }
        
        input, textarea {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-family: Arial;
        }
        
        button {
            background: #FF9900;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        
        button:hover {
            background: #ec7211;
        }
        
        .note {
            background: #fff9e6;
            border-left: 4px solid #FF9900;
            padding: 15px;
            margin: 10px 0;
            border-radius: 4px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .note-content h3 {
            margin-bottom: 5px;
        }
        
        .delete-btn {
            background: #ff4444;
            padding: 5px 10px;
            font-size: 14px;
        }
        
        .delete-btn:hover {
            background: #cc0000;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📝 My Notes</h1>
        
        <div class="input-section">
            <input type="text" id="title" placeholder="Note title...">
            <textarea id="content" placeholder="Note content..." rows="4"></textarea>
            <button onclick="createNote()">Add Note</button>
        </div>
        
        <div id="notes-container"></div>
    </div>
    
    <script>
        // Replace with your API URL
        const API_URL = 'https://YOUR-API-ID.execute-api.us-east-1.amazonaws.com/prod';
        
        // Load notes on page load
        window.onload = function() {
            loadNotes();
        };
        
        async function createNote() {
            const title = document.getElementById('title').value;
            const content = document.getElementById('content').value;
            
            if (!title || !content) {
                alert('Please fill in all fields');
                return;
            }
            
            try {
                const response = await fetch(`${API_URL}/notes`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ title, content })
                });
                
                if (response.ok) {
                    document.getElementById('title').value = '';
                    document.getElementById('content').value = '';
                    loadNotes();
                } else {
                    alert('Error creating note');
                }
            } catch (error) {
                console.error('Error:', error);
                alert('Error creating note');
            }
        }
        
        async function loadNotes() {
            try {
                const response = await fetch(`${API_URL}/notes`);
                const data = await response.json();
                
                const container = document.getElementById('notes-container');
                container.innerHTML = '';
                
                (data.notes || []).forEach(note => {
                    const noteDiv = document.createElement('div');
                    noteDiv.className = 'note';
                    noteDiv.innerHTML = `
                        <div class="note-content">
                            <h3>${note.title}</h3>
                            <p>${note.content}</p>
                            <small>${note.created_at}</small>
                        </div>
                        <button class="delete-btn" onclick="deleteNote('${note.note_id}')">Delete</button>
                    `;
                    container.appendChild(noteDiv);
                });
            } catch (error) {
                console.error('Error:', error);
            }
        }
        
        async function deleteNote(noteId) {
            if (!confirm('Delete this note?')) return;
            
            try {
                const response = await fetch(`${API_URL}/notes/${noteId}`, {
                    method: 'DELETE'
                });
                
                if (response.ok) {
                    loadNotes();
                } else {
                    alert('Error deleting note');
                }
            } catch (error) {
                console.error('Error:', error);
                alert('Error deleting note');
            }
        }
    </script>
</body>
</html>
```

3. **Upload to S3**
4. **Enable static website hosting**
5. **Access via browser**

---

## Testing

### Using cURL

```bash
# Create note
curl -X POST https://abc123.execute-api.us-east-1.amazonaws.com/prod/notes \
  -H "Content-Type: application/json" \
  -d '{"title": "My First Note", "content": "Hello AWS!"}'

# List notes
curl https://abc123.execute-api.us-east-1.amazonaws.com/prod/notes

# Get note
curl https://abc123.execute-api.us-east-1.amazonaws.com/prod/notes/note-id-here

# Delete note
curl -X DELETE https://abc123.execute-api.us-east-1.amazonaws.com/prod/notes/note-id-here
```

---

## Architecture Benefits

✅ **Serverless**: No servers to manage
✅ **Scalable**: Automatic scaling
✅ **Cost-Effective**: Pay only for usage
✅ **Secure**: Managed security
✅ **Available**: Multi-AZ deployment

---

## Interview Questions

**Q: What services did we use?**
A: API Gateway (REST API), Lambda (serverless compute), DynamoDB (NoSQL database), S3 (frontend hosting), CloudWatch (monitoring).

**Q: Why serverless?**
A: No infrastructure management, automatic scaling, pay-per-use, faster deployment.

**Q: What are limitations?**
A: 15-minute timeout, cold starts, no persistent storage in /tmp, eventually consistent databases.

**Q: How would you add authentication?**
A: Use API Gateway API Key or AWS Cognito for user authentication.

---

## Cleanup

```bash
# Delete API Gateway
aws apigateway delete-rest-api --rest-api-id abc123

# Delete Lambda functions
aws lambda delete-function --function-name CreateNote
aws lambda delete-function --function-name ListNotes
aws lambda delete-function --function-name GetNote
aws lambda delete-function --function-name DeleteNote

# Delete DynamoDB table
aws dynamodb delete-table --table-name Notes

# Delete S3 bucket
aws s3 rb s3://my-notes-app-frontend --force
```

---

## Congratulations! 🎉

You've built a complete serverless application!

**Skills You've Learned**:
- ✅ Lambda functions
- ✅ API Gateway REST APIs
- ✅ DynamoDB CRUD operations
- ✅ S3 static hosting
- ✅ Frontend-backend integration
- ✅ Serverless architecture

---

Last updated: June 29, 2026
