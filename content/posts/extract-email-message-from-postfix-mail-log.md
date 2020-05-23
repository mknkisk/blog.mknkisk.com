---
title: 'Extract email & message of bounced mail from Postfix logs'
date: 2016-03-01T01:36:02+09:00
draft: false
---

Extract email & message from Postfix logs

```
$ grep 'status=bounced' mail.log | gawk 'match($0, /to=<(.+)>,.+said: (.+)$/, m) {print m[1] "," m[2]}' > extract_email_message_from_maillog.csv
```

```
$ cat extract_email_message_from_maillog.csv
hoge@example.com, 550 <hoge@example.com>: User unknown (in reply to RCPT TO command))
hoge@example.com, 50-5.1.1 The email account that you tried to reach does not exist. Please try 550-5.1.1 double-checking the recipient's email address for typos or 550-5.1.1 unnecessary spaces. Learn more at 550 5.1.1  https://support.google.com/mail/answer/6596
```

Sort error code

```
$ sort -t, -nk2 extract_email_message_from_maillog.csv
```

For rejecting reception

```
$ grep '550 Unknown user' extract_email_message_from_maillog.csv | grep 'end of DATA'
```

Email address unknow

```
$ grep '550 Unknown user' extract_email_message_from_maillog.csv | grep 'reply to RCPT TO'
```
