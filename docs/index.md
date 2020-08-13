# Lastpass Provider

The Lastpass provider is used to read, manage, or destroy secrets inside Lastpass. Goodbye secret .tfvars files 👋

Make sure to have [lastpass-cli](https://github.com/lastpass/lastpass-cli) in your current `$PATH`. 

-> Set `LPASS_AGENT_TIMEOUT=86400` inside your `~/.lpass/env` to stay logged in for 24h. Set to `0` to never logout (less secure).

-> Set `LASTPASS_USER` and `LASTPASS_PASSWORD` env variables to avoid writing login to your .tf-files.

## Example Usage

```hcl
provider "lastpass" {
    version = "0.5.0"
    username = "user@example.com"
    password = file("${path.module}/.lpass")
} 

# secret with random generated password
resource "lastpass_secret" "mylogin" {
    name = "My service"
    username = "foobar"
    generate {
        length = 24
        use_symbols = false
    }
}

resource "lastpass_secret" "mysecret" {
    name = "My site"
    username = "foobar"
    password = file("${path.module}/secret")
    url = "https://example.com"
    note = <<EOF
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam sed elit nec orci
cursus rhoncus. Morbi lacus turpis, volutpat in lobortis vel, mattis nec magna.
Cras gravida libero vitae nisl iaculis ultrices. Fusce odio ligula, pharetra ac
viverra semper, consequat quis risus.
EOF
}
```

## Argument Reference

* `username` - (Required) 
  * Can be set via `LASTPASS_USER` env variable.
  * Can be set to empty string for manual lpass login.
  * With 2FA enabled you will need to login manually with `--trust` at least once.
* `password` - (Required)
  * Can be set via `LASTPASS_PASSWORD` env variable.
  * Can be set to empty string for manual lpass login.
