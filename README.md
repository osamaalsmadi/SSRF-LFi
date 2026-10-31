# SSRF & File Disclosure Lab

Educational server-side request and local file disclosure lab with mitigation notes.

**Python · Docker**

## Learning focus

Unvalidated resource URLs, server-side requests, and local file disclosure.

## Repository guide

| Resource | Location |
| --- | --- |
| Assessment | [cyber1x2.1.00/pro_02/Readme.md](cyber1x2.1.00/pro_02/Readme.md) |
| Remediation | [cyber1x2.1.00/pro_02/Fix.md](cyber1x2.1.00/pro_02/Fix.md) |
| Compose configuration | [cyber1x2.1.00/pro_02/docker-compose.yml](cyber1x2.1.00/pro_02/docker-compose.yml) |

## Inspect the lab

1. Read the assessment and remediation notes together.
2. Inspect the Dockerfile and Compose configuration before running the application.
3. Use an isolated local environment for authorized training. The supplied Compose file publishes a host port; it is not a production deployment configuration.
4. Record observations and compare them with the mitigation notes.

## Setup requirements

Docker with Compose is required for the supplied container configuration. The lab directory is `cyber1x2.1.00/pro_02`.

Container builds and vulnerability demonstrations have not been revalidated as part of this documentation update.

The Dockerfile uses Python 2.7. Treat this as a legacy lab; dependency installation may require maintenance before it runs on a current system.

---

Part of [Osama Alsmadi’s project portfolio](https://github.com/osamaalsmadi).
