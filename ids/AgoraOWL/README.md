# 🏛️ AgoraOWL

> **Persistent identifier (PID) home** for the **AgoraOWL** ontology, its modular SKOS
> vocabularies and its distribution artefacts.

`.htaccess` in this directory resolves <https://w3id.org/AgoraOWL> and everything
underneath it. Targets are served from GitHub Pages:
**`https://khaosresearch.github.io/AgoraOWL/`**

| | |
| --- | --- |
| 🌐 **PID** | <https://w3id.org/AgoraOWL> |
| 📦 **Source** | [KhaosResearch/AgoraOWL](https://github.com/KhaosResearch/AgoraOWL) |
| 🚀 **Hosting** | GitHub Pages (`gh-pages` branch) |
| 🧭 **Scope** | Data spaces · aligned with **IDSA** · connected to **BIGOWL** |
| 📜 **Licence** | Apache-2.0 |

---

## 📖 Ontology

The base PID resolves to the **latest** release. Any published version is reachable
by appending it.

| PID | `Accept` | ➡️ Resolves to |
| --- | --- | --- |
| `https://w3id.org/AgoraOWL` | `text/html` | `latest/index-en.html` |
| `https://w3id.org/AgoraOWL` | `text/turtle` | `latest/ontology.ttl` |
| `https://w3id.org/AgoraOWL` | `application/rdf+xml` | `latest/ontology.owl` |
| `https://w3id.org/AgoraOWL` | `application/n-triples` | `latest/ontology.nt` |
| `https://w3id.org/AgoraOWL` | `application/ld+json` | `latest/ontology.jsonld` |
| `https://w3id.org/AgoraOWL/1.3.0` | *any of the above* | the same, pinned to that version |

```bash
curl -L -H "Accept: text/turtle" https://w3id.org/AgoraOWL
```

---

## 🏷️ Terms

```
https://w3id.org/AgoraOWL/DataApp
https://w3id.org/AgoraOWL/hasFieldMapping
```

| `Accept` | ➡️ Resolves to |
| --- | --- |
| `text/html` | `latest/index-en.html#DataApp` — the term's section in the documentation |
| `text/turtle` · `application/rdf+xml` · `application/n-triples` · `application/ld+json` | `latest/ontology.*` — the document that defines the term |

> ✨ **New.** The RDF branch did not exist: a term IRI returned `404` for every RDF
> media type while the browser branch worked, so terms could not be dereferenced
> by a machine.

---

## 📚 Vocabularies

Concept IRIs are **version independent**. A concept keeps the same IRI across
releases; the versioned path is only a document location.

```
https://w3id.org/AgoraOWL/vocabularies/observed-properties#ndvi   ← identity
https://w3id.org/AgoraOWL/1.3.0/vocabularies/observed-properties  ← that release's document
```

| PID | `Accept` | ➡️ Resolves to |
| --- | --- | --- |
| `/vocabularies/` | `text/html` | `latest/vocabularies/index.html` |
| `/vocabularies/observed-properties` | `text/html` | `latest/vocabularies/observed-properties.html` |
| `/vocabularies/observed-properties` | `text/turtle` | `latest/vocabularies/observed-properties.ttl` |
| `/1.3.0/vocabularies/siex` | `text/html` | `1.3.0/vocabularies/siex.html` |
| `/vocabularies/siex.ttl` | *any* | `latest/vocabularies/siex.ttl` |

### 🔗 Why HTML for browsers

A **fragment is never sent to the server**, so a redirect cannot resolve `#ndvi`
by itself — the landing page has to. Browsers therefore receive the generated
documentation, which carries one anchor per concept:

```
https://w3id.org/AgoraOWL/vocabularies/observed-properties#ndvi
  │
  ├─ 1. browser drops the fragment, requests /vocabularies/observed-properties
  ├─ 2. w3id → 303 → …/latest/vocabularies/observed-properties.html
  └─ 3. browser re-applies the fragment (RFC 7231 §7.1.2) → jumps to the NDVI section ✅
```

RDF clients still get the complete Turtle document. 🤖

> 🧩 SIEX ships **82 240** concepts, so its page resolves the fragment against a
> sharded client-side index instead of inlining every concept.

---

## 🧰 Distribution artefacts

| PID | ➡️ Resolves to | Consumed by |
| --- | --- | --- |
| `/context.jsonld` | `latest/context.jsonld` | 🔌 Eclipse EDC 0.18+ — `edc.dataspace.profiles.*.jsonld.context.urls` |
| `/alignment.ttl` | `latest/alignment.ttl` | 🧷 optional third-party alignment module |
| `/1.3.0/context.jsonld` | that release's copy | |

> ✨ **New.** Both returned `404`, so an EDC connector could not fetch the context
> it needs to compact Dataspace Protocol messages.

---

## ✅ How these rules were verified

These rules were **not** guessed. The AgoraOWL repository ships a simulator that
runs this exact file inside Apache 2.4 configured the way w3id.org configures it
(per-directory `.htaccess`, `mod_rewrite`, `AllowOverride All`) and asserts the
`Location` header for every path and `Accept` combination:

```bash
python3 w3id/simulation/verify_redirects.py                  # these rules
python3 w3id/simulation/verify_redirects.py --baseline       # the rules being replaced
python3 w3id/simulation/verify_redirects.py --gh-pages DIR   # also assert every target exists
python3 w3id/simulation/verify_fragments.py                  # real headless Chrome, fragments end to end
```

| Check | Result |
| --- | --- |
| 🎯 Redirection contracts (`Location` header) | **33 / 33** |
| 📉 Same contracts against the rules being replaced | 14 / 33 |
| 📁 Every redirect target exists in the published tree | **33 / 33** |
| 🧭 Concept fragments, real headless Chrome | **6 / 6** |
| 📄 Turtle branch serves the full parsed document | **2 / 2** |

---

## 👥 Maintainers

**Martín J. Salvachúa** — [@MartinM10](https://github.com/MartinM10) ·
[martinjs@uma.es](mailto:martinjs@uma.es) ·
[martin.salvachua1@gmail.com](mailto:martin.salvachua1@gmail.com)

[Khaos Research](https://khaos.uma.es) · Universidad de Málaga 🇪🇸
