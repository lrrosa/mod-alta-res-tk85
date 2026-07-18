# Correções / Errata

O artigo original (*Alta-Resolução no TK-85*, Milton Maldonado Jr., *Microhobby*)
contém alguns erros, omissões e ambiguidades. As correções abaixo **já estão
aplicadas** neste projeto — não as reverta ao comparar com a revista.

## 1. `/WE` e `/OE` das SRAMs 6116 (omissão do artigo)

- O `/WE` (pino 21) das três 6116 vai ao **`WR` do Z80 (pino 22 da CPU)**.
- O `/OE` (pino 20) das três 6116 vai ao **GND**.

**Por que isso não aparece no artigo:** o roteiro de montagem (Parte II) liga os
pinos 18 das 6116 (`/CS0..2`, na tabela do 74LS138) e os pinos 19 e 22 (`A10'` e
`A9'`, na tabela do 74LS157), mas não diz o que fazer com os pinos 20 e 21; e o
"Esquema Completo" (Microhobby, pág. 33) mostra apenas a lógica (74LS157, 74LS138 e
os dois 74LS93) — as 6116 não aparecem nele. Sem essas duas ligações o circuito não
funciona: sem `/WE` a CPU nunca escreve no framebuffer, e com `/OE` flutuando a
leitura fica indefinida.

**Por que estas ligações:** `WR` é o único strobe de escrita do Z80, e a escrita já é
qualificada chip a chip pelo `/CS` vindo do 74LS138 (que por sua vez só ativa com
`MREQ` e a janela de endereços A15/A14 correta) — é a forma canônica de pendurar SRAM
num barramento Z80. `/OE` em GND é seguro pela tabela-verdade da 6116: as saídas só
habilitam com `/CS`=0 **e** `/WE`=1; chip desselecionado (standby) ou em escrita fica
em alta impedância (`/OE` é *don't care* na escrita, conforme o datasheet). Como a
janela das SRAMs não se sobrepõe à da ROM, nunca há dois chips dirigindo o barramento
ao mesmo tempo, e não é preciso usar o `/RD` (que nem está entre os sinais tomados
do TK-85).

## 2. Trimpot de reset (RV1)

É necessário um trimpot de **15 a 22 kΩ** na ligação entre o pino 2 de ambos os 74LS93
e o pino 2 do IC-24 do TK-85 (que carrega o clock de 60 Hz). Esse é o `RV1` (22 K), que
ajusta a temporização do reset dos contadores. O procedimento de ajuste está descrito
na Parte III do artigo.

No esquema, o cursor do trimpot (pino 2) e o extremo não usado (pino 3) estão ligados ao
mesmo nó (`CNT_RST`). Isso é boa prática para trimpots usados como reostato: se o cursor
perder contato, a resistência vai ao máximo em vez de abrir o circuito.

## 3. Ambiguidade pino 9 vs. pino 5 do 74LS157

O texto do artigo manda ligar o **pino 9 do primeiro 74LS93** ao **pino 9 do 74LS157**,
mas isso está errado — o destino correto é o **pino 5 do 74LS157** (entrada `2A`/`I0b`).
A própria tabela do artigo se contradiz: lista "9 → 2A (pino 9)", mas `2A` do 74LS157 é
o pino **5**. As demais entradas da tabela (1A=pino 2, 3A=pino 11, 4A=pino 14) estão
corretas, confirmando que o pino 5 é o certo.

Neste projeto: `U6` pino 9 (Q1) → `U7` pino 5 (I0b), na net `VQ1`.

## 4. `/CS` da ROM de caracteres (montagem em placa)

Na montagem "teia de aranha" do artigo, a ROM continua no soquete do TK-85 e o pino 20
dela (`/CS`, gerado pela lógica do próprio TK-85) não é tocado — por isso o artigo não
fala dele. Nesta implementação em placa, a ROM é remanejada para o soquete `U4` da
placa do mod, e o `/CS` precisa acompanhá-la: o **pino 20 do soquete do IC2** deve ser
ligado ao **pino 20 da ROM** (net `CS_ROM`). Sem essa ligação a ROM nunca é
selecionada e o TK-85 não dá boot.

Detalhe de leitura do esquema: o soquete do IC2 é desenhado como dois conectores de 12
posições. As posições 1–12 de `IC2_PINS_1_12` correspondem aos pinos 1–12 do IC2, e as
posições 1–12 de `IC2_PINS_13_24` correspondem aos pinos **24–13** (nessa ordem) — o
`/CS` (pino 20 do IC2) é a posição 5 desse segundo conector.
