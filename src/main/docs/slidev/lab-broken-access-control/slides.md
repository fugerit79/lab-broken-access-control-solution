---
theme: default
title: "Lab: Broken Access Control"
titleTemplate: '%s - Secure Code Academy'
info: |
  ## Secure Code Academy
  Laboratorio su Broken Access Control (OWASP Top 10 #1)
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
mdc: true
layout: cover
background: https://images.unsplash.com/photo-1555949963-aa79dcee981c?w=1920&q=80
favicon: /favicon.svg
---

# 🔐 Broken Access Control

**Secure Code Academy** — Laboratorio pratico

<div class="pt-4 text-gray-300">
  OWASP Top 10:2025 #1 · OWASP API Security 2023 #1
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/lab-sca/lab-broken-access-control" target="_blank" 
     class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
layout: default
---

# Contenuto

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### Teoria
1. 🎯 Broken Access Control: Cosa sono?
2. 📊 Dati e impatto (OWASP 2025)
3. 🔎 Tipologie di vulnerabilità
4. 🌐 OWASP API Security: BOLA
5. 🛡️ Remediation & Best Practice

</div>

<div>

### Pratica
6. 🧪 Struttura del laboratorio
7. 💻 Le 6 vulnerabilità del lab
8. 🔴 Vuln (1) — ID Enumeration
9. 🔴 Vuln (2) — Privilege Escalation (Data)
10. 🔴 Vuln (3) — Privilege Escalation (Action)
11. 🔴 Vuln (4) — Broken Object Authorization
12. 🔴 Vuln (5) — Missing Authentication
13. 🎁 Vuln (X) — Hidden Vulnerability
14. ✅ Approccio TDD & Verifica

</div>

</div>

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
<div style="transform: scale(0.7); transform-origin: top center;">

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

---
layout: default
---

# Tipologie di Vulnerabilità

<div class="grid grid-cols-2 gap-4 mt-2 text-sm">

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-red-500">
  <div class="font-bold text-red-400">🔓 Violazione del Least Privilege</div>
  <div class="mt-1 text-gray-300">Risorse accessibili a chiunque invece che solo agli utenti autorizzati</div>
</div>

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-orange-500">
  <div class="font-bold text-orange-400">🔗 IDOR — Insecure Direct Object Reference</div>
  <div class="mt-1 text-gray-300">Accedere all'account altrui modificando un ID nella richiesta</div>
</div>

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-yellow-500">
  <div class="font-bold text-yellow-400">🚀 Privilege Escalation</div>
  <div class="mt-1 text-gray-300">Agire come utente non autenticato, o ottenere privilegi admin senza averne diritto</div>
</div>

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-purple-500">
  <div class="font-bold text-purple-400">🌐 CORS Misconfiguration</div>
  <div class="mt-1 text-gray-300">Configurazione errata che permette accesso API da origini non autorizzate</div>
</div>

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-blue-500">
  <div class="font-bold text-blue-400">🔑 JWT / Metadata Manipulation</div>
  <div class="mt-1 text-gray-300">Replay o tampering di token JWT per elevare i propri privilegi</div>
</div>

<div class="bg-gray-800 rounded-lg p-3 border-l-4 border-green-500">
  <div class="font-bold text-green-400">🗺️ Force Browsing</div>
  <div class="mt-1 text-gray-300">Accedere direttamente a URL privilegiati senza autenticazione</div>
</div>

</div>

---
layout: two-cols
---

# BOLA — Perché le API sono a rischio

Le API sono particolarmente vulnerabili perché:

- Il server **non traccia lo stato** del client
- Le decisioni di accesso si basano su **parametri inviati dal client** (object ID, VIN, documentId...)
- La risposta HTTP è spesso **sufficiente** per capire se l'attacco ha avuto successo

<br>

> **BOLA ≠ BFLA**  
> In BOLA l'endpoint è accessibile, il problema è a livello di **oggetto**.  
> In BFLA (API5) l'utente non dovrebbe accedere all'endpoint stesso.

::right::

### Scenario reale

```http
# Utente autenticato accede al proprio profilo
GET /api/v1/users/1337/profile
Authorization: Bearer eyJ...

# Attaccante prova ad accedere ad altri utenti
GET /api/v1/users/1338/profile  ← ID modificato
GET /api/v1/users/1/profile     ← Prova admin!
Authorization: Bearer eyJ...    ← Stesso token!
```

<br>

**Rischi concreti:**
- Data breach
- Manipolazione dati altrui
- Account takeover completo

---
layout: default
---

# Come Prevenire il BAC

<div class="grid grid-cols-2 gap-4 mt-2 text-sm">

<div class="bg-green-900 bg-opacity-30 rounded-lg p-4 border border-green-600">

### ✅ Lato Server
- **Deny by default**: nega tutto ciò che non è esplicitamente permesso
- Implementa i controlli di accesso **una sola volta** e riusali
- Valida che l'utente sia il proprietario della risorsa (**record ownership**)
- Centralizza la logica di autorizzazione (no duplicazioni!)

</div>

<div class="bg-blue-900 bg-opacity-30 rounded-lg p-4 border border-blue-600">

### 🔑 Gestione Token e Sessioni
- Invalida i session token **lato server** al logout
- Usa JWT **short-lived** (breve durata)
- Per JWT long-lived: usa **refresh token** con revoca OAuth2
- Non basarti mai su claim del token senza validarli

</div>

<div class="bg-purple-900 bg-opacity-30 rounded-lg p-4 border border-purple-600">

### 🌐 API & CORS
- Disabilita il **directory listing** del server
- Minimizza l'uso di CORS, configura allowed origins in modo restrittivo
- Applica **rate limiting** su endpoint API e controller
- Rimuovi backup e metadata (.git) dalla web root

</div>

<div class="bg-yellow-900 bg-opacity-30 rounded-lg p-4 border border-yellow-600">

### 🧪 Test & Monitoring
- Scrivi **unit e integration test** per il controllo accessi
- **Logga** ogni fallimento di accesso e avvisa gli admin
- Usa **GUID casuali** come ID degli oggetti (non ID sequenziali!)
- Includi test di autorizzazione nelle pipeline CI/CD

</div>

</div>

---
layout: section
---

# Il Laboratorio Pratico

---
layout: default
---

# Struttura del Laboratorio

<div style="display: flex; justify-content: center;">
<div style="transform: scale(0.75); transform-origin: top center;">

```mermaid
graph TD
    MAIN["📦 lab-broken-access-control\n(Repository principale)"]
    Q["🔷 lab-broken-access-control-quarkus\n(Versione Quarkus)"]
    S["🍃 lab-broken-access-control-springboot\n(Versione Spring Boot)"]
    
    MAIN -->|scegli| Q
    MAIN -->|scegli| S
    
    Q --> QT["Quarkus · MicroProfile\nJAX-RS · SmallRye JWT · Panache"]
    S --> ST["Spring Boot · Spring MVC\nOAuth2 Resource Server · Spring Data JPA"]
    
    style MAIN fill:#1971c2,color:#fff
    style Q fill:#4263eb,color:#fff
    style S fill:#2f9e44,color:#fff
```

</div>
</div>

<div class="mt-2 text-sm text-gray-400 text-center">
  Repository principale: <a href="https://github.com/lab-sca/lab-broken-access-control" class="text-blue-400">github.com/lab-sca/lab-broken-access-control</a>
</div>

---
layout: default
---

# Le 6 Vulnerabilità del Laboratorio

<div class="mt-4 text-sm">

| # | Tipo | Classificazione | Endpoint |
|---|------|-----------------|----------|
| **(1)** | ID Enumeration | IDOR | `GET /person/find/{id}` |
| **(2)** | Privilege Escalation — Dati | BOLA | `GET /doc/example.md` · `/doc/person/list` |
| **(3)** | Privilege Escalation — Azione | BOLA | `DELETE /doc/person/delete/{id}` |
| **(4)** | Broken Object Authorization | BOLA | `GET /doc/person/find/{id}` |
| **(5)** | Missing Authentication | Access Control | `GET /doc/example.md` |
| **(X)** | 🎁 Hidden Vulnerability (BONUS) | Access Control | `PUT /person/add` |

</div>

<div class="mt-5 grid grid-cols-2 gap-3 text-xs">
<div class="bg-gray-800 rounded p-3 border border-gray-600">
  📄 <strong>File da modificare:</strong><br/>
  <code>DocResource.java</code> · <code>PersonResource.java</code> · <code>PersonRepository.java</code>
</div>
<div class="bg-gray-800 rounded p-3 border border-gray-600">
  🧪 <strong>Test di riferimento:</strong><br/>
  <code>DocResourceSicurezzaTest.java</code>
</div>
</div>

<div class="mt-3 bg-blue-900 bg-opacity-30 rounded-lg p-3 border border-blue-600 text-xs">
  💡 Ogni vulnerabilità fa fallire almeno un test. La <strong>(2)</strong> ne fa fallire 2. La <strong>(X)</strong> non è coperta dai test — trovala tu!
</div>

---
layout: default
---

# Vuln (1) — ID Enumeration <span class="text-sm font-normal text-orange-400 ml-2">IDOR · GET /person/find/{id}</span>

**Il problema:** restituendo `404` per ID inesistenti e `403` per ID esistenti ma non autorizzati, un attaccante può enumerare gli ID validi nel database.

```http
GET /person/999   → 404 Not Found   ← questo ID non esiste
GET /person/10002 → 403 Forbidden   ← questo ID esiste! 😱
```

<div class="grid grid-cols-2 gap-3 mt-3 text-xs">

<div>

### ❌ Vulnerabile — `PersonResource.java`

```java {5}
@RolesAllowed({ "admin", "user" })
public Response findPerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        return Response.status(NOT_FOUND).build(); // rivela l'esistenza!
    }
    return Response.ok(person.toDTO()).build();
}
```

</div>

<div>

### ✅ Soluzione

```java {5,6}
@RolesAllowed({ "admin", "user" })
public Response findPerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        // SOLUTION (1): sempre FORBIDDEN, mai NOT_FOUND
        return Response.status(FORBIDDEN).build();
    }
    return Response.ok(person.toDTO()).build();
}
```

</div>

</div>

<div class="mt-3 bg-red-900 bg-opacity-20 rounded p-2 text-xs border border-red-800">
  🎯 <strong>Tecnica di attacco:</strong> iterare sugli ID e distinguere 404 da 403 per costruire una mappa degli oggetti esistenti nel database.
</div>

---
layout: default
---

# Vuln (2) — Privilege Escalation (Data) <span class="text-sm font-normal text-orange-400 ml-2">BOLA</span>

**Il problema:** `findByRolesOrderedByName()` riceve i ruoli dell'utente come parametro ma **li ignora nella query**, restituendo tutte le persone senza filtro.

**Endpoint interessati:** `GET /doc/example.md` · `GET /doc/example.html` · `GET /doc/person/list`

<div class="grid grid-cols-2 gap-3 mt-3 text-xs">

<div>

### ❌ Vulnerabile — `PersonRepository.java`

```java {4,5}
public List<Person> findByRolesOrderedByName(
        Collection<String> roles) {
    // Il parametro 'roles' è ricevuto ma ignorato!
    return find("order by lastName, firstName")
               .list();
}
```

</div>

<div>

### ✅ Soluzione

```java {4,5,6}
public List<Person> findByRolesOrderedByName(
        Collection<String> roles) {
    // SOLUTION (2): usiamo 'roles' come filtro nella query
    return find("minRole is null or minRole in ?1 " +
                "order by lastName, firstName", roles)
               .list();
}
```

</div>

</div>

<div class="mt-3 bg-yellow-900 bg-opacity-20 rounded p-2 text-xs border border-yellow-800">
  ⚠️ Fa fallire <strong>2 casi di test</strong> (MD e HTML). Un utente <code>user</code> vede persone con <code>minRole=admin</code> come Richard Feynman.
</div>

---
layout: default
---

# Vuln (3) — Privilege Escalation (Action) <span class="text-sm font-normal text-orange-400 ml-2">BOLA · DELETE /doc/person/delete/{id}</span>

**Il problema:** `@RolesAllowed` include erroneamente il ruolo `user`, permettendo a qualsiasi utente autenticato di **cancellare** persone — operazione riservata solo agli `admin`.

<div class="grid grid-cols-2 gap-3 mt-4 text-xs">

<div>

### ❌ Vulnerabile — `DocResource.java`

```java {3}
@DELETE
@Path("/person/delete/{id}")
@RolesAllowed({ "admin", "user" }) // ← user non dovrebbe cancellare!
@Transactional
public Response deletePerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        return Response.status(FORBIDDEN).build();
    }
    person.delete();
    return Response.ok().build();
}
```

</div>

<div>

### ✅ Soluzione

```java {3,4}
@DELETE
@Path("/person/delete/{id}")
// SOLUTION (3): solo 'admin' può cancellare
@RolesAllowed({ "admin" })
@Transactional
public Response deletePerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        return Response.status(FORBIDDEN).build();
    }
    person.delete();
    return Response.ok().build();
}
```

</div>

</div>

<div class="mt-3 bg-red-900 bg-opacity-20 rounded p-2 text-xs border border-red-800">
  🎯 <strong>Principio violato:</strong> Least Privilege — un utente ottiene più permessi di quelli necessari al suo ruolo.
</div>

---
layout: default
---

# Vuln (4) — Broken Object Authorization <span class="text-sm font-normal text-orange-400 ml-2">BOLA · GET /doc/person/find/{id}</span>

**Il problema:** anche se l'utente è autenticato, non viene verificato se ha il ruolo minimo richiesto **dalla singola risorsa** (`person.getMinRole()`). La fix incorpora anche la (1).

<div class="grid grid-cols-2 gap-3 mt-3 text-xs">

<div>

### ❌ Vulnerabile — `DocResource.java`

```java
@RolesAllowed({ "admin", "user" })
public Response findPerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        return Response.status(NOT_FOUND).build(); // vuln (1)
    }
    // Nessun controllo su person.getMinRole()!
    return Response.ok(person.toDTO()).build();
}
```

</div>

<div>

### ✅ Soluzione (fix 1 + 4)

```java
@RolesAllowed({ "admin", "user" })
public Response findPerson(@PathParam("id") Long id) {
    Person person = this.personRepository.findById(id);
    if (person == null) {
        return Response.status(FORBIDDEN).build(); // fix (1)
    }
    // SOLUTION (4): verifica il minRole della risorsa
    if (person.getMinRole() == null || securityIdentity
            .getRoles().contains(person.getMinRole())) {
        return Response.ok(person.toDTO()).build();
    }
    return Response.status(FORBIDDEN).build();
}
```

</div>

</div>

<div class="mt-3 bg-blue-900 bg-opacity-20 rounded p-2 text-xs border border-blue-800">
  💡 La soluzione della (4) ingloba anche la fix della (1) — i due problemi vivono nello stesso metodo.
</div>

---
layout: default
---

# Vuln (5) — Missing Authentication <span class="text-sm font-normal text-orange-400 ml-2">Access Control · GET /doc/example.md</span>

**Il problema:** il metodo ha `@SecurityRequirement` (solo documentazione Swagger) ma **manca di `@RolesAllowed`**, rendendolo accessibile senza alcuna autenticazione.

<div class="grid grid-cols-2 gap-3 mt-4 text-xs">

<div>

### ❌ Vulnerabile — `DocResource.java`

```java
@GET
@Path("/example.md")
@SecurityRequirement(name = "SecurityScheme")
// @RolesAllowed mancante!
// Chiunque, anche non autenticato, può chiamarlo
public Response markdownExample() throws IOException {
    return Response.ok(
        processDocument(DocConfig.TYPE_MD)
    ).build();
}
```

</div>

<div>

### ✅ Soluzione

```java
@GET
@Path("/example.md")
@SecurityRequirement(name = "SecurityScheme")
// SOLUTION (5): aggiungiamo i ruoli autorizzati.
// 'guest' è il ruolo minimo previsto dalle specifiche
@RolesAllowed({ "admin", "user", "guest" })
public Response markdownExample() throws IOException {
    return Response.ok(
        processDocument(DocConfig.TYPE_MD)
    ).build();
}
```

</div>

</div>

<div class="mt-3 bg-yellow-900 bg-opacity-20 rounded p-2 text-xs border border-yellow-800">
  ⚠️ <strong>Attenzione:</strong> <code>@SecurityRequirement</code> serve solo per la documentazione OpenAPI — <strong>non applica alcun controllo di sicurezza reale</strong>. Serve sempre <code>@RolesAllowed</code>.
</div>

---
layout: default
---

# Vuln (X) — Hidden Vulnerability 🎁 <span class="text-sm font-normal text-purple-400 ml-2">BONUS · PUT /person/add</span>

**Il problema:** un metodo `PUT` alternativo per aggiungere persone è rimasto attivo **senza alcun controllo di autenticazione**. Non è coperto dai test — va trovato analizzando il codice.

<div class="grid grid-cols-2 gap-3 mt-4 text-xs">

<div>

### ❌ Vulnerabile — `PersonResource.java`

```java
@PUT
@Path("/person/add")
@Transactional
// Nessun @RolesAllowed — nessun @SecurityRequirement
// Chiunque può aggiungere persone al database!
public Response addPersonPut(
        AddPersonRequestDTO request) {
    return this.addPerson(request);
}
```

</div>

<div>

### ✅ Soluzione: rimozione completa

```java
// SOLUTION (X): il metodo addPersonPut() è rimasto
// abilitato per errore senza controllo di autorizzazione.
//
// La soluzione corretta è rimuoverlo completamente:
// esiste già addPersonPost() con i controlli appropriati.
//
// ← metodo eliminato
```

</div>

</div>

<div class="mt-3 bg-purple-900 bg-opacity-20 rounded p-2 text-xs border border-purple-800">
  🎁 <strong>Lezione:</strong> API dimenticate o duplicate sono una fonte comune di vulnerabilità. Revisioni regolari del codice e inventario degli endpoint aiutano a individuarle.
</div>

---
layout: default
---

# Approccio TDD: prima i test, poi il codice

Il laboratorio segue il **Test-Driven Development**: i test di sicurezza sono scritti prima e **falliscono** finché le vulnerabilità non vengono corrette.

<div class="grid grid-cols-2 gap-4 mt-4">

<div class="text-center">

### ❌ Prima della fix

```bash
mvn test
```

<div class="bg-red-900 bg-opacity-30 border border-red-700 rounded p-3 mt-2 text-xs font-mono text-left">
Tests run: 11, Failures: 6<br/>
<br/>
<span class="text-red-400">FAIL</span> testFindPersonUser<br/>
<span class="text-red-400">FAIL</span> testListPersonUser<br/>
<span class="text-red-400">FAIL</span> testListPersonUserHtml<br/>
<span class="text-red-400">FAIL</span> testDeletePersonUser<br/>
<span class="text-red-400">FAIL</span> testFindPersonDocUser<br/>
<span class="text-red-400">FAIL</span> testMarkdownGuest
</div>

</div>

<div class="text-center">

### ✅ Dopo la fix

```bash
mvn test
```

<div class="bg-green-900 bg-opacity-30 border border-green-700 rounded p-3 mt-2 text-xs font-mono text-left">
Tests run: 11, Failures: 0<br/>
<br/>
<span class="text-green-400">PASS</span> testFindPersonUser<br/>
<span class="text-green-400">PASS</span> testListPersonUser<br/>
<span class="text-green-400">PASS</span> testListPersonUserHtml<br/>
<span class="text-green-400">PASS</span> testDeletePersonUser<br/>
<span class="text-green-400">PASS</span> testFindPersonDocUser<br/>
<span class="text-green-400">PASS</span> testMarkdownGuest
</div>

</div>

</div>

---
layout: default
---

# Come Affrontare il Laboratorio

<div class="grid grid-cols-3 gap-4 mt-4 text-center text-sm">

<div class="bg-gray-800 rounded-xl p-4">
  <div class="text-3xl mb-2">1️⃣</div>
  <div class="font-bold text-blue-400">Setup</div>
  <div class="mt-2 text-gray-300 text-xs">
    Clona il repo e avvia con <code>mvn quarkus:dev</code>. Esegui i test: vedrai 6 fallimenti.
  </div>
</div>

<div class="bg-gray-800 rounded-xl p-4">
  <div class="text-3xl mb-2">2️⃣</div>
  <div class="font-bold text-yellow-400">Analizza & Attacca</div>
  <div class="mt-2 text-gray-300 text-xs">
    Cerca i commenti <code>// VULNERABILITY: (n)</code>. Prova a sfruttare ogni falla con curl o Swagger UI.
  </div>
</div>

<div class="bg-gray-800 rounded-xl p-4">
  <div class="text-3xl mb-2">3️⃣</div>
  <div class="font-bold text-green-400">Correggi & Verifica</div>
  <div class="mt-2 text-gray-300 text-xs">
    Applica le fix una alla volta. Riesegui i test finché tutti e 11 passano. Poi cerca la (X)!
  </div>
</div>

</div>

<div class="mt-5 grid grid-cols-2 gap-3 text-xs">

<div class="bg-gray-800 rounded p-3 border-l-4 border-blue-500">
  <strong class="text-blue-400">File da modificare</strong><br/>
  <code>PersonResource.java</code> — vuln 1, X<br/>
  <code>PersonRepository.java</code> — vuln 2<br/>
  <code>DocResource.java</code> — vuln 3, 4, 5
</div>

<div class="bg-gray-800 rounded p-3 border-l-4 border-green-500">
  <strong class="text-green-400">Come verificare le soluzioni</strong><br/>
  Cerca: <code>// VULNERABILITY: (n)</code> nel codice<br/>
  Confronta con il branch <code>solution</code><br/>
  oppure cerca: <code>// SOLUTION: (n)</code>
</div>

</div>

---
layout: default
---

# Riferimenti e Risorse

<div class="grid grid-cols-2 gap-4 mt-4 text-sm">

<div>

### OWASP
- [OWASP Top 10:2025 — A01 Broken Access Control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/)
- [OWASP API Security 2023 — API1 BOLA](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Proactive Controls: C1 Access Control](https://top10proactive.owasp.org/archive/2024/the-top-10/c1-accesscontrol/)
- [OWASP ASVS V8 Authorization](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x17-V8-Authorization.md)

</div>

<div>

### Laboratorio
- [📦 Repository principale](https://github.com/lab-sca/lab-broken-access-control)
- [🔷 Lab Quarkus](https://github.com/lab-sca/lab-broken-access-control-quarkus)
- [🍃 Lab Spring Boot](https://github.com/lab-sca/lab-broken-access-control-springboot)

### Strumenti Utili
- [Fugerit Venus Doc](https://venusdocs.fugerit.org/)
- [PortSwigger Web Academy — Access Control](https://portswigger.net/web-security/access-control)
- [OAuth 2.0 — Revoking Access](https://www.oauth.com/oauth2-servers/listing-authorizations/revoking-access/)

</div>

</div>

---
layout: end
---

# Buon Laboratorio! 🚀

<div class="flex gap-8 items-center justify-between mt-6">

  <div class="flex-1">
    <div class="text-gray-400 mt-2">
      Domande? Apri una issue su <strong>GitHub</strong>
    </div>
    <div class="mt-4">
      <a href="https://github.com/lab-sca/lab-broken-access-control"
         class="text-blue-400 hover:text-blue-300 text-sm">
        🔗 github.com/lab-sca/lab-broken-access-control
      </a>
    </div>
    <div class="mt-8 text-xs text-gray-600">
      Basato su OWASP Top 10:2025 · OWASP API Security Top 10:2023 · Licenza MIT
    </div>
  </div>

  <div class="flex-1 flex flex-col items-center justify-center">
    <img src="/qrcode.svg" alt="QR Code" class="w-48 h-48"/>
    <div class="text-xs text-gray-500 mt-2 text-center">
      github.com/lab-sca/lab-broken-access-control
    </div>
  </div>

</div>

