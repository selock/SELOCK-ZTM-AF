# SELOCK Zero Trust Mobile Attestation Framework (ZTM-AF)

![SELOCK ZTM-AF Cover](./docs/ztm_af_cover.png)

## Conceito Central

O **SELOCK Zero Trust Mobile Attestation Framework (ZTM-AF)** é um framework open-core inovador desenvolvido pela SELOCK para estabelecer um novo padrão em segurança e privacidade de dispositivos móveis. Ele permite a construção determinística de imagens Android hardened, validação criptográfica de integridade pós-instalação, e um sistema de attestation offline independente de Google/Apple. O ZTM-AF gera automaticamente evidências técnicas para compliance com **LGPD, ISO 27001 e NIST**.

Em termos práticos, o ZTM-AF é um sistema que permite provar, matematicamente, que um dispositivo móvel está exatamente no estado de segurança declarado pela SELOCK.

## O Desafio Atual do Mercado

Atualmente, o mercado depende fortemente de soluções proprietárias e centralizadas para attestation de dispositivos móveis, como:

*   Google Play Integrity API
*   Apple MDM attestation
*   Soluções proprietárias de EMM (Enterprise Mobility Management)

Não existe uma solução open, soberana e auditável focada em Android hardened sob controle total do operador, o que limita a autonomia e a capacidade de verificação independente da segurança.

## Arquitetura Proposta pela SELOCK

### 1. Build Determinístico de ROM Hardened

Um pipeline robusto baseado em AOSP (Android Open Source Project) que garante a reprodutibilidade da construção de imagens Android hardened. Utiliza ferramentas como Nix/Guix ou Docker pinned para assegurar que cada build seja idêntica, resultando em um hash criptográfico imutável da imagem final. Um SBOM (Software Bill of Materials) completo é gerado automaticamente, oferecendo transparência total sobre os componentes da ROM.

**Estrutura de Diretórios:**

*   `/build`: Contém scripts e configurações para o pipeline de build determinístico.
*   `/attestation`: Módulos para o processo de attestation e geração de provas (`.ztmproof`), incluindo a ferramenta CLI (`ztm_attest_cli.py`) para geração e verificação.
*   `/compliance`: Ferramentas e documentação para automação de compliance.
*   `/policies`: Definições de políticas de segurança e hardening.
*   `/docs`: Documentação técnica detalhada do framework.

### 2. Motor de Hardening Declarativo

Em vez de scripts bash avulsos, o ZTM-AF utiliza um motor declarativo baseado em arquivos YAML para definir as políticas de hardening. Isso permite que as configurações de segurança sejam versionáveis, auditáveis e facilmente gerenciáveis. Exemplos de políticas incluem:

```yaml
camera: disabled_physical
microphone: disabled_logical
bluetooth: restricted
usb: charging_only
baseband: monitored
firewall:
  default_policy: deny
  exceptions:
    - vpn_tunnel
```

O motor converte essas definições em regras de segurança aplicáveis, como regras `iptables/nftables`, `sysctl tuning`, políticas SELinux customizadas, desativação de serviços e bloqueio de sensores via HAL (Hardware Abstraction Layer).

### 3. Attestation Soberana Offline

Este é o diferencial do ZTM-AF. Um sistema que gera uma chave raiz protegida por hardware (utilizando StrongBox, se disponível) para assinar o estado do sistema. Isso permite a geração de um relatório verificável offline (`.ztmproof`), que pode ser validado por um auditor externo sem depender de serviços de Google ou Apple. O processo envolve:

*   **Dispositivo**: Gera um hash criptográfico do estado do sistema, assina com a chave protegida e produz um arquivo `.ztmproof`.
*   **Auditor**: Valida a assinatura e confirma o hash da build oficial, provando a integridade técnica do dispositivo de forma independente de vendor.

### 4. Compliance Automation Engine

Cada política de segurança aplicada pelo ZTM-AF gera um mapeamento automático para controles de compliance, incluindo:

*   ISO 27001
*   NIST SP 800-53
*   LGPD art. 46
*   Marco Civil art. 7

O resultado é um relatório técnico exportável em PDF/JSON, contendo evidências de hardening, controles implementados, hash da imagem e versão da política. Isso é um diferencial extremamente forte para o mercado corporativo que busca comprovação de segurança e conformidade.

### 5. Threat Surface Index

O ZTM-AF introduz um índice quantitativo para medir a superfície de ataque de um dispositivo. Este score é baseado em:

*   Serviços ativos
*   Sensores habilitados
*   Conexões abertas
*   Permissões concedidas
*   Módulos kernel carregados

O resultado é uma métrica objetiva de redução de superfície de ataque, algo que nenhuma solução de hardening atual entrega de forma clara ao cliente final.

---

## 🚀 Objetivos Estratégicos

- **Soberania Digital**: Prover controle total sobre a segurança e privacidade de dispositivos móveis, sem dependência de terceiros.
- **Transparência e Auditabilidade**: Gerar provas criptográficas e relatórios de compliance para validação independente.
- **Redução de Superfície de Ataque**: Implementar hardening declarativo e medir objetivamente a exposição a ameaças.

---

## ✨ Status Atual e MVPs Funcionais

Este repositório já contém **MVPs (Produtos Mínimos Viáveis) funcionais** para os componentes chave, transformando a visão em execução:

*   **Build Determinístico**: Script `reproducible_build.py` em `/build` para simular a geração de builds e SBOMs.
*   **Motor de Hardening Declarativo**: Script `declarative_hardening_engine.py` em `/build` que interpreta políticas YAML (ex: `mobile_hardening_policy.yaml` em `/policies`).
*   **Attestation Soberana**: Ferramenta CLI `ztm_attest_cli.py` em `/attestation` para gerar e verificar provas `.ztmproof`.
*   **Compliance Automation**: Script `compliance_automation_engine.py` em `/compliance` para gerar relatórios de compliance baseados nas políticas.

---

## 💡 Próximos Passos (Roadmap)

Para tornar o ZTM-AF ainda mais "revolucionário", as próximas prioridades técnicas incluem:

*   **Especificação ZTMAttest**: Detalhar o formato e a certificação do artefato de attestation soberana.
*   **Integração de Ferramentas**: Incorporar ferramentas como Nix/Guix para builds verdadeiramente determinísticos.
*   **Key Management**: Desenvolver uma estratégia segura para gerenciamento de chaves privadas de attestation.
*   **Threat Surface Index**: Implementar o cálculo do score de superfície de ataque.

---

## 📌 Contato e Suporte
Para suporte técnico ou consultas sobre o ZTM-AF:
**E-mail:** geral@selock.net  
**Site:** [https://www.selock.net](https://www.selock.net)

---

## 🛠 Licença
Este framework e seus ativos são licenciados sob a **MIT License** pela SELOCK.

---

## 📌 Contato e Suporte
Para suporte técnico ou consultas sobre o ZTM-AF:
**E-mail:** geral@selock.net  
**Site:** [https://www.selock.net](https://www.selock.net)

---

## 🛠 Licença
Este framework e seus ativos são licenciados sob a **MIT License** pela SELOCK.


---

## 📌 Contato e Suporte
Para suporte técnico ou consultas sobre o ZTM-AF:
**E-mail:** geral@selock.net  
**Site:** [https://www.selock.net](https://www.selock.net)

---

## 🛠 Licença
Este framework e seus ativos são licenciados sob a **MIT License** pela SELOCK.
