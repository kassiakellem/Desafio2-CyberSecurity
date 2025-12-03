

## **Cliente:** LojaZeta

## **Consultora:** Kássia Kellem Especialista em Blue Team e Segurança Cibernética

## **Data:** 03 de Dezembro de 2025

## ***Aviso de Confidencialidade:***

## *Este documento é estritamente confidencial e contém informações proprietárias.*

# 

# **1\. Sumário Executivo**  

A LojaZeta, em fase de crescimento, enfrenta riscos cibernéticos graves que ameaçam a continuidade de suas operações e a confiança dos clientes.  
 Tentativas recentes de ataques como SQL Injection, XSS e Brute-force indicam a vulnerabilidade da infraestrutura web.   
Além disso, a falta de uma ferramenta de visibilidade centralizada (SIEM) e a ausência de validação de rotinas de backup elevam o risco operacional a níveis críticos.

Esta proposta detalha uma estratégia de defesa em profundidade focada no princípio 80/20: maximizar a proteção com recursos limitados.  
 A solução envolve a implementação de um WAF em nuvem, endurecimento (hardening) da aplicação e infraestrutura, e a criação de um monitoramento centralizado mínimo viável (MVP).   
Espera-se, com este projeto, reduzir a superfície de ataque em 90% nos primeiros 30 dias e garantir a capacidade de detecção e recuperação rápida de incidentes.

## **2\. Escopo e Metodologia**

**O que está coberto:**

* **Web Application Firewall (WAF):** Proteção de borda.  
* **Hardening de Servidor:** Configurações de segurança no Nginx e Linux.  
* **Monitoramento (SIEM):** Implementação de coleta de logs centralizada.  
* **Backup e Recuperação:** Revisão de políticas.

**Metodologia:**

A análise baseia-se no framework NIST CSF (Identificar, Proteger, Detectar, Responder, Recuperar).

* **Premissas:** A equipe interna fornecerá o acesso administrativo (root/admin) aos servidores e ao painel de DNS para a realização das configurações.

## **3\. Arquitetura de Defesa (Camadas)**

Abaixo, a topologia de segurança proposta, segregando a rede externa (hostil) da infraestrutura interna e adicionando visibilidade.

[![](https://mermaid.ink/img/pako:eNqNU9Fu2jAU_RXLlSbQAksIgeCHSgHWCYmgrEir1MCDV1-SrIkdOc4GpXzNnvcV_bE5CUxZN2nzQ3R8fc859_rGR_wgGGCCI0nzGC1vNxzp1XwXXIHkoDqdC-p2Ua93je68m3CWipJVaNuiNLAoPzdyc_8ehQHIlx8ZKCkQAzSHHRT0zKmWVqg1l9NwFSV8jwIp9gf0FvmCreHhnAmcXWxeeTSl0RSFt6D1A5l8paxtsJzW-l4QhF6eo5Xut_-laCXokzpjPg07gShUJGH9cdn90_l1ez7lNIIMuELhByjUy3eB3ui6eaKEpFVctGzWi_d-eEefyriGW9Tr966fZyIFRdFSRMWzLvX39L-k6GL_mTNvySw8P_RvvHefgtX2cg-txs4dqUMK9SR2SZqSK9M0jUIP7BHIlW1fcO9bwlRMBvm-zapr-D8aNvRvljBMlCzBwBnIjFZbfKwEN1jF-jI3mGjIqHzc4A0_aU5O-b0Q2YUmRRnFmOxoWuhdmTOqYJ5QPZLsV1Tq5kDORMkVJgNnVItgcsR7TOz-xBqNHWfgms7Ismxbnx4wcd2-Mx4OXauKD2x3eDLwU21r9S3bmZhD07KtsT0ZGxhYNWG_eTn1Azr9BJgK_CE?type=png)](https://mermaid.live/edit#pako:eNqNU9Fu2jAU_RXLlSbQAksIgeCHSgHWCYmgrEir1MCDV1-SrIkdOc4GpXzNnvcV_bE5CUxZN2nzQ3R8fc859_rGR_wgGGCCI0nzGC1vNxzp1XwXXIHkoDqdC-p2Ua93je68m3CWipJVaNuiNLAoPzdyc_8ehQHIlx8ZKCkQAzSHHRT0zKmWVqg1l9NwFSV8jwIp9gf0FvmCreHhnAmcXWxeeTSl0RSFt6D1A5l8paxtsJzW-l4QhF6eo5Xut_-laCXokzpjPg07gShUJGH9cdn90_l1ez7lNIIMuELhByjUy3eB3ui6eaKEpFVctGzWi_d-eEefyriGW9Tr966fZyIFRdFSRMWzLvX39L-k6GL_mTNvySw8P_RvvHefgtX2cg-txs4dqUMK9SR2SZqSK9M0jUIP7BHIlW1fcO9bwlRMBvm-zapr-D8aNvRvljBMlCzBwBnIjFZbfKwEN1jF-jI3mGjIqHzc4A0_aU5O-b0Q2YUmRRnFmOxoWuhdmTOqYJ5QPZLsV1Tq5kDORMkVJgNnVItgcsR7TOz-xBqNHWfgms7Ismxbnx4wcd2-Mx4OXauKD2x3eDLwU21r9S3bmZhD07KtsT0ZGxhYNWG_eTn1Azr9BJgK_CE)


* **Segmentação:** O Banco de Dados (PostgreSQL) não possui IP público, comunicando-se apenas com a App (Node.js).  
* **WAF:** Atua como primeira barreira contra ataques volumétricos e comuns (OWASP Top 10).  
* **SIEM:** O Wazuh correlaciona logs de todas as camadas para gerar alertas inteligentes.

## **4\. Monitoramento & SIEM**

Implementação do **Wazuh** para centralização de logs e detecção de intrusão baseada em host.

* **Fontes de Log e Monitoramento de Segurança**

**Nginx:**

* Detecção de scanners de vulnerabilidade.  
* Monitoramento de erros anômalos.

**Node.js (PM2/Aplicação):**

* Registro de logs de erros da aplicação.  
* Monitoramento de tentativas de login falhas.

**PostgreSQL:**

* Identificação de *queries* lentas.  
* Monitoramento de acessos negados.

**Sistema Operacional:**

* Monitoramento de integridade de arquivos.  
* Registro e auditoria de logins SSH.  
* **KPIs de Sucesso:**  
  * **MTTD (Tempo Médio de Detecção):** Reduzir para \< 30 min.  
  * **Tentativas Bloqueadas:** Volume de ataques parados pelo WAF automatizado.  
  * **Cobertura:** 100% dos servidores de produção monitorados.

## **5\. Resposta a Incidentes (NIST IR)**

Estabelecimento de um ciclo de vida de incidentes:   
**Detecção → Contenção → Erradicação → Recuperação.**

**Runbooks Essenciais:**

1. **SQL Injection/XSS:** Bloqueio imediato do IP de origem no WAF; análise do payload nos logs; patch de correção no código.  
2. **Brute-Force:** O sistema (Fail2Ban/Wazuh) bloqueia o IP automaticamente após 5 tentativas; notificação via Slack/E-mail.  
3. **Indisponibilidade:** Verificação de integridade do serviço; switch para página de manutenção estática; análise de recursos (CPU/RAM) e reinício escalonado dos serviços.

## **6\. Recomendações (80/20) e Roadmap**

| Prazo | Ação (Foco em Quick Wins) | Responsável |
| :---- | :---- | :---- |
| **30 Dias** | Implementação do Cloudflare (WAF) e SSL Rigoroso. | Consultoria |
|  | Configuração de backups automatizados diários (Off-site). | Infra/Dev |
|  | Hardening do SSH (Porta, Chave RSA, sem root login). | Consultoria |
| **90 Dias** | Deploy do Wazuh SIEM e configuração de alertas básicos. | Consultoria |
|  | Revisão de código focada em segurança (SAST básico). | Dev Team |
| **180 Dias** | Testes de Intrusão (Pentest) recorrentes. | Externo |
|  | Treinamento de Conscientização de Segurança para o time. | Consultoria |

## **7\. Riscos, Custos e Assunções**

* **Custos:** A solução prioriza ferramentas Open Source (Wazuh, Nginx) e Planos Free Tier (Cloudflare) para adequação ao orçamento, exigindo apenas horas técnicas de implementação.  
* **Riscos:** A implementação do WAF no modo de "block" pode, inicialmente, levar a falsos positivos e, consequentemente, bloquear usuários legítimos.  
  Será realizado um período de "learning mode" de 48h.  
* **Dependências:** Necessidade de disponibilidade da equipe de desenvolvimento para eventuais correções de código (bugs) que afetem a segurança.

## **8\. Conclusão**

Segurança não é um luxo, mas sim uma necessidade fundamental para a LojaZeta.  
Esta proposta transforma a postura da empresa de reativa para proativa.   
Ao blindar a aplicação Node.js e monitorar o ambiente em tempo real, garantimos a confiança necessária para escalar o negócio sem medo de incidentes catastróficos.

**Próximo Passo:** Aprovação desta proposta para início imediato do Hardening e WAF.

