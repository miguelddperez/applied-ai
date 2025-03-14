# N8N and Supabase Setup Documentation

## N8N Installation and Configuration

### Installation Method
I installed N8N using Docker with the following command:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Configuration Details
- **N8N Version:** 0.219.0
- **Access URL:** http://localhost:5678
- **Webhook Base URL:** https://mywebhook.example.com (using ngrok for development)
- **Timezone Setting:** America/Sao_Paulo

### Environment Variables
I've set up the following environment variables for N8N:

```
N8N_ENCRYPTION_KEY=********
N8N_PORT=5678
N8N_PROTOCOL=http
N8N_HOST=localhost
WEBHOOK_URL=https://mywebhook.example.com
```

### Installed Nodes
- HTTP Request
- WhatsApp
- Supabase
- OpenAI
- Function
- IF/Switch
- Set
- Cron

### Initial Workflow
I've created a basic test workflow to verify the N8N installation:
1. Trigger: Manual
2. HTTP Request: GET to test API
3. Function: Process response
4. Set: Format output

The workflow successfully executes and returns the expected results.

## Supabase Setup

### Project Creation
I created a new Supabase project with the following details:
- **Project Name:** HealthBuddy
- **Database Password:** [Redacted]
- **Region:** us-east-1
- **Pricing Plan:** Free tier

### Database Schema
I've designed the initial database schema with the following tables:

#### 1. users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  phone_number VARCHAR NOT NULL UNIQUE,
  first_name VARCHAR,
  last_name VARCHAR,
  date_of_birth DATE,
  medical_conditions TEXT[],
  emergency_contact VARCHAR,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 2. medications
```sql
CREATE TABLE medications (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  medication_name VARCHAR NOT NULL,
  dosage VARCHAR NOT NULL,
  frequency VARCHAR NOT NULL,
  time_of_day TIME[] NOT NULL,
  start_date DATE NOT NULL,
  end_date DATE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3. health_metrics
```sql
CREATE TABLE health_metrics (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  metric_type VARCHAR NOT NULL,
  metric_value DECIMAL NOT NULL,
  unit VARCHAR NOT NULL,
  recorded_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  notes TEXT
);
```

#### 4. messages
```sql
CREATE TABLE messages (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  direction VARCHAR NOT NULL CHECK (direction IN ('incoming', 'outgoing')),
  content TEXT NOT NULL,
  message_type VARCHAR NOT NULL DEFAULT 'text',
  sent_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  metadata JSONB
);
```

### API Configuration
I've set up the following API policies:
- Public access for user registration
- Authenticated access for all other operations
- Row-level security policies to ensure users can only access their own data

### Authentication Setup
I've configured Supabase Auth with:
- Phone authentication enabled
- Custom email templates
- Session expiry set to 7 days

## Integration Testing

I've verified the connection between N8N and Supabase by:
1. Creating a test workflow in N8N that inserts a record into the users table
2. Confirming the record appears in the Supabase database
3. Testing a query operation from N8N to retrieve data from Supabase

All integration tests were successful, confirming that the basic infrastructure is properly set up.

## Challenges and Solutions

### Challenge 1: N8N Webhook Configuration
Initially, I had trouble getting the webhook URL to work properly. The issue was that my local N8N instance wasn't accessible from the internet.

**Solution:** I used ngrok to create a secure tunnel to my local instance with:
```bash
ngrok http 5678
```
Then I configured the `WEBHOOK_URL` environment variable with the ngrok URL.

### Challenge 2: Supabase RLS Policies
I encountered issues with row-level security policies where queries were returning no results even for authenticated users.

**Solution:** I discovered that my RLS policy was incorrectly written. I fixed it by changing:
```sql
-- Incorrect
CREATE POLICY "Users can only access their own data"
ON users FOR ALL
USING (auth.uid() = id);

-- Correct
CREATE POLICY "Users can only access their own data"
ON users FOR ALL
USING (auth.uid()::text = id::text);
```

## Next Steps

For the next phase of development, I plan to:
1. Set up the WhatsApp API integration
2. Create the initial message handling workflow in N8N
3. Implement the database triggers for automated responses
4. Design the conversation flow for user onboarding
