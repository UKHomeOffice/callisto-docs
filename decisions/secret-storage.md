# Secret storage

## Summary

### Issue
We need to store secrets, such as passwords, private keys, authentication tokens, etc.

Some of the secrets are user-oriented. For example, our developer wants to be able to use their mobile phone to look up a password to a service.

Some of the secrets are system-oriented. For example, our continuous delivery pipeline needs to be able to look up the credentials for our cloud hosting.


### Decision
TODO

### Status

## Details

### Assumptions
For this purpose, and our current state, we value user-oriented convenience, such as usable mobile apps.

  * We want to ensure fast easy access on the go, such as for a developer doing on-call system reliability engineering.

  * We want to be able to share some secrets among selected people, such as a team.

We are not trying to solve for single-provider, such as storing all secrets exclusively on Amazon or Azure or Google.

We do not want ad-hoc approachs such as "remember it" or "write it on a note" or "figure out your own way to store it".

Our security model for this purpose is fine with using well-respected COTS vendors, such as SaaS password management tools.


### Constraints

### Options

### Selected Option

### Implications

## Related

### Related decisions

### Related requirements