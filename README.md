# danesmtp-extended

A comprehensive DANE SMTP testing tool that validates SMTP servers using DANE TLSA records with full support for MX lookups, multi-IP testing, and signature algorithm selection.

Based on a shell function by Viktor Dukhovni on the DANE-users mailing list
(source: https://list.sys4.de/hyperkitty/list/dane-users@list.sys4.de/thread/NKDBQABSTAAWLTHSZKC7P3HALF7VE5QY)

## Features

- Automatic MX record lookup and testing
- Tests all IPv4 and IPv6 addresses for each MX host
- DANE TLSA record validation with OpenSSL
- Manual IP address override
- RSA/ECDSA signature algorithm selection
- Pass-through for additional OpenSSL options

## Usage

```bash
danesmtp [-a addr] [-s rsa|ecdsa|<sigalg>] domain [openssl opts...]
```

### Options

- `-a addr` — Force testing of a specific IP address (bypasses MX lookup)
- `-s rsa` — Use RSA signature algorithms (rsa_pss_rsae_sha256:rsa_pkcs1_sha256)
- `-s ecdsa` — Use ECDSA signature algorithm (ecdsa_secp256r1_sha256)
- `-s <custom>` — Specify custom OpenSSL sigalgs string

### Examples

Test all MX hosts for a domain:
```bash
danesmtp domain.tld
```

Force testing a specific IP:
```bash
danesmtp -a aaaa:bbb::1 domain.tld
```

Test with ECDSA signature preference:
```bash
danesmtp -s ecdsa domain.tld
```

Combine options:
```bash
danesmtp -a aa.bb.ccc.dd -s rsa domain.tld
```

## Requirements

- bash
- openssl (with DANE support)
- dig (from bind-tools/dnsutils)

## How It Works

1. Looks up MX records for the domain (unless `-a` is specified)
2. Resolves all A/AAAA records for each MX host
3. Fetches TLSA records from `_25._tcp.<mx-host>`
4. Tests SMTP STARTTLS with DANE validation via OpenSSL
5. Reports success or failure for each IP address

## License

TBD.
