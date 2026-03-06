#  SOC no Microsoft Azure com Honeypot e Microsoft Sentinel

![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoft-azure&logoColor=white)
![SIEM](https://img.shields.io/badge/SIEM-FF0000?style=flat&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-FFA500?style=flat&logoColor=white)

## Sobre o Projeto

O projeto documenta a criação de um **laboratório prático de SOC** utilizando o Microsoft Azure. Com o objetivo de simular um ambiente real de monitoramento de segurança, coletando logs de ataques reais e visualizando tudo em um mapa interativo.

**O que foi feito:**
- ✅ Criação de uma VM Windows propositalmente vulnerável (honeypot)
- ✅ Exposição da VM à internet para atrair ataques reais
- ✅ Coleta de logs de segurança (EventID 4625 - falhas de login)
- ✅ Configuração do Microsoft Sentinel (SIEM)
- ✅ Criação de um mapa de ataques com geolocalização

---

##  Resultados do Laboratório

### Mapa de ataques em tempo real (aproximadamente 20 horas)

<img width="896" alt="Mapa de ataques" src="https://github.com/user-attachments/assets/e1753701-8c15-41b8-a706-3ecdeb5c8683" />

>  Em aproximadamente **20 horas**, a VM honeypot recebeu **milhares de tentativas de ataque** de diversos países. Destaque para os **15.3 mil ataques vindos da Holanda**.

### Custos do Projeto

>  **Custo total do projeto:** **R$ 13,83** (aproximadamente **US$ 2.50**) utilizando o crédito gratuito de $200 da Azure.

<img width="1676" alt="Custos do laboratório" src="https://github.com/user-attachments/assets/32efc7d0-6fbd-49f4-becd-788e345cf879" />

| Serviço | Custo (R$) |
|---------|------------|
| **Virtual Machines** | R$ 10.52 |
| **Storage** | R$ 2.76 |
| **Virtual Network** | R$ 0.55 |
| **Bandwidth** | R$ 0.01 |
| **TOTAL** | **R$ 13.83** |

### Recursos Criados

<img width="805" alt="Recursos do laboratório" src="https://github.com/user-attachments/assets/04834413-59cb-4035-a837-e400a66e9c29" />

Foram criados diversos recursos no grupo **SOC-LAB**:
- Máquina Virtual (propositalmente vulnerável)
- Log Analytics Workspace
- Network Security Group
- IP Público
- Interface de Rede
- Virtual Network (vNET-SOC-LAB)

---

## 📊 Ranking de ataques por País (Em aproximadamente 20h)

| País | Tentativas de Ataque |
|------|---------------------|
| **Holanda** | **15.300** |
| **África do Sul** | **5.300** |
| **Alemanha** | **1.260** |
| **Índia** | **800** |
| **Estados Unidos** | **800** |
| **Laos** | **735** |
| **Itália** | **657** |
| **Japão** | **421** |
| **França** | **182** |
| **Outros** | **367** |

---

## Arquitetura do Projeto:

## 🌐 INTERNET (Ataques Reais)

─ VM WINDOWS (Honeypot) - Propositalmente vulnerável

─ Firewall do Windows:  DESATIVADO

─ Porta RDP (3389):  ABERTA 

─ Coleta de Logs:  Azure Monitor Agent

## 📊 LOG ANALYTICS WORKSPACE

─ Armazena todos os eventos de segurança

─ Especialmente EventID 4625 (falhas de login)

## 🔍 MICROSOFT SENTINEL (SIEM)

─ Workbooks:  Mapa interativo de ataques

─ Análise:  Consultas KQL

---

### Consulta KQL para Geolocalização de Ataques

Esta query foi utilizada no workbook para criar o mapa interativo:

```kql
// Consulta KQL para mapear ataques com geolocalização
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents
| project TimeGenerated, Computer, AttackerIp = IpAddress, cityname, countryname, latitude, longitude
```
- **let GeoIPDB_FULL** ➡ Carrega a watchlist com dados de geolocalização

- **where EventID == 4625** ➡ Filtra apenas eventos de falha de login

- **evaluate ipv4_lookup** ➡ Correlaciona IP's com localização geográfica

- **project** ➡ Exibe apenas as colunas relevantes para o mapa


---
## 📌 O que aprendi nesse projeto

─ Configurar uma VM honeypot no Azure e coletar logs reais de ataque

─ Trabalhar com Microsoft Sentinel e criar visualizações (workbooks)

─ Aprendi a utilizar o KQL (foi fácil por já ter aprendido SQL antes)

─ Analisar padrões de ataque: Holanda foi o país com mais tentativas (15.3k)

─ Gerenciar custos na nuvem: laboratório completo com custo de R$13,83

---

## ⚠️AVISO DE SEGURANÇA: 
- Esta configuração é extremamente perigosa em ambientes reais. Foi criada apenas para fins educacionais e o laboratório foi encerrado depois dos testes.
