# Security Group Configuration

## Database Server
- MySQL (3306): Application Server SG only
- SSH (22): My IP

## Application Server
- HTTP (80): 0.0.0.0/0
- HTTPS (443): 0.0.0.0/0
- SSH (22): My IP

## Security Rationale
- Prevents public database exposure
- Enforces least privilege access
- Aligns with AWS best practices
