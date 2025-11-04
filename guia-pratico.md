# 🛡️ Guia Completo de Segurança para Devs — CIDA-I + DevSecOps

> Segurança não é feature — é requisito fundamental.

Este guia reúne **práticas reais, diretas e aplicáveis** para garantir os pilares de segurança da informação no desenvolvimento moderno, tanto em apps web quanto APIs, serviços e nuvem.

---

## 🎯 Objetivo

Ajudar devs a aplicar segurança na prática, com foco em:

- Pilares de segurança (CIDA-I)
- API segura
- DevSecOps
- Zero-Trust
- Controles de infraestrutura
- OWASP Top 10
- Checklist de implementação

---

## 🔐 Os 5 Pilares: CIDA-I

| Pilar | O que é | Como implementar | Ferramentas / Ex. |
|---|---|---|---|
**Confidencialidade** | Somente quem deve acessa | RBAC/ABAC, Criptografia, Segredos | AES, TLS, JWT com escopos, Vault, `.env` |
**Integridade** | Dado não é modificado sem autorização | Hash, assinatura digital, validação, logs | SHA-256, HMAC, Validação server-side, Git |
**Disponibilidade** | Sistema sempre operacional | HA, redundância, cache, rate-limit, backups | Load Balancer, Redis, Cloud HA, DRP |
**Autenticidade** | Confirmar quem é o usuário/serviço | MFA, chaves, certificados, OAuth2/OIDC | FIDO2, TLS, JWT, Keycloak |
**Irretratabilidade** | Não pode negar que fez | Logs auditáveis, assinatura digital, trilha | Auditoria DB, ICP-Brasil, blockchain opcional |

> Mnemônico: **CIDA-I** → *“Confio, Investigo, Disponibilizo, Autentico, Imputo”*

---

## 🧠 Mental Model do Dev Seguro

> **“Este código resiste a abuso?”**

Perguntas-gatilho:

- Quem pode acessar? (Confidencialidade)
- Dá pra provar quem fez? (Autenticidade/Irretratabilidade)
- Pode ser alterado? (Integridade)
- Aguenta carga/ataque? (Disponibilidade)

---

## 🔥 Segurança no Ciclo de Dev (DevSecOps)

### SAST, SCA e CI/CD

| Etapa | Controles |
|---|---|
Pré-commit | Husky, lint, prettier, **secret scanner (gitleaks)** |
CI | SAST, SCA (dependabot/renovate), testes de segurança |
Build | Docker scan, SBOM (CycloneDX / Syft) |
Deploy | IaC scan (Trivy, Checkov), OPA/Conftest |
Runtime | Logs estruturados, métricas, IDS/IPS, observabilidade |

---

## 🧱 Zero-Trust Principles

- Não confiar em ninguém por padrão
- Toda requisição autenticada e autorizada
- Segredos rotacionados
- Privilégio mínimo (least privilege)
- Segmentar tudo (rede, API, banco)
- Validar comportamento (métrica, logs, auditoria)

---

## 🌐 API e Backend Seguro

### Controles essenciais

| Recurso | Medidas |
|---|---|
Autenticação | OAuth2/OIDC, MFA, refresh tokens curto |
Autorização | RBAC/ABAC, escopos, claims |
Proteção | Rate-limit, throttling, bot-defense |
Input | Sanitização, validação estrita |
Transporte | HTTPS Only, HSTS, TLS moderno |
Segredos | Vault, KMS, nunca em repo |
Logs | Estruturados + correlação de requisição |
Erro | Mensagens seguras (sem stack para usuário) |

---

## 🛑 Proteção contra abuso e ataques

| Medida | Por quê | Ferramentas |
|---|---|---|
Rate-Limiting | Evita brute-force e flood | express-rate-limit, NGINX, Cloudflare |
WAF | Filtra ataques OWASP | Cloudflare, AWS WAF, ModSecurity |
Bot-Challenge | Protege formulários e APIs | hCaptcha, Cloudflare Bot Fight Mode |
Anti-brute-force | Travamento progressivo | Cache, regra temporária, MFA |
Pagination + limites | Anti-exhaustion | cursor-based pagination |
Circuit breaker | Protege serviço instável | Resilience4j, Istio |
Retry + backoff | Evita loops destrutivos | Retry libs, API Gateways |

---

## 🔧 Infra, Cloud & Containers

| Controle | Benefício |
|---|---|
Rootless containers | Mitiga escape |
Network policies | Zero-trust na rede |
Secrets manager | Protege credenciais |
Hardened base images | Minimiza ataque |
Firewall/SG | Redução de superfície |
Logging centralizado | Auditoria e rastreio |
Backups + restore testado | Disponibilidade real |

---

## 🧾 OWASP Top 10 (Resumo prático)

- Quebra de acesso → RBAC/ABAC + testes
- Criptografia fraca → TLS, AES, hashing moderno
- Exposição de dados → mascarar logs, validação
- Input sem sanitizar → validação server-side
- Configurações inseguras → IaC + hardening
- Falha na autenticação → MFA, expiração, refresh seguro
- Depêndencias vulneráveis → SCA, renovação contínua
- Logging pobre → logs estruturados + correlação
- SSRF/XSS/CSRF → validação, CORS, tokens, sandbox

---

## ✅ Checklist Final

```txt
🔐 Auth & Access
[ ] OAuth2/OIDC + MFA
[ ] Escopos/roles
[ ] Refresh token seguro

🧼 Input & Data
[ ] Sanitização e validação
[ ] Criptografia em trânsito e repouso
[ ] Hashing seguro

⚔️ Segurança contra abuso
[ ] Rate-Limit + throttling
[ ] WAF + bot protection
[ ] Paginação com limites

🔥 Infra
[ ] Segredos seguros (Vault/KMS)
[ ] TLS + HSTS
[ ] Logs + alertas + métricas
[ ] Backup + restore testado

🏁 DevSecOps
[ ] SAST/SCA/DAST
[ ] IaC scanning
[ ] SBOM (Syft/CycloneDX)
