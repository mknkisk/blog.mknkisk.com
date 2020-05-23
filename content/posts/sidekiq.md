---
title: 'Sidekiq API'
date: 2016-03-22T16:23:08+09:00
draft: false
---

## Queue

Deletes all Jobs in a Queue, by removing the queue.

```rb
Sidekiq::Queue.new.clear
```

link: [API · mperham/sidekiq Wiki](https://github.com/mperham/sidekiq/wiki/API#queue)
