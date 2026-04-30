# devproof-tinfoil-experiments

Test container deployments used by the [tinfoil-confidential-containers DEVPROOF report](https://github.com/amiller/devproof-audits-guide/blob/main/case-studies/tinfoil-confidential-containers/DEVPROOF-REPORT.md).

Each tag is a self-contained probe of one operator-controllable surface in the Tinfoil-Containers product:

| Tag | What it tests |
|---|---|
| `v0.1` | Env-echo: minimal `python:3.11-alpine` container that returns `os.environ` as JSON. Used to confirm whether `tinfoil container create --variable KEY=VALUE` actually reaches the container's runtime environment (vs being stored client-side only). |
| `v0.2` | Same image, but the YAML declares a string-form `env: - DECLARED_STRING_FORM` slot. Tests whether the platform's schema validator accepts string-form (vs map-form) entries, and what the attested config exposes about each slot. |

Deploy with:

```
tinfoil container create env-echo \
  --repo amiller/devproof-tinfoil-experiments \
  --tag v0.1 \
  --variable TEST_INJECTION=evil_exfil_value
```

Then `curl -k https://env-echo.<org>.containers.tinfoil.dev/` to see the env the container received.
