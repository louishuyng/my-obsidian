---
tags: ROR, Backend
---
```ruby
bun = Amqp::Client.client bun.start channel = bun.create_channel queue = channel.queue('process_transaction', durable: true)
```