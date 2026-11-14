# Datacosmos fork workflow

This fork publishes qualified Gas City packs for Datacosmos from `dc-use`.
The upstream project owns the pack code; the fork carries only changes with
a current downstream consumer.

| Branch | Purpose |
| --- | --- |
| `main` | Exact mirror of the official `main`. The sync job advances it by fast-forward only. A divergent mirror fails the job without publishing. |
| `dc-use` | Reviewed integration and release lane. Merge the mirrored upstream SHA before qualifying a release. |
| `contrib/<subject>` | One generally useful change based on official `main`. Open its PR directly against the upstream repository. |
| `adopt/<subject>` | An upstream PR or other third-party change awaiting integration. Record its source SHA and preserve authorship. |
| `super/<integration>` | A dedicated working branch for a cross-component integration or large change. Integrate only qualified changes into `dc-use`; never release from this branch. |

Before retaining a local change, reproduce the consumer's need on upstream
`main` and check existing upstream PRs. Use the upstream implementation when
it covers that need. Remove the downstream copy once the official history
carries it. For a remaining generic gap, submit the smallest fix upstream
and link its PR in the fork integration PR.

The sync workflow belongs on the default branch `dc-use` so scheduled runs
can update the mirror without putting workflow files on `main`. A sync that
advances the mirror does not release anything: integrate it into `dc-use`
through one reviewed PR with the native checks.

Publish stable fork deliveries from the reviewed, green `dc-use` commit as
`<upstream-version>-dc.N`, starting at `N=1` for each upstream version. Record
the official upstream SHA in the tag annotation and the qualified pack
content hashes in the release. The native registry keeps its numeric version
format; the city pins the commit behind the `-dc.N` tag.
