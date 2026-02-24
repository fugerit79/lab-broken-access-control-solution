---
layout: default
---

# Authentication & Authorization

<div style="display: flex; justify-content: center; align-items: center; gap: 2rem; height: 80%;">

  <!-- Sinistra: Authentication -->
  <div style="display: flex; flex-direction: column; align-items: center; gap: 1rem;">
    <img src="./authentication.png" alt="Authentication" style="max-height: 280px; object-fit: contain;" />
    <span style="font-size: 1.4rem; font-weight: bold; color: #74c0fc;">Authentication</span>
  </div>

  <!-- Separatore -->
  <div style="font-size: 4rem; font-weight: bold; color: #ff6b6b; line-height: 1;">≠</div>

  <!-- Destra: Authorization -->
  <div style="display: flex; flex-direction: column; align-items: center; gap: 1rem;">
    <img src="./authorization.png" alt="Authorization" style="max-height: 280px; object-fit: contain;" />
    <span style="font-size: 1.4rem; font-weight: bold; color: #74c0fc;">Authorization</span>
  </div>

</div>


---
layout: default
---

# Authentication & Authorization

<div class="flex items-center gap-8 h-4/5">

  <div class="flex flex-col gap-4 flex-1">
    <div class="rounded-xl border border-blue-400 border-opacity-50 bg-blue-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🔍 Processo mediante il quale un'applicazione verifica l'identità di un utente. In altre parole, risponde alla domanda: <strong class="text-blue-300">"Chi sei?"</strong>
    </div>
    <div class="rounded-xl border border-blue-400 border-opacity-50 bg-blue-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🔑 Gli utenti devono dimostrare la loro identità, di solito attraverso l'inserimento di credenziali, come <strong class="text-blue-300">username e password</strong>.
    </div>
    <div class="rounded-xl border border-blue-400 border-opacity-50 bg-blue-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🛠️ Esistono diversi metodi di autenticazione: <strong class="text-blue-300">Password, MFA, SSO, Biometrica, Certificato</strong>, etc.
    </div>
  </div>

  <div class="flex flex-col items-center gap-4 flex-1">
    <img src="./authentication.png" alt="Authentication" class="max-h-72 object-contain" />
    <span class="text-xl font-bold text-blue-300">Authentication</span>
  </div>

</div>

---
layout: default
---

# Authentication & Authorization

<div class="flex items-center gap-8 h-4/5">

  <div class="flex flex-col items-center gap-4 flex-1">
    <img src="./authorization.png" alt="Authorization" class="max-h-72 object-contain" />
    <span class="text-xl font-bold text-green-300">Authorization</span>
  </div>

  <div class="flex flex-col gap-4 flex-1">
    <div class="rounded-xl border border-green-400 border-opacity-50 bg-green-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🔍 Processo mediante il quale un'applicazione verifica cosa può fare l'utente. In altre parole, risponde alla domanda: <strong class="text-green-600">"Cosa sei autorizzato a fare?"</strong>
    </div>
    <div class="rounded-xl border border-green-400 border-opacity-50 bg-green-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🛡️ L'autorizzazione definisce quali <strong class="text-green-600">risorse e funzionalità</strong> un utente può accedere o modificare.
    </div>
    <div class="rounded-xl border border-green-400 border-opacity-50 bg-green-900 bg-opacity-10 p-4 text-sm text-black-200 leading-relaxed">
      🛠️ Esistono diverse politiche autorizzative: <strong class="text-green-600">RBAC, ABAC, MAC, DAC</strong>, etc.
    </div>
  </div>

</div>