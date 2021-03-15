# Rails Puma SSL Bind Issue Example

This repo is a demonstration of an issue I discovered concerning the behavior
or `rails server`, Puma, and Puma's `ssl_bind` configuration option.

## Reproduce

To reproduce the issue:

First, run `rails s`.

```
$ rails s                
=> Booting Puma
=> Rails 6.1.3 application starting in development 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.2.2 (ruby 2.7.2-p137) ("Fettisdagsbulle")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 2169
* Listening on http://127.0.0.1:3000
* Listening on http://[::1]:3000
* Listening on ssl://0.0.0.0:3001?cert=/Users/chris/local-certs/localhost.pem&key=/Users/chris/local-certs/localhost-key.pem&verify_mode=none
Use Ctrl-C to stop
^C- Gracefully stopping, waiting for requests to finish
=== puma shutdown: 2021-03-15 11:15:34 -0700 ===
- Goodbye!
Exiting
```

Notice that when Rails starts the server, both a normal HTTP binding and a SSL
binding are created.

Stop the server.

Next, run the server with `rails s -p 5000` to specify an alternative port.

```
$ rails s -p 5000
=> Booting Puma
=> Rails 6.1.3 application starting in development 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.2.2 (ruby 2.7.2-p137) ("Fettisdagsbulle")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 2212
* Listening on http://127.0.0.1:5000
* Listening on http://[::1]:5000
Use Ctrl-C to stop
^C- Gracefully stopping, waiting for requests to finish
=== puma shutdown: 2021-03-15 11:15:46 -0700 ===
- Goodbye!
Exiting
```

Notice that when Rails starts the server, only the HTTP binding is created. The
SSL binding is not created.
