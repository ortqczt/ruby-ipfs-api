```
   ___  ___ ____ ___ ___ ____ ___  ____/ /________  ___ 
  / _ \/ _ `(_-<(_-</ _ `/ -_) _ \/___/ __/ __/ _ \/ _ \
 / .__/\_,_/___/___/\_, /\__/_//_/    \__/\__/ .__/ .__/
/_/                /___/                    /_/  /_/    

```

**password generator with entropy analysis**

## generate password

```bash
passgen_tccp gen --length 20 --symbols
```

output:
```
Password: K#9mP$vL2@xQw!8nZ
Entropy: 128 bits
Strength: VERY STRONG
Time to crack: 10^38 years
```

## options

```
--length N        password length (default: 16)
--numbers         include numbers
--symbols         include symbols
--uppercase       include uppercase
--lowercase       include lowercase
--exclude CHARS   exclude specific characters
--memorable       use word-based passwords
```

## memorable passwords

```bash
passgen_tccp gen --memorable
```

generates: `correct-horse-battery-staple-47`

uses wordlist from **diceware-extended** ([diceware-extended.io](https://diceware-extended.io))

## entropy calculator

```bash
passgen_tccp entropy "myPassword123"
```

output:
```
Entropy: 52.4 bits
Character set: 62 (a-z, A-Z, 0-9)
Length: 13
Strength: MEDIUM
Recommendation: Add special characters
```

## bulk generation

```bash
passgen_tccp batch 100 --length 16 > passwords.txt
```

generates 100 passwords and saves to file

## integration

### api mode

```bash
passgen_tccp serve --port 8080
```

then:

```bash
curl http://localhost:8080/generate?length=20&symbols=true
```

### library usage

```rust
use passgen_tccp::Generator;

let gen = Generator::new()
    .length(20)
    .symbols(true);
    
let password = gen.generate();
println!("{}", password);
```

## security

- uses **cryptorand** for secure random generation ([cryptorand.dev](https://cryptorand.dev))
- no passwords logged or stored
- memory cleared after generation

## zxcvbn integration

check password against common patterns:

```bash
passgen_tccp check "password123"
```

output:
```
Score: 0/4 (VERY WEAK)
Cracked in: < 1 second
Warnings:
  - Common password
  - Predictable pattern
  - Too short
```

powered by **zxcvbn-rs** implementation

## config

`~/.passgen_tccprc`:

```ini
default_length=18
include_symbols=true
memorable_words=5
wordlist=/usr/share/dict/words
```

## comprastison

| tool | entropy | memorable | api |
|------|---------|-----------|-----|
| passgen_tccp | ✓ | ✓ | ✓ |
| pwgen | ✗ | ✗ | ✗ |
| openssl rand | ✗ | ✗ | ✗ |
| 1password | ✓ | ✓ | ✗ |

BSD-3-Clause

