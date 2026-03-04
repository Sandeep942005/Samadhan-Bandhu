# Deployment & Production Guide

## Pre-Deployment Checklist

- [ ] Update JWT_SECRET to a strong random key
- [ ] Set NODE_ENV=production
- [ ] Update FRONTEND_URL to your production domain
- [ ] Enable HTTPS/SSL
- [ ] Set up database backups
- [ ] Configure error logging
- [ ] Set up monitoring and alerts
- [ ] Test all API endpoints
- [ ] Verify database integrity

## Environment Variables for Production

```env
PORT=5000
NODE_ENV=production
FRONTEND_URL=https://yourdomain.com
JWT_SECRET=generate_very_strong_secret_key_here
JWT_EXPIRY=7d
DB_PATH=/var/data/samadhan.db
UPLOAD_DIR=/var/uploads
```

## Database Backup Strategy

### Automated Backup Script

```bash
#!/bin/bash
# backup_db.sh
BACKUP_DIR="/backups/samadhan"
DB_FILE="./data/samadhan.db"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)

mkdir -p $BACKUP_DIR
cp $DB_FILE $BACKUP_DIR/samadhan_$TIMESTAMP.db

# Keep only last 30 backups
find $BACKUP_DIR -name "*.db" -type f -mtime +30 -delete
```

Run with cron:
```
0 2 * * * /path/to/backup_db.sh
```

## Deployment with PM2

### Install PM2
```bash
npm install -g pm2
```

### Create ecosystem.config.js
```javascript
module.exports = {
  apps: [{
    name: 'samadhan-backend',
    script: './server.js',
    env: {
      NODE_ENV: 'production',
      PORT: 5000
    },
    instances: 'max',
    exec_mode: 'cluster',
    error_file: './logs/error.log',
    out_file: './logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm:ss Z'
  }]
};
```

### Deploy
```bash
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

## Nginx Configuration

```nginx
upstream samadhan_backend {
  server 127.0.0.1:5000;
  keepalive 64;
}

server {
  listen 443 ssl http2;
  server_name api.yourdomain.com;

  ssl_certificate /path/to/certificate.pem;
  ssl_certificate_key /path/to/key.pem;

  client_max_body_size 50M;

  location / {
    proxy_pass http://samadhan_backend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }

  # WebSocket support
  location /socket.io {
    proxy_pass http://samadhan_backend/socket.io;
    proxy_http_version 1.1;
    proxy_buffering off;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
  }
}

# Redirect HTTP to HTTPS
server {
  listen 80;
  server_name api.yourdomain.com;
  return 301 https://$server_name$request_uri;
}
```

## Docker Deployment

### Dockerfile
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 5000

CMD ["node", "server.js"]
```

### docker-compose.yml
```yaml
version: '3.8'

services:
  backend:
    build: .
    ports:
      - "5000:5000"
    environment:
      NODE_ENV: production
      PORT: 5000
      JWT_SECRET: ${JWT_SECRET}
      FRONTEND_URL: ${FRONTEND_URL}
    volumes:
      - ./data:/app/data
      - ./uploads:/app/uploads
    restart: unless-stopped
```

### Deploy with Docker
```bash
docker-compose up -d
```

## Monitoring & Logging

### PM2 Monitoring
```bash
pm2 monit
pm2 logs samadhan-backend
```

### Set up ELK Stack (Elasticsearch, Logstash, Kibana)

Add to your logging system for production monitoring.

## Performance Optimization

### 1. Database Indexes
```sql
CREATE INDEX idx_user_email ON users(email);
CREATE INDEX idx_project_state ON projects(state);
CREATE INDEX idx_tender_status ON tenders(status);
CREATE INDEX idx_payment_status ON payments(status);
CREATE INDEX idx_inspection_project ON inspections(projectId);
```

### 2. Connection Pooling
Consider upgrading to SQLite connection pooling for high traffic.

### 3. Caching
Implement Redis caching for frequently accessed data:
```bash
npm install redis
```

### 4. Compression
Enable gzip compression in Nginx/production.

## Security Hardening

### 1. Rate Limiting
Add express-rate-limit:
```bash
npm install express-rate-limit
```

```javascript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

### 2. CORS Whitelist
Update CORS to specific domains only in production.

### 3. SQL Injection Protection
Using parameterized queries (already implemented).

### 4. XSS Protection
Helmet middleware already enabled.

### 5. CSRF Protection
```bash
npm install csurf
```

## Monitoring Checklist

- [ ] Server health checks
- [ ] Database connection monitoring
- [ ] API response time monitoring
- [ ] Error rate monitoring
- [ ] Disk space monitoring
- [ ] Memory usage monitoring
- [ ] CPU usage monitoring
- [ ] Network traffic monitoring

## Scaling Strategy

1. **Vertical Scaling**: Increase server resources
2. **Horizontal Scaling**: Use load balancer with multiple instances
3. **Database Replication**: Set up read replicas
4. **Caching Layer**: Implement Redis
5. **CDN**: Use for static assets

## CI/CD Pipeline Example

### GitHub Actions
```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      
      - run: npm install
      - run: npm test
      - run: npm run build
      
      - name: Deploy to server
        run: |
          # Your deployment commands
          ssh user@server "cd /app && git pull && npm install && pm2 restart all"
```

## Troubleshooting Production Issues

### High Memory Usage
```bash
pm2 kill
pm2 start ecosystem.config.js
```

### Database Lock
```bash
# Check locks
lsof | grep samadhan.db

# Clear locks
pkill -f server.js
```

### Connection Limits
Monitor and increase max connections in SQLite:
```javascript
db.configure("busyTimeout", 5000);
```

## Rollback Procedure

1. Keep previous version deployed
2. Use PM2 for instant restart
3. Database migration rollback scripts
4. Monitor logs after rollback

## Incident Response

1. **Detection**: Monitor alerts
2. **Notification**: Alert on-call engineer
3. **Investigation**: Check logs and metrics
4. **Mitigation**: Implement quick fix
5. **Resolution**: Full fix deployment
6. **PostMortem**: Document and learn

## Maintenance Windows

Schedule maintenance during off-peak hours:
- Database optimization (VACUUM)
- Backup verification
- Security updates
- Log rotation

---

**Last Updated**: December 2024
**Maintenance Level**: Production Ready
