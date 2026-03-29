# Connect to API

Use `Mass.set` to configure your SDK client before calling any endpoint.

```ruby
require 'uri'
require 'mass-client'

Mass.set(
	api_key: '<su api key>',
	api_url: 'https://connectionsphere.com',
	api_port: 443,
	backtrace: true,
)
```

Use this when you already know your master API host and port.

