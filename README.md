# Aqueduct — Compliance Remediation Content for RHEL

**Aqueduct** was an open-source project that provided automated security hardening scripts and configuration management content for Red Hat Enterprise Linux (RHEL) systems. Originally hosted on [Fedora Hosted](https://fedorahosted.org/aqueduct/), the project delivered Bash scripts, Puppet manifests, and Ansible roles to remediate findings from DISA STIGs, CIS Benchmarks, and other compliance frameworks.

Active from **2011–2014**, Aqueduct became one of the earliest community-driven efforts to automate STIG compliance at scale — predating much of the tooling the industry now takes for granted.

> **Note:** This repository is archived for historical purposes. The compliance landscape has since evolved toward projects like [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content) (formerly SCAP Security Guide) and tools like [OpenSCAP](https://www.open-scap.org/). Aqueduct's work contributed to and influenced that evolution.

---

## What Aqueduct Did

Aqueduct aimed to simplify RHEL compliance by providing ready-to-run remediation content organized by regulatory framework and RHEL version. The project covered:

- **DISA STIG** — Defense Information Systems Agency Security Technical Implementation Guides for RHEL 5 and RHEL 6
- **CIS Benchmarks** — Center for Internet Security hardening guidelines
- **NISPOM** — National Industrial Security Program Operating Manual controls
- **DHS** — Department of Homeland Security baseline configurations
- **PCI DSS** — Payment Card Industry Data Security Standard controls

Each STIG finding mapped to an individual Bash script (e.g., `GEN000480.sh` for RHEL 5, `RHEL-06-000011.sh` for RHEL 6), making it straightforward to audit, customize, or integrate into larger automation pipelines.

## Repository Structure

```
├── compliance/
│   ├── ansible/
│   │   └── ssg/
│   │       └── rhel-6/            # Ansible role for RHEL 6 SSG hardening
│   │           └── roles/harden/  # tasks, templates, handlers, vars
│   ├── bash/
│   │   ├── stig/
│   │   │   ├── rhel-5/            # ~493 scripts (GEN-series STIG IDs)
│   │   │   │   ├── prod/          # Production-ready remediation scripts
│   │   │   │   ├── dev/           # In-development scripts
│   │   │   │   └── manual/        # Checks requiring human review
│   │   │   └── rhel-6/            # ~57 scripts (RHEL-06-series STIG IDs)
│   │   │       ├── prod/
│   │   │       ├── dev/
│   │   │       └── manual-check/
│   │   └── ssg/                   # SCAP Security Guide integration tooling
│   └── puppet/
│       └── stig/
│           └── rhel-6/            # Puppet modules for RHEL 6 STIG
│               ├── manifests/
│               └── modules/       # accounts, aide, audit, banner, cron,
│                                  # fstab, iptables, kernel, named, networking,
│                                  # nfs, ntp, packages, pam, postfix, puppet,
│                                  # rsyslog, services, ssh, sudo, sysctl, yum
├── etc/aqueduct/profiles/         # Configuration profiles (DISA RHEL 5/6)
├── aqueduct.spec                  # RPM spec for packaging as system RPMs
├── docs/license.txt               # GPL v2
└── README                         # Original project README
```

The project shipped **1,366 files** in total across Bash, Puppet, and Ansible, with the bulk being individual per-finding remediation scripts.

## Impact and Prior Art

### The Problem Aqueduct Solved

In 2011, automating DISA STIG compliance on Linux was essentially a do-it-yourself affair. DISA published STIGs as PDF checklists — hundreds of individual security requirements — and every organization was left to translate those requirements into scripts on their own. Most did it behind closed doors, never sharing the work. The result was thousands of engineers across the DoD, federal agencies, and defense contractors all solving the same problem independently, poorly, and repeatedly.

There was no open-source remediation content to speak of. No shared library of scripts. No way to say "here's how finding GEN000480 should be fixed" and have the community vet, improve, and maintain that fix over time. Aqueduct changed that.

### What Aqueduct Pioneered

Aqueduct was among the first projects to take a **compliance-as-code** approach to STIG remediation — before that term was in common use. The core design decisions were deliberate and influential:

**One script per finding.** Each STIG requirement mapped to a discrete, auditable Bash script. This made it possible to run individual checks, customize specific remediations without touching others, and trace every system change back to a specific compliance requirement. The 493 RHEL 5 scripts and 57 RHEL 6 scripts represented one of the most comprehensive open-source mappings of STIG requirements to executable remediation available at the time.

**Multi-framework coverage.** Aqueduct didn't stop at STIGs. It provided content for five compliance frameworks — DISA STIG, CIS Benchmarks, NISPOM, DHS baselines, and PCI DSS — organized by framework and OS version. This breadth was unusual for a community project and reflected real-world demand: organizations rarely faced just one compliance requirement.

**Bash-first, by design.** While Puppet manifests were developed for RHEL 6, the project deliberately prioritized Bash. This was a community-driven decision — Bash was portable, required no agent infrastructure, had no licensing concerns, and could be understood and modified by any sysadmin. This philosophy proved prescient: the compliance automation space largely followed the same pattern for years afterward.

**Multi-tool automation.** Over time, Aqueduct expanded beyond Bash to include Puppet modules for RHEL 6 STIG compliance and an Ansible role for RHEL 6 SSG hardening — reflecting the project's pragmatic approach of meeting administrators where they were, regardless of their preferred automation tooling.

**Enterprise packaging.** Aqueduct shipped with an RPM spec file (`aqueduct.spec`) with separate sub-packages for each framework (CIS, DISA, DHS), designed for deployment through Red Hat Satellite and enterprise package management. This wasn't a collection of loose scripts — it was built for production use at scale.

### Community Adoption

Aqueduct's reach extended well beyond its Fedora Hosted repository. People were pulling the content, wrapping it into their own automation pipelines, building STIG-compliant installation disks, and modifying RHEL ISOs for DISA compliance using Aqueduct as the foundation.

**Linux Journal (August 2014).** Mark Dotson's article *"Security Hardening with Ansible"* in Linux Journal Issue 242 didn't just mention Aqueduct — it built an entire production workflow around it. Dotson demonstrated hardening a ten-node HPC cluster to DISA STIG standards by combining Aqueduct's Bash scripts with Ansible playbooks, calling out the project by name and walking readers through cloning the repository, deploying the scripts, and validating the results. The article was subsequently indexed in the [ACM Digital Library](https://dl.acm.org/doi/abs/10.5555/2642922.2642926), giving the work academic citation permanence.

  - [Linux Journal — Security Hardening with Ansible](https://www.linuxjournal.com/content/security-hardening-ansible)

**Military Open Source Software (Mil-OSS).** Aqueduct was discussed in the Mil-OSS Google Group — a community of defense and government practitioners focused on open-source adoption in military contexts. A 2011 thread on "RHEL STIG via Puppet" drew nearly 2,000 views and explored using Aqueduct's Puppet manifests for STIG compliance. This wasn't a casual audience — these were DoD engineers evaluating the project for operational use.

  - [Mil-OSS Discussion — RHEL STIG via Puppet](https://groups.google.com/g/mil-oss/c/e2-mrDjLAeo/m/ShalxS3bLPAJ)

**SCAP Security Guide and the ComplianceAsCode Lineage.** Aqueduct's remediation content was actively discussed on the SCAP Security Guide (SSG) mailing list, where the community debated how to align Aqueduct's Bash remediation scripts with the emerging SCAP/OpenSCAP ecosystem. Aqueduct and SSG were running as parallel efforts during this period, and the conversations about convergence, content sharing, and the relationship between standalone remediation scripts and SCAP-integrated profiles helped shape the direction of what eventually became [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content) — now the dominant open-source compliance content project.

  - [SSG Mailing List Discussion](https://lists.pagure.io/archives/list/scap-security-guide@lists.fedorahosted.org/message/O6SPJ7HNZ65LS3GRKWMWXTKX2S2LIROI/)

**Downstream Forks and Reuse.** Organizations forked and repackaged Aqueduct's content for their own compliance pipelines. Documented examples include [atomicturtle/t-stig](https://github.com/atomicturtle/t-stig), which bundled Aqueduct v0.4 scripts alongside custom NISPOM content, and [Berico-Technologies/aqueduct](https://github.com/Berico-Technologies/aqueduct), a fork used within a defense technology firm.

### Legacy

The compliance-as-code ecosystem that exists today — ComplianceAsCode, ansible-lockdown, OpenSCAP remediation content, vendor hardening tools — didn't emerge from nothing. Projects like Aqueduct demonstrated that open-source, community-maintained compliance content was viable, that organizations would adopt it, and that the one-script-per-finding model was the right level of granularity. Much of the tooling that followed carried forward patterns that Aqueduct helped establish.

## Contributors

- **Vincent C. Passaro** — Project creator, lead, and primary author. Wrote the majority of the Bash remediation scripts across both RHEL 5 and RHEL 6, the Puppet manifests, and the project's overall architecture. At the time, Senior Security Architect at Fotis Networks and principal at Buddha Labs LLC.
- **Shannon Mitchell** ([Fusion Technology LLC](http://www.fusiontechnology-llc.com)) — Contributed to a subset of the RHEL 5 STIG Bash scripts and the RPM packaging (aqueduct.spec) from 2011–2012.

## License

This project is licensed under the **GNU General Public License v2** (GPL-2.0). See [docs/license.txt](docs/license.txt) for the full text.

```
Copyright (C) 2011–2013 Vincent C. Passaro (vincent.passaro@gmail.com)
```

## See Also

- [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content) — The spiritual successor to projects like Aqueduct; now the primary open-source compliance content project
- [OpenSCAP](https://www.open-scap.org/) — SCAP scanning and remediation toolkit
- [ansible-lockdown](https://github.com/ansible-lockdown) — Ansible roles for STIG/CIS compliance (carries forward the approach Aqueduct pioneered)
- [DISA STIG Library](https://public.cyber.mil/stigs/) — Current STIG publications from DISA
