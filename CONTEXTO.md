# Contexto do Projeto — Simulador de Renda Fixa Brasil

## Repositório
**GitHub Pages:** https://andressaponzo.github.io/simulador-renda-fixa  
**Repositório:** https://github.com/AndressaPonzo/simulador-renda-fixa  
**Workflow:** `.github/workflows/atualizar-dados.yml`

---

## O que é
Calculadora de renda fixa brasileira (CDB, LCI, LCA, IPCA+, Tesouro Selic) hospedada no GitHub Pages. Os dados são atualizados automaticamente via GitHub Actions que consulta a API pública do Banco Central do Brasil.

---

## Arquivos principais
| Arquivo | Função |
|---|---|
| `index.html` | Página única — todo o HTML, CSS e JS |
| `dados.json` | Dados gerados pelo workflow (Selic, IPCA, Focus, Copom) |
| `atualizar-dados.yml` | GitHub Actions — roda seg a sex às 06h e 18h BRT |

---

## Como funciona
1. O workflow busca da API do BCB: Selic efetiva (SGS 4189), Selic meta (SGS 432), IPCA 12m (SGS 13522), Focus Selic anual e Focus IPCA anual
2. Gera `dados.json` com trajetória automática do Copom (queda por reunião arredondada a 0,25pp)
3. O `index.html` carrega `dados.json` e aplica os dados; se falhar, usa fallbacks hardcoded

---

## Decisões técnicas importantes

### CDI
- Exibido nos cards como `Selic meta − 0,10pp`
- Calculado internamente como `calcCDIPonderado(dias) = média ponderada da Selic pelo tempo * 0,999`

### Fallbacks (quando dados.json vem com arrays vazios)
- `focusSelic []` → usa `FOCUS_SELIC_FALLBACK`
- `focusIPCA []` → usa `FOCUS_IPCA_FALLBACK`
- `copomCalendario` flat (todos 14,75%) → usa `COPOM_FALLBACK`
- Validação: copom é válido só se houver pelo menos 2 valores de Selic distintos

### Equivalências entre produtos
- Card **independente** do slider de prazo do simulador
- Cada faixa de IR (180d / 181–360d / 361–720d / 720d+) calcula seu próprio CDI e IPCA ponderados com os dias fixos dela

### Meta do IPCA (classificação)
- `> 4,5%` → "acima da meta" (badge laranja escuro)
- `> 3,0%` → "acima do centro" (badge laranja suave)
- `≤ 3,0%` → "dentro da meta" (badge verde)
- `< 1,5%` → "abaixo da meta"

---

## Dados do boletim Focus — 27/04/2026
| Indicador | 2026 | 2027 | 2028 | 2029 |
|---|---|---|---|---|
| Selic | 13,00% | 11,00% | 10,00% | 9,88% |
| IPCA | 4,86% | 4,00% | 3,61% | 3,50% |

---

## Trajetória Copom (fallback hardcoded)
Selic meta atual: **14,75%** (decisão de março/2026)  
Próxima reunião: **29/04/2026** — consenso de mercado: corte de **0,25pp → 14,50%**

| Data | Selic projetada |
|---|---|
| 29/04/2026 | 14,50% |
| 17/06/2026 | 14,25% |
| 05/08/2026 | 14,00% |
| 16/09/2026 | 13,75% |
| 04/11/2026 | 13,50% |
| 09/12/2026 | 13,25% |
| 03/02/2027 | 13,00% |
| 17/03/2027 | 12,75% |
| 05/05/2027 | 12,50% |
| 16/06/2027 | 12,25% |
| 28/07/2027 | 12,00% |
| 15/09/2027 | 11,50% |
| 03/11/2027 | 11,25% |
| 08/12/2027 | 11,00% |

---

## Pendências / ideias para próximas sessões
- Verificar comportamento após reunião do Copom de 29/04 (dados.json deve atualizar na execução das 18h)
- Qualquer outra melhoria que surgir

---

## Para retomar a conversa
Envie este arquivo + `index.html` + `dados.json` + `atualizar-dados.yml` e diga: **"continuando o projeto"**.
