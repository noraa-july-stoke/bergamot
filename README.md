# bergamot
An email manager for Tangerine



```python
# heres the work in progress for the class
import re
import smtplib

class Bergamot:
    def __init__(self, email, password):
        self.email = email
        self.password = password

        self.servers = {
            'gmail': {
                'smtp_server': 'smtp.gmail.com',
                'port': 587
            },
            'yahoo': {
                'smtp_server': 'smtp.mail.yahoo.com',
                'port': 465
            },
            'hotmail': {
                'smtp_server': 'smtp.live.com',
                'port': 587
            }
        }

    def send_email(self, recipient, message):
        # Use regex to determine the email provider
        match = re.search('@(\w+)', recipient)
        if not match:
            raise ValueError('Invalid recipient email')
        provider = match.group(1)

        # Get the smtp server and port based on the email provider
        if provider in self.servers:
            server_info = self.servers[provider]
            smtp_server = server_info['smtp_server']
            port = server_info['port']
        else:
            raise ValueError('Unsupported email provider')

        # Try to send the email
        try:
            server = smtplib.SMTP(smtp_server, port)
            server.starttls()
            server.login(self.email, self.password)

            subject = 'Hello from Bergamot!'
            body = f'This message was sent from Bergamot: {message}'
            message = f'Subject: {subject}\n\n{body}'

            server.sendmail(self.email, recipient, message)

            return f'Email sent to {recipient}!'
        except:
            raise ValueError('Failed to send email')

```
