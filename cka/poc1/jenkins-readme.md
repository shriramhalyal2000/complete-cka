# configuring jenkins
- select vm type, on premise or cloud provider based.
- AWS ec2 h/a h/w.
- jenkinsport 8080, SG allow traffic on port 8080.
- if EC2 uses amzn linux 2 , then use amzn optimised jave , for Jenkins dependency.
- If has Agent-Nodes, then same dependency should also be present, with their *.pem files to configure jenkins nodes as agents.
- Jenkins if configured on AWS, should have relevent iam roles to access other enbviornments and to deploy.
- Configure SCm with branch or Dir,
- Add jenkinsfile in same code scm and configure webhooks, libraries and build notifications.