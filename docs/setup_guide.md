# Splunk SIEM Setup Guide

This guide will walk you through setting up a complete Splunk SIEM implementation for threat detection and monitoring.

## Prerequisites

- Windows 10/11 or Linux (Ubuntu 20.04+)
- Minimum 8GB RAM (16GB recommended)
- 100GB free disk space
- Administrator/root access

## Step 1: Download and Install Splunk Enterprise

### Windows Installation

1. **Download Splunk Enterprise**
   - Go to: https://www.splunk.com/en_us/download/splunk-enterprise.html
   - Click "Download Splunk Enterprise"
   - Choose Windows x64 version
   - Download the .msi installer

2. **Install Splunk**
   ```powershell
   # Run the installer as Administrator
   # Accept the license agreement
   # Choose installation directory (default: C:\Program Files\Splunk)
   # Set admin username and password
   ```

3. **Start Splunk**
   ```powershell
   # Navigate to Splunk directory
   cd "C:\Program Files\Splunk\bin"
   
   # Start Splunk
   .\splunk.exe start
   
   # Access web interface
   # Open browser: http://localhost:8000
   ```

### Linux Installation

1. **Download Splunk Enterprise**
   ```bash
   wget https://download.splunk.com/products/splunk/releases/8.2.9/linux/splunk-8.2.9-4a20fb65aa2d-linux-2.6-amd64.deb
   ```

2. **Install Splunk**
   ```bash
   sudo dpkg -i splunk-8.2.9-4a20fb65aa2d-linux-2.6-amd64.deb
   sudo /opt/splunk/bin/splunk start --accept-license
   ```

3. **Set up admin account**
   ```bash
   sudo /opt/splunk/bin/splunk edit user admin -password your_password -role admin -auth admin:changeme
   ```

## Step 2: Configure Data Inputs

1. **Access Splunk Web Interface**
   - Open browser: http://localhost:8000
   - Login with admin credentials

2. **Add Data Inputs**
   - Go to Settings → Data Inputs
   - Add the following inputs:

   **Windows Event Logs:**
   - Type: Windows Event Log
   - Name: Windows Security Events
   - Event Log: Security
   - Index: security

   **Syslog (Linux):**
   - Type: UDP
   - Port: 514
   - Index: security

   **File Monitoring:**
   - Type: Files & Directories
   - Path: /var/log/* (Linux) or C:\logs\* (Windows)
   - Index: security

## Step 3: Install Splunk Enterprise Security (ES)

1. **Download Splunk ES**
   - Go to Splunk Apps: https://splunkbase.splunk.com/app/263/
   - Download Splunk Enterprise Security

2. **Install Splunk ES**
   - Go to Apps → Manage Apps
   - Click "Install app from file"
   - Upload the downloaded .spl file
   - Restart Splunk

3. **Configure Splunk ES**
   - Access ES: http://localhost:8000/en-US/app/SplunkEnterpriseSecuritySuite
   - Complete initial setup wizard
   - Configure data sources and indexes

## Step 4: Import Correlation Searches

1. **Access Search & Reporting**
   - Go to Apps → Search & Reporting

2. **Import Correlation Searches**
   - Copy the contents of each .spl file from `searches/correlation_searches/`
   - Paste into the search bar and save as alerts

3. **Configure Alerts**
   - Set appropriate thresholds
   - Configure email notifications
   - Set up dashboard panels

## Step 5: Create Dashboards

1. **Security Overview Dashboard**
   - Create new dashboard
   - Add panels for:
     - Failed login attempts
     - Brute force attacks
     - Insider threats
     - Data exfiltration attempts

2. **Threat Monitoring Dashboard**
   - Real-time threat feed
   - Alert status
   - Incident response metrics

## Step 6: Configure Indexes

1. **Create Custom Indexes**
   ```bash
   # Via CLI
   /opt/splunk/bin/splunk add index security
   /opt/splunk/bin/splunk add index web
   /opt/splunk/bin/splunk add index network
   /opt/splunk/bin/splunk add index windows
   ```

2. **Apply Index Configuration**
   - Copy `config/indexes.conf` to `$SPLUNK_HOME/etc/system/local/`
   - Restart Splunk

## Step 7: Test the Implementation

1. **Generate Test Data**
   ```bash
   # Linux - Generate failed login attempts
   for i in {1..10}; do ssh nonexistent@localhost; done
   
   # Windows - Generate security events
   # Use Event Viewer to generate test events
   ```

2. **Verify Detection**
   - Check correlation searches are firing
   - Verify alerts are being generated
   - Test dashboard functionality

## Step 8: Performance Tuning

1. **Optimize Search Performance**
   - Use summary indexing for heavy searches
   - Implement data model acceleration
   - Configure search time limits

2. **Storage Management**
   - Set appropriate retention policies
   - Configure data archiving
   - Monitor disk usage

## Troubleshooting

### Common Issues

1. **Splunk won't start**
   - Check system requirements
   - Verify port availability
   - Check log files in `$SPLUNK_HOME/var/log/`

2. **No data appearing**
   - Verify data inputs are configured
   - Check sourcetype assignments
   - Verify index permissions

3. **Search performance issues**
   - Optimize search queries
   - Add summary indexes
   - Increase system resources

### Useful Commands

```bash
# Check Splunk status
/opt/splunk/bin/splunk status

# View Splunk logs
tail -f /opt/splunk/var/log/splunk/splunkd.log

# Restart Splunk
/opt/splunk/bin/splunk restart

# Check index status
/opt/splunk/bin/splunk list index
```

## Next Steps

1. **Integrate with Security Tools**
   - Configure SIEM integrations
   - Set up automated response
   - Implement threat intelligence feeds

2. **Advanced Analytics**
   - Implement machine learning
   - Create custom dashboards
   - Develop custom apps

3. **Compliance Reporting**
   - Configure compliance dashboards
   - Set up automated reporting
   - Implement audit trails

## Support Resources

- [Splunk Documentation](https://docs.splunk.com/)
- [Splunk Community](https://community.splunk.com/)
- [Splunk Answers](https://answers.splunk.com/)
