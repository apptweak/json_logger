# Huyegger

This is JSON Logger (Huyegger) for ruby.

## Installation

```ruby
gem install hyegger

gem "huyegger"
```

## Usage

```ruby
# require "json" # to use default json encoder (see below)

file_logger = Logger.new("/var/log/your_progname.log")
logger = Huyegger::Logger.new(file_logger)

# or better
# require "syslog/logger"
syslog_logger = Syslog::Logger.new("your_progname")
logger = Huyegger::Logger.new(syslog_logger)

# Write messages:
logger.info "log message"
# => { "level": "INFO", "message": "test" }
logger.info { "http.host" => '127.0.0.1', message: "log message" }
# => { "level": "INFO", "message": "test", "http.host": "127.0.0.1" }

# Store context for all log messages, it will be merged to resulting messages
logger.context("http.host" => '127.0.0.1')
logger.info('test')
# => { "level": "INFO", "message": "test", "http.host": "127.0.0.1" }

# Remove context
logger.purge_context!

# Configure json encoder
Huyegger.json_encoder = proc { |obj, *opts| Oj.dump(obj, *opts) }

# Default implementation of json_encoder uses `Object#to_json`,
# but you need to `require "json"` to use default json encoder.
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/mainameiz/huyegger.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
