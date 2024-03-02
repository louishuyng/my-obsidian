---
tags: ROR, Backend
---
https://stackoverflow.com/questions/4703830/set-time-zone-offset-in-ruby
```ruby
def with_time_zone(tz_name)
  prev_tz = ENV['TZ']
  ENV['TZ'] = tz_name
  yield
ensure
  ENV['TZ'] = prev_tz
end
```