# slsa-demo
A demo of SLSA functionality

## Level 1

> **The build process must be fully scripted/automated and generate
> provenance.** Provenance is metadata about how an artifact was built,
> including the build process, top-level source, and dependencies. Knowing the
> provenance allows software consumers to make risk-based security decisions.
> Provenance at SLSA 1 does not protect against tampering, but it offers a basic
> level of code source identification and can aid in vulnerability management.

Source: [https://slsa.dev/spec/v0.1/levels#detailed-explanation](https://slsa.dev/spec/v0.1/levels#detailed-explanation)

Detailed requirements:

### Build
- [Scripted build](https://slsa.dev/spec/v0.1/requirements#scripted-build)

### Provenance
- [Available](https://slsa.dev/spec/v0.1/requirements#available)

Demo: Create a new tag, e.g., `git tag -a v0.1.0 -m "v0.1.0"` and push it to
GitHub. This will trigger a GitHub Action that will build the source and
generate a provenance file. The provenance file will be uploaded together with
the resulting binary.

## Level 2

> **Requires using version control and a hosted build service that generates
> authenticated provenance.** These additional requirements give the software
> consumer greater confidence in the origin of the software. At this level, the
> provenance prevents tampering to the extent that the build service is trusted.
> SLSA 2 also provides an easy upgrade path to SLSA 3.

Detailed requirements:

Same as 1, plus:

### Source

- [Version controlled](https://slsa.dev/spec/v0.1/requirements#version-controlled)

### Build

- [Build service](https://slsa.dev/spec/v0.1/requirements#build-service)

### Provenance

- [Authenticated](https://slsa.dev/spec/v0.1/requirements#authenticated) - The
  provenance is signed by the build service in a way that allows the consumer to
  verify that it has not been tampered with.
- [Service generated](https://slsa.dev/spec/v0.1/requirements#service-generated)
  - The provenance is generated by the build service, not directly by e.g. a
    developer.

Demo: Create a new tag, e.g., `git tag -a v0.2.0 -m "v0.2.0"` and push it to
GitHub. Then you can download the provenance file and verify it using the
signature like so: `gpg --verify build.provenance.asc build.provenance`.

## Level 3

> **The source and build platforms meet specific standards to guarantee the
> auditability of the source and the integrity of the provenance respectively.**
> We envision an accreditation process whereby auditors certify that platforms
> meet the requirements, which consumers can then rely on. SLSA 3 provides much
> stronger protections against tampering than earlier levels by preventing
> specific classes of threats, such as cross-build contamination.

Detailed requirements:

Same as 2, plus:

### Source

- [Retained for 18 months](https://slsa.dev/spec/v0.1/requirements#retained-indefinitely)

### Build

- [Build as code](https://slsa.dev/spec/v0.1/requirements#build-as-code)
    - For instance, a build.yaml file specifying the build process. Must be
      verifyably derived from a version controlled document.
- [Ephemeral environment](https://slsa.dev/spec/v0.1/requirements#ephemeral-environment)
    - The build is ran in a stateless environment, e.g., a docker container.
- [Isolated](https://slsa.dev/spec/v0.1/requirements#isolated)
    - Build instances must not affect each other.
    - Build containers must not access to secrets.
    - Etc (see link)

### Provenance

- [Non-falsifiable](https://slsa.dev/spec/v0.1/requirements#non-falsifiable)
    - Higher demands on how the secret keys are stored, mainly.


