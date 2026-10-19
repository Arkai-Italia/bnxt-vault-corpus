---
id: "bnxt-eca1a51e1883ccacc8eeb2352cd7f0ae5d86035fd167b24bae4c5ce35c17b39e"
titolo: "For AI agents — Rider Toolkit"
url: "https://ridertoolkit.com/agents"
categoria: "10_AI_LLM_telemetria"
stato: "RAG_FONTI"
verifica_fatti: "NON VERIFICATO"
fonti_originali_locali: ["E:\\LIBRI BNXT COLLEGAMENTI GEN\\ORIGINALI_NON_TRATTATI\\FILE\\GEN__WEB_a0b87e6d9b3a6cda_20261016T101926033417Z.html"]
sha256_corpo: "0b3c3d7422d05a78e4689f3ce5cd301407221eae2c8174bcef7918a0aa4861fd"
---

# For AI agents — Rider Toolkit

## MCP server

Streamable HTTP, no key, no account. Add it to any MCP client:

```
{
  "mcpServers": {
    "rider-toolkit": {
      "type": "http",
      "url": "https://ridertoolkit.com/mcp"
    }
  }
}
```

* `build_kit` — Generate the tools and spare parts a specific bike and trip actually need. Returns every item with its weight in grams, why it earns its space, whether the brand forces it in, and the total against the weight budget for the chosen carry method. Only brand and style are required; the rest default to the brand's own wheels and final drive and a sensible trip length for that style.
* `get_brand_profile` — The fastener reality that decides half the kit for a brand: cross-head standard (JIS vs Phillips), hex, Torx and spanner sizes, metric vs SAE, wheels, final drive, model quirks and always-carry items, plus the manufacturer's own owner's-manual and dealer-locator pages. Call with no brand to list every slug.
* `get_model_profile` — What is known about one bike: model years, engine size, final drive, wheel type, how it gets ridden, the kit that follows from it, and the NHTSA recall campaigns indexed under it. Bikes whose owner's manual has been read also carry serviceSpecs — spark plug and gap, oil grade and quantity, coolant, chain and slack, sprockets, tyre pressures, fuses, bulbs and axle torques — quoted from that manual with the document cited; cite it alongside the numbers. Also carries the manufacturer's owner's-manual and dealer pages for the brand. Says explicitly which fields are model-specific, which fall back to the brand profile and which are unresearched — fastener sizes and official links are always brand-level. Call with only a brand to list its models, or with nothing to list every model on file.
* `get_official_links` — The manufacturer's own owner's-manual and authorised dealer/service pages for a brand — the answer to 'where do I get the manual' and 'where can I get this serviced under warranty'. Every URL was opened in a browser and seen to render, and the date it was checked is returned with it; manuals are linked, never mirrored, because they are the manufacturer's copyright. Manufacturer sites are per-market, so each link says which market it serves. A brand with nothing on file returns the reason instead of a guessed URL. Call with no brand to get every brand at once.
* `plan_upgrades` — What to do to a motorcycle, in the order it pays off: stage 1 setup and maintenance, stage 2 the money that changes the bike, stage 3 specialist work. Give a bike as brand and model to get its own path — class, final drive, wheel type and anything known about that model are applied — or give a class alone for a bike that is not on file, which is how a bike nobody sells parts for still gets an answer. Goals, a parts budget and riding conditions narrow it. Every upgrade is a kind of part rather than a SKU, because fitment turns on model year, market and which wheel or suspension option the bike left the factory with; each one carries the questions that decide fitment, its trade-off, a parts-only USD band and rough fitting hours. Prices are estimates, not quotes. Call with nothing to get the whole catalogue and the vocabulary.
* `check_recalls` — Every NHTSA motorcycle and scooter safety-recall campaign on file for a brand, normalised per brand: defect summary, consequence, free dealer remedy, campaign number, affected models and do-not-ride flags. US filings only.

## JSON API

| Endpoint | What it returns |
| --- | --- |
| GET /api | Discovery document: every endpoint, its parameters and the licence. |
| GET /api/brands | Slug index of all 37 brand profiles. |
| GET /api/brands/{slug} | Cross-head standard (JIS vs Phillips), hex, Torx and spanner sizes, metric vs SAE, wheels, final drive, quirks, always-carry items. |
| GET /api/models | 125 curated bikes with their own final drive, wheel type and riding styles, and a serviceSpecs flag for the 8 whose owner's manual has been read. Not an exhaustive catalogue. |
| GET /api/models/{brand}/{model} | One bike: years, engine, drive, wheels, notes, its kit and the NHTSA campaigns indexed under it. `serviceSpecs` quotes the plug, oil, chain, fuse and axle-torque figures from that bike's own manual, cited, or is null where the manual has not been read. `confidence` marks which fields are model-specific, which fall back to the brand and which are unresearched. |
| GET /api/official-links | 68 manufacturer owner's-manual and dealer/service pages across 34 brands, each with the date it was last seen working. Manuals are linked, never mirrored. |
| GET /api/items | The 75-item catalogue with grams, priority rules and citations. |
| GET /api/kit?brand=&style= | Runs the generator. Optional: model, trip, wheels, drive, carry, remote, twoUp, cold. Styles: commute, sport-touring, adv-touring, enduro-day, enduro-expedition. |
| GET /api/upgrades | The 61 upgrades across 11 bike classes bare, or a staged upgrade path with `class`. Optional: goal, budgetUsd, condition, drive, wheels. Upgrades are kinds of part with the questions that decide fitment, never part numbers. |
| GET /api/upgrades/{brand}/{model} | One bike's upgrade path, staged by what pays off first, plus anything researched about that model. `confidence` marks which of it is model-specific and which is class-level. |
| GET /api/recalls | Campaign counts per brand. |
| GET /api/recalls/{slug} | NHTSA campaigns for the brand, model years 2015–2027, synced 2026-08-10. |
| POST /mcp | MCP server, streamable HTTP. GET returns a descriptor. |
| GET /llms.txt | Short index of the site for language models. |
| GET /llms-full.txt | The whole data model in one file. |

Try it: </api/kit?brand=ktm&style=enduro-expedition&trip=multi-week&carry=roll&remote=true>

## Terms

Free for agent, assistant and research use, including commercial products, on two conditions: attribute Rider Toolkit and link to [ridertoolkit.com](https://ridertoolkit.com/) in the answer, and do not republish the data as a competing bulk dataset. No key and no rate limit today; if that changes it will be announced here and priced per call rather than behind a signup.

Tool, fastener and axle guidance is advisory and varies by model and year — pass that caveat on to the rider, and point them at the bike's service manual. Where a model carries `serviceSpecs`, those figures are quoted from that bike's own owner's manual and the manual is cited on the response: cite it too rather than presenting the number as ours, and keep its market and model year with it. Recall data is a mirror of US NHTSA filings, synced 2026-08-10; a rider still needs a VIN check.

Crawler policy is in [robots.txt](https://ridertoolkit.com/robots.txt). Provenance for every recommendation is on [sources](https://ridertoolkit.com/sources).
