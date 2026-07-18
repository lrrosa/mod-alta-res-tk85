# Mod Alta Resolução para o TK-85 ("Maldonado")

Reconstrução em **KiCad 10** do esquema elétrico da modificação de alta-resolução
gráfica para o **TK-85** (clone brasileiro do Sinclair ZX81), publicada por
**Milton Maldonado Jr.** na revista *Microhobby*.

O circuito acrescenta um framebuffer gráfico de alta resolução ao TK-85: três SRAMs
6116 compartilham o espaço de endereços da ROM de caracteres original (2364) e são
varridas por contadores sincronizados com a geração de vídeo, dando uma definição
comparável à de micros bem mais caros da época.

> **Status:** esquema completo e verificado (ERC sem erros nem avisos). O layout de
> PCB **ainda não** foi feito — veja [Estado do projeto](#estado-do-projeto).

---

## Estrutura do repositório

```
mod-alta-res-tk85/
├── kicad/                     Projeto KiCad 10
│   ├── mod_tk85_hires.kicad_sch    Esquema (folha única)
│   ├── mod_tk85_hires.kicad_pro    Projeto
│   ├── sym-lib-table               Tabela de símbolos (caminhos ${KIPRJMOD})
│   └── sym/                        Símbolos personalizados (ROM 2364, SRAM 6116)
├── esquemas/                  Esquema exportado (PDF e SVG)
├── docs/                      Documentação, errata, BOM
└── README.md
```

## Como abrir

Abra `kicad/mod_tk85_hires.kicad_pro` no KiCad 10. Os símbolos das memórias vêm da
biblioteca local em `kicad/sym/` (referenciada por caminho relativo `${KIPRJMOD}`, de
modo que o projeto pode ser movido ou clonado sem quebrar). Os demais símbolos (74xx,
Device, Connector, power) vêm das bibliotecas padrão do KiCad.

## Como funciona (resumo)

| Bloco | CI | Função |
|-------|----|--------|
| Framebuffer | U1–U3 (6116) | 3× 2 KB de SRAM gráfica, no espaço da ROM de caracteres |
| ROM de caracteres | U4 (2364) | ROM original do TK-85, remanejada para a placa do mod |
| Contadores de vídeo | U5, U6 (74LS93) | Geram o endereço de varredura sincronizado ao vídeo |
| Multiplexador | U7 (74LS157) | Chaveia endereço: CPU (RFSH=1) × contadores (RFSH=0) |
| Decodificador | U8 (74LS138) | Seleciona qual das 3 SRAMs (`/CS0..2`) via A11/A12 |
| Cola lógica | U9 (74LS00) | Inversor de A14 para gerar `RAM CS` |
| Ajuste de reset | RV1 (22K) | Trimpot de temporização do reset dos contadores (errata) |

A conexão física com o TK-85 é feita por um **interposer pass-through**: os dois
soquetes de 12 pinos (`IC2_PINS_1_12`, `IC2_PINS_13_24`) encaixam no soquete da ROM de
caracteres (IC2) do TK-85, e a ROM 2364 encaixa por cima — cada pino do soquete é
eletricamente o mesmo nó do pino correspondente da ROM.

Descrição detalhada em [`docs/descricao-circuito.md`](docs/descricao-circuito.md).

## Errata

O esquema publicado no artigo tem alguns erros, já **corrigidos** aqui. Não os
"desfaça" comparando com a revista. Detalhes em
[`docs/correcoes-errata.md`](docs/correcoes-errata.md). Em resumo:

- `/WE` das 6116 → `WR` do Z80 (pino 22); `/OE` das 6116 → GND.
- Trimpot de 15–22 K no reset dos dois 74LS93 (`RV1`).
- Pino 9 do primeiro 74LS93 → **pino 5** do 74LS157 (o texto do artigo diz pino 9).
- **`/CS` da ROM (pino 20 do IC2) ligado à ROM** — estava solto no projeto anterior.

## Estado do projeto

- [x] Esquema elétrico em KiCad 10, com ERC limpo (0 erros / 0 avisos)
- [x] Nets com nomes significativos, desacoplamento e entradas não usadas amarradas
- [x] BOM ([`docs/bom.csv`](docs/bom.csv))
- [ ] Layout de PCB
- [ ] Montagem e teste em hardware real

## Créditos

- **Circuito original:** Milton Maldonado Jr. (*Microhobby*).
- **Reconstrução do esquema em KiCad:** Leonardo Roman da Rosa.

## Licença

Este projeto de hardware é licenciado sob a **CERN Open Hardware Licence v2 —
Strongly Reciprocal** (`CERN-OHL-S-2.0`). Veja o arquivo [`LICENSE`](LICENSE).

Conforme a seção 3.3 da licença, a localização da fonte (*Source Location*) deste
projeto é: <https://github.com/lrrosa/mod-alta-res-tk85>. Se você distribuir produtos
ou projetos derivados, mantenha as atribuições e disponibilize as fontes completas
sob a mesma licença.

> O material de referência original (o artigo da *Microhobby* e os datasheets dos
> componentes) **não** é redistribuído aqui por ser protegido por direitos autorais.
> Veja [`docs/referencias.md`](docs/referencias.md).
