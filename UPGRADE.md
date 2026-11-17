# Upgrading
## Upgrading from version 3.x to version 4

### Requirements
- Composer version >=2.8 is required. If you have Composer installed as (dev) dependency in your project, then update if necessary. Otherwise make sure your composer binary has a compatible version.

### Update allow list
Old allow list configuration in `grumphp.yml`
```
parameters:
  securitychecker.allow_list:
    - CVE-2002-0121 # Will be resolved in PIMBBBPROJ-123 (upgrade to 10)
    - CVE-2002-0128 # Will be resolved in PIMBBBPROJ-123 (upgrade to 10)
```

New allow list configuration in `composer.json`
- parameter `securitychecker.allow_list` is removed
- `ignore-id` can either be a `simple list` or `with reasons`
#### For composer versions between 2.8 and 2.10:
- configuration is done with `config.audit.ignore` in `composer.json`

```
# simple list
"config": {
    "audit": {
        "ignore": ["CVE-1234"]
    }
}
```
```
# with reason
"config": {
    "audit": {
        "ignore": {
            "CVE-1234": "Will be resolved in PIMBBBPROJ-123 (upgrade to 10)"
        }
    }
}
```

To ignore abandoned packages, set the `config.audit.abandoned` value in `composer.json`
```
"config": {
    "audit": {
        "abandoned": "ignore"
    }
}
```
#### For composer versions >= 2.10:
- configuration is done with `config.policy.advisories.ignore-id` in composer.json`
```
# simple list
"config": {
    "policy": {
        "advisories": {
            "ignore-id": ["CVE-1234", "GHSA-xx"]
        }
    }
}
```
```
# with reason
"config": {
    "policy": {
        "advisories": {
            "ignore-id": {
                "CVE-1234": "Will be resolved in PIMBBBPROJ-123 (upgrade to 10)",
                "GHSA-xx": "Will be resolved in PIMBBBPROJ-123 (upgrade to 10)"
            }
        }
    }
}
```

To ignore abandoned packages, set the `config.policy.abandoned.ignore` value in `composer.json`
```
"config": {
    "policy": {
        "abandoned": {
            "ignore": {
                "vendor/*": "Scheduled for replacement next quarter.",
                "vendor/legacy": {
                    "on-block": false,
                    "reason": "Allow in updates but still report."
                }
            }
        }
    }
}
```