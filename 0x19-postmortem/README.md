How a Root Command Ruined Our Day: Lessons from a Web Server Meltdown

Issue Summary
Duration of the Outage: July 23, 2024, 13:30pm - 15:15pm WAT (1 hours, 45 minutes)
Impact: The web server turned into a ghost town, inaccessible to all users. Total blackout!
Root Cause: The outage was caused by an accidental execution of the ‘rm -rf /’ command while logged in as the root user, which resulted in the deletion of the entire web directory. The web server was configured to run as root instead of a less privileged user, exacerbating the impact.

Timeline
13:30 WAT: Issue detected by ALX (platform); alerts indicated that the web server was down.
13:35 WAT: On-call engineer notified the site was down, and the issue escalated to me.
13:45 WAT: Server logs indicated missing web directory files; assumed an error during a deployment process.
13:50 WAT: Deployment scripts reviewed; no errors found. Focus shifted to filesystem integrity.
13:55 WAT: Discovered that critical web files were missing; realized that the ‘rm -rf /’ command was executed.
14:20 WAT: Further investigation confirmed that the web server was running as root.
14:45 WAT: Decision made to restore from backups; initiated the process.
14:50 WAT: Backup restoration completed; web directory restored.
15:00 WAT: Web server reconfigured to run under the nginx user, limiting future damage.
15:15 WAT: The service fully restored and accessible to all users.

Root Cause and Resolution
The root cause was the accidental execution of the ‘rm -rf /’ command while logged in as the root user. Because the web server was running with root privileges, the command deleted the entire web directory, making the site inaccessible. If the server had been running under a less privileged user like nginx, the damage would have been limited to the specific user’s permissions.
The resolution involved restoring the web directory from backups and reconfiguring the web server to run as the nginx user. This change ensured that any future accidental commands or security breaches would have a much more limited impact.

Corrective and Preventative Measures
Improvements/Fixes:
Privilege management: Ensure that no web servers run as root; instead, they should use a less privileged user like nginx.
Backup strategy: Regularly update and test backup procedures to ensure quick recovery.
Command safety: Implement safety measures to prevent the accidental execution of destructive commands.

Tasks:
Reconfigure all production web servers: Ensure they run under the nginx user.
Update system security policies: Restrict root access and enforce the use of sudo for privileged operations.
Implement command confirmation aliases: Add aliases for dangerous commands like ‘rm’ that require confirmation before execution.
Review and update backup procedures: Schedule regular tests to ensure they work effectively in disaster scenarios.
Conduct security training: Educate all engineers on the risks of using root privileges and safe command practices.
This incident underscores the importance of following best practices for system security and privilege management to minimize the risk and impact of human error.
