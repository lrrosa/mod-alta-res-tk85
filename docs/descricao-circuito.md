# Descrição do circuito

Este documento descreve o funcionamento do mod e o significado dos sinais do esquema.
A fonte da lógica é o artigo original de Milton Maldonado Jr.; a nomenclatura de nets
abaixo foi padronizada nesta reconstrução.

## Ideia geral

O TK-85 gera a imagem lendo, a cada caractere, uma tabela de 8 bytes na ROM de
caracteres (IC2 = 2364). O mod intercepta essa ROM e acrescenta **três SRAMs 6116**
(6 KB) mapeadas no mesmo espaço. Assim é possível redefinir dinamicamente as tabelas de
caracteres (POKE nas 6116) e obter 256×192 pixels de alta resolução, sem consumir os
16 KB de RAM do usuário.

O ponto delicado é o **endereçamento**: durante a geração da imagem, quem endereça as
memórias não é a CPU, e sim contadores sincronizados ao vídeo.

## Blocos

### Framebuffer — U1, U2, U3 (6116)
Três SRAMs de 2 KB. Endereços baixos `A0–A8` são compartilhados com a ROM (vêm do
soquete do IC2). `A9`/`A10` de cada SRAM vêm do multiplexador (`RAM_A9`, `RAM_A10`).
Seleção de chip por `/CS0`, `/CS1`, `/CS2` (do 74LS138). `/WE = WR` do Z80, `/OE = GND`
(ver errata). Dados `D0–D7` compartilhados com a ROM e o soquete.

### ROM de caracteres — U4 (2364)
A ROM original do TK-85, remanejada para a placa do mod. Endereços `A0–A8` iguais aos das
SRAMs; `A9–A12` vêm direto da CPU (soquete). `/CS = CS_ROM` (pino 20 do soquete).

### Contadores de vídeo — U5, U6 (74LS93)
- `U5` (CI-A): recebe o **clock de vídeo** (`CLK_VID`) em CP0; Q0→CP1 forma o contador
  de 4 bits; Q3 (`CT_CARRY`) faz o "carry" para o CI-B.
- `U6` (CI-B): clocado por `CT_CARRY`; saídas `VQ0–VQ3` são o endereço de vídeo que
  alimenta o multiplexador.
- Reset: `R0(1)` dos dois CIs no nó `CNT_RST`, alimentado pelo clock de 60 Hz através do
  trimpot `RV1`. `R0(2)` de ambos amarrado a `+5V` (determinístico; era flutuante).

### Multiplexador de endereços — U7 (74LS157)
Seleciona, conforme `RFSH`:
- `RFSH = 1` (CPU acessando as memórias): passa `A9–A12` da CPU.
- `RFSH = 0` (geração de imagem): passa `VQ0–VQ3` dos contadores.

Saídas: `RAM_A9`/`RAM_A10` (para as SRAMs) e `SEL_A0`/`SEL_A1` (para o decodificador).
Enable `/E` (pino 15) em GND (sempre ativo).

### Decodificador de banco — U8 (74LS138)
Entradas `A0/A1 = SEL_A0/SEL_A1` (o par A11/A12 já multiplexado), `A2 = GND`. Habilitação
`G1 = A15`, `/G2A = A14`, `/G2B = MREQ`. Saídas `O0–O2 = /CS0../CS2`. `O3–O7` não usadas.

### Cola lógica — U9 (74LS00)
Apenas a porta A é usada: `NAND(A14, A14) = /A14`, que gera o sinal `RAM CS` de volta ao
TK-85. As outras três portas têm entradas amarradas a GND e saídas em no-connect.

### Interface com o TK-85 — J1
Barra de 8 pinos com os sinais tomados de outros pontos da placa do TK-85:
`A14, A15, RAM CS, MREQ, RFSH, WR, CLK_60, CLK_VID`.

## Referência de nets

| Net | Significado |
|-----|-------------|
| `A0`–`A8` | Endereços baixos compartilhados (soquete → ROM + SRAMs) |
| `A9`–`A12` | Endereços altos da CPU (soquete → ROM + entradas `I1` do mux) |
| `D0`–`D7` | Barramento de dados (soquete ↔ ROM ↔ SRAMs) |
| `CS_ROM` | `/CS` da ROM 2364 (pino 20 do IC2) |
| `RAM_A9`, `RAM_A10` | Endereços A9/A10 das SRAMs (saída do mux) |
| `SEL_A0`, `SEL_A1` | A11/A12 multiplexados → entradas do decodificador |
| `CS0`, `CS1`, `CS2` | Seleção das três SRAMs (`/CS`, do 74LS138) |
| `VQ0`–`VQ3` | Endereço de vídeo (saídas do contador CI-B) |
| `CT_CARRY` | Q3 do CI-A → CP0 do CI-B (cascata dos contadores) |
| `CTA_Q0` | Q0→CP1 do CI-A (realimentação interna do contador) |
| `CNT_RST` | Reset dos contadores (via trimpot `RV1`, do clock de 60 Hz) |
| `RFSH`, `MREQ`, `WR` | Sinais de controle do Z80 |
| `A14`, `A15` | Endereços altos da CPU para o decodificador/cola |
| `RAM CS` | Sinal gerado (`/A14`) devolvido ao TK-85 |
| `CLK_VID`, `CLK_60` | Clock de vídeo (≈15750 Hz) e clock de 60 Hz |

## Desacoplamento

Um capacitor de 100 nF por CI (`C1–C9`) e um eletrolítico de 47 µF de bulk (`C10`).
São acréscimo deste projeto em relação ao artigo — ver a seção 5 de
[`correcoes-errata.md`](correcoes-errata.md).
