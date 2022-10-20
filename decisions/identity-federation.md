
# Identity federation

## Summary

### Issue
We want to federate identity to avoid Callisto having to manage end user identities and also to leverage existing systems that form part of the Home Office enterprise. By federating we support the important concept of a single source of truth for identity across the Home Office

### Decision
Azure Active Directory (POISE)

### Status
Decided

## Details

### Assumptions
- The availability (up-time) of Azure AD or Oracle IDCS are assumed to be greater than that of Callisto

### Constraints

### Options

 1. Oracle IDCS (Metis) 
 2. Azure AD (POISE)

### Selected Option
Azure AD (POISE)

- The distinction between Azure AD and Oracle IDCS from an end user perspective is minimal (see implication 1 below) 
- Federation with Azure AD from a system running within ACP is well documented and is a known quantity

### Implications
1. Callisto will forgo the ability to have a launch area from within Metis

## Notes
- Azure AD and Oracle IDCS (Metis) are effectively equivalent in the user base that they manage as Oracle IDCS syncs with Azure AD in this way Callisto does not loose user reach by adopting Azure AD.

Oracle IDCS keeps Azure AD up to date with joiners, movers and leavers via 1191 and 1192 interfaces. In effect a user accesses Metis through their POISE identity.


