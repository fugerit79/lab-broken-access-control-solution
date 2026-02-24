---
layout: section
---

# Broken Access Control:

# Cosa Sono?

---
layout: two-cols
---

# Broken Access Control

Il controllo degli accessi garantisce che gli utenti **non possano agire al di fuori dei loro permessi previsti**.

Un fallimento di questo meccanismo porta tipicamente a:

- **Divulgazione non autorizzata** di informazioni
- **Modifica o distruzione** di dati altrui
- **Esecuzione di funzioni privilegiate** senza averne il diritto

::right::

<div style="display: flex; justify-content: center;">
<div style="transform: scale(1.0); transform-origin: top center;">

```mermaid
flowchart TD
    U([👤 Utente]) --> R{Richiesta API}
    R --> AC{Access Control Check}
    AC -->|✅ Autorizzato| D[(Database)]
    AC -->|❌ Non autorizzato| E[403 Forbidden]
    AC -->|⚠️ ASSENTE o ROTTO| D
    D --> RES([📄 Risposta])
    
    style AC fill:#ff6b6b,color:#fff
    style E fill:#51cf66,color:#fff
    style D fill:#339af0,color:#fff
```

</div>
</div>

---
layout: default
---

# OWASP Top 10:2025 — #1

<div class="grid grid-cols-3 gap-4 mt-6">

<div class="bg-red-900 bg-opacity-40 rounded-lg p-4 border border-red-500">
  <div class="text-3xl font-bold text-red-400">100%</div>
  <div class="text-sm mt-1">delle applicazioni testate presenta qualche forma di BAC</div>
</div>

<div class="bg-orange-900 bg-opacity-40 rounded-lg p-4 border border-orange-500">
  <div class="text-3xl font-bold text-orange-400">1.8M+</div>
  <div class="text-sm mt-1">occorrenze rilevate nei dati raccolti</div>
</div>

<div class="bg-yellow-900 bg-opacity-40 rounded-lg p-4 border border-yellow-500">
  <div class="text-3xl font-bold text-yellow-400">32.654</div>
  <div class="text-sm mt-1">CVE correlati — il secondo numero più alto in assoluto</div>
</div>

</div>

<div class="mt-6 text-sm text-gray-400">

| Metrica | Valore |
|--------|--------|
| CWE mappate | 40 |
| Max Incidence Rate | 20,15% |
| Avg Weighted Exploit | 7,04 / 10 |
| Avg Weighted Impact | 3,84 / 10 |

</div>

<div class="mt-4 text-xs text-gray-500">
  Fonte: <a href="https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/" class="text-blue-400">OWASP Top 10:2025 — A01</a>
</div>
