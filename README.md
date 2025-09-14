# Sproutify Monitoring System

An intelligent security monitoring and incident response system that combines AI-powered log analysis with automated workflow management.

## Overview

The Sproutify Monitoring System automatically analyzes system logs, detects security threats, and creates actionable incident reports. It transforms raw log data into structured security intelligence and provides automated response workflows.

## Features

- **AI-Powered Log Analysis**: LLM-based analysis of system logs for intelligent threat detection
- **Real-time Security Monitoring**: Continuous monitoring of authentication failures, suspicious IP activity, and system anomalies
- **Automated Incident Reporting**: Daily comprehensive security reports generated automatically
- **Structured Data Storage**: Security events stored in Supabase for historical analysis and trending
- **GitHub Integration**: Security incidents tracked as GitHub issues with proper categorization
- **N8N Workflow Automation**: Flexible workflow engine for security response automation

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Loki Logs     │───▶│  AI Analysis    │───▶│    N8N          │
│   (Raw Data)    │    │  (LLM Summary)  │    │  (Workflows)    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                       │
                                               ┌───────▼───────┐
┌─────────────────┐    ┌─────────────────┐    │               │
│   GitHub        │◄───│   Supabase      │◄───│   Security    │
│   (Issues)      │    │   (Database)    │    │   Processing  │
└─────────────────┘    └─────────────────┘    └───────────────┘
```

## Components

### 1. Log Analysis Engine
- **Source**: Loki log aggregation system
- **Processing**: AI-powered analysis using LLM summarization
- **Output**: Categorized security events with severity levels

### 2. Data Storage (Supabase)
- **Table**: `security_alerts`
- **Fields**: timestamp, source_ip, attack_type, severity, raw_content, occurrence_count
- **Indexing**: Optimized for date-based queries and IP address lookups

### 3. Workflow Automation (N8N)
- **Hourly Workflows**: Process and store security events
- **Daily Workflows**: Generate comprehensive security reports
- **Integration**: Webhook-based triggers and API integrations

### 4. Incident Management (GitHub)
- **Daily Reports**: Automated issue creation with security summaries
- **Labeling**: Smart categorization (security, critical, auth-attack, etc.)
- **Tracking**: Historical incident data and response tracking

## Security Event Categories

### Critical Events
- Authentication failures from unknown IPs
- Brute force attacks
- Suspicious login patterns
- System compromise indicators

### Warning Events
- Multiple failed authentication attempts
- Unusual connection patterns
- Configuration changes
- Service disruptions

### Monitor Events
- Repeated cron sessions
- Service timeouts
- Performance anomalies
- Routine maintenance activities

## Installation & Setup

### Prerequisites
- N8N workflow automation platform
- Supabase database account
- GitHub repository access
- Loki log aggregation system

### Database Setup

1. Create Supabase table:
```sql
CREATE TABLE security_alerts (
  id SERIAL PRIMARY KEY,
  timestamp TIMESTAMPTZ,
  source_ip INET,
  attack_type TEXT,
  severity TEXT,
  raw_content TEXT,
  first_seen TIMESTAMPTZ,
  last_seen TIMESTAMPTZ,
  occurrence_count INTEGER DEFAULT 1,
  date_only TEXT
);
```

2. Create indexes for performance:
```sql
CREATE INDEX idx_security_alerts_date ON security_alerts(date_only);
CREATE INDEX idx_security_alerts_ip ON security_alerts(source_ip);
CREATE INDEX idx_security_alerts_severity ON security_alerts(severity);
```

### N8N Workflow Configuration

1. **Import Workflows**: Import the provided N8N workflow JSON files
2. **Configure Credentials**: Set up Supabase, GitHub, and webhook credentials
3. **Set Webhook URL**: Configure Loki to send data to N8N webhook endpoint
4. **Schedule Daily Reports**: Set cron schedule for daily report generation

### GitHub Integration

1. **Create Labels**: Set up security-specific labels
   - `security` (red)
   - `critical` (dark red)
   - `daily-report` (blue)
   - `auth-attack` (orange)
   - `high-priority` (yellow)

2. **Generate Token**: Create GitHub Personal Access Token with `repo` permissions

## Usage

### Monitoring Dashboard
- View real-time security events in Supabase dashboard
- Filter by date, IP address, or attack type
- Monitor attack trends and patterns

### Daily Security Reports
- Automated GitHub issues created daily at 6 AM
- Comprehensive analysis of previous day's security events
- Actionable recommendations and IP blocking lists

### Incident Response
- Critical alerts trigger immediate notifications
- GitHub issues provide structured incident tracking
- Historical data enables pattern recognition and trend analysis

## Sample Daily Report

Daily reports include:

- **Overview**: Total events, unique IPs, critical incidents
- **Top Attacking IPs**: Detailed analysis with attack counts and duration
- **Attack Type Breakdown**: Categorized threat analysis
- **Hourly Patterns**: Visual representation of attack timing
- **Immediate Actions**: Specific IPs to block and security recommendations
- **Network Analysis**: Geographic distribution and persistence patterns

## API Endpoints

### Webhook Endpoint
```
POST /webhook/{workflow-id}
Content-Type: application/json

{
  "timestamp": "2025-09-14T15:32:08.935Z",
  "system_status": "CRITICAL",
  "critical_issues": [...],
  "summary_counts": {...}
}
```

## Configuration

### Environment Variables
- `SUPABASE_URL`: Supabase project URL
- `SUPABASE_ANON_KEY`: Supabase anonymous key
- `GITHUB_TOKEN`: GitHub Personal Access Token
- `N8N_WEBHOOK_URL`: N8N webhook endpoint

### Customization
- Modify severity thresholds in analysis logic
- Adjust report scheduling in N8N workflows
- Customize GitHub issue templates
- Configure additional alert channels (Slack, email, etc.)

## Security Considerations

- Store credentials securely in N8N credential store
- Use Supabase Row Level Security (RLS) policies
- Regularly rotate GitHub tokens
- Monitor N8N workflow execution logs
- Implement IP whitelisting for webhook endpoints

## Troubleshooting

### Common Issues
1. **Webhook not receiving data**: Check Loki configuration and N8N endpoint URL
2. **Database connection errors**: Verify Supabase credentials and network access
3. **GitHub API rate limits**: Monitor API usage and implement retry logic
4. **Missing daily reports**: Check N8N workflow schedule and execution logs

### Log Analysis
- Review N8N execution logs for workflow errors
- Check Supabase logs for database connection issues
- Monitor GitHub API response codes for integration problems

## Contributing

1. Fork the repository
2. Create a feature branch
3. Implement changes with appropriate testing
4. Submit pull request with detailed description

## License

MIT License - see LICENSE file for details

## Support

For issues and questions:
- Create GitHub issues for bug reports
- Use discussions for feature requests
- Check workflow execution logs for troubleshooting

---

**Sproutify Monitoring System** - Intelligent Security Monitoring & Incident Response
