# eval-personality

**Personality**

An evaluation suite consisting of multiple personality tests that can be applied to LLMs.
Its primary goals are twofold:
  1. Assess a model's default personality: the persona it naturally exhibits without specific
prompting.
  2. Evaluate whether a model can embody a specified persona**: how effectively it adopts
certain personality traits when prompted or guided.

## At a glance

| | |
|---|---|
| Upstream | [`src/inspect_evals/personality`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/personality) |
| Group | Personality |
| Total samples | 8,044 |
| Execution class | `plain` |
| Cost class | `medium` |
| Flags | no sandbox, no network |
| Tags | — |

### Tasks

| Task | Samples |
|---|---|
| `personality_BFI` | 44 |
| `personality_TRAIT` | 8,000 |

### External assets

- `direct_url` — `https://raw.githubusercontent.com/guiem/personality-tests/{SHA}/` (pinned)
- `huggingface` — `mirlab/TRAIT` (pinned)

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/personality_BFI \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
