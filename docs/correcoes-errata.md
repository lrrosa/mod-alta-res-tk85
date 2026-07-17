# Correções / Errata

O esquema publicado no artigo original (*Alta-Resolução no TK-85*, Milton Maldonado Jr.,
*Microhobby*) contém alguns erros e ambiguidades. As correções abaixo **já estão
aplicadas** neste projeto — não as reverta ao comparar com a revista.

## 1. `/WE` e `/OE` das SRAMs 6116

- O `/WE` (pino 21) das três 6116 vai ao **`WR` do Z80 (pino 22 da CPU)**.
- O `/OE` (pino 20) das três 6116 vai ao **GND**.

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

## 4. `/CS` da ROM de caracteres — ligação faltando (novo)

**Não estava no artigo nem no projeto KiCad anterior.** No projeto anterior, o `/CS` da
ROM 2364 (pino 20 do IC2) ficava **eletricamente solto**: nem no esquema nem no cobre
havia ligação ao pino 20.

Isso passou despercebido porque, na montagem "teia de aranha" do artigo, a ROM continua
no soquete do TK-85 e ninguém mexe no pino 20. Ao migrar para uma placa em que a ROM é
remanejada, essa ligação passa a ser necessária.

Como o interposer é **pass-through** (o pino do soquete é o mesmo condutor que sobe até a
ROM), a correção é ligar o pino do soquete que carrega o `/CS` (`IC2_PINS_13_24` pino 5)
ao pino 20 da ROma `U4` — ambos na net `CS_ROM`. Sem isso, a ROM nunca é selecionada e o
TK-85 não dá boot.

Confirmação: dos 24 pinos do soquete IC2, 23 seguiam o padrão "pino do soquete = pino da
ROM"; exatamente esse `/CS` era o único que faltava — assinatura de esquecimento, não de
intenção de projeto.
