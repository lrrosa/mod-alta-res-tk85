# Mecânica — contorno, alturas e empilhamento

Este documento registra as medidas físicas que definiram o contorno da placa e o
esquema de montagem do interposer. As medidas foram tiradas de uma foto calibrada da
placa do TK-85 (calibração pelo passo de 2,54 mm dos DIPs; escala conferida no
DIP-40 do Z80 — erro estimado ±2–3%).

## Sistema de coordenadas

Tudo é medido em **mm relativos ao centro do IC2** (a ROM principal), com
+x para leste (em direção ao Z80) e +y para sul (em direção ao teclado). No arquivo
`kicad/mod_tk85_hires.kicad_pcb`, o centro do IC2 corresponde à posição absoluta
(150, 100).

## Contorno (forma de T)

- **Língua** (sobre o IC2): x −10,5…+10 / y −20…+17,5 — carrega os dois pentes do
  interposer, `U9` e `U6`.
- **Corpo**: x −70…+35,6 / y +17,5…+59 — paira sobre as fileiras de TTL (IC17…) e
  de RAM (IC30–37) do TK-85.
- **Recorte SE**: para x > +15,5 o corpo pára em y = +44, desviando do capacitor
  de tântalo ("gota") ao lado do IC9.

## Obstáculos altos medidos (sob ou ao lado da placa)

| Obstáculo | Posição (mm rel. IC2) | Altura aprox. | Situação |
|-----------|----------------------|---------------|----------|
| Capacitor azul deitado | x +11,5…+24,5 / y −17…−8 | ~10–11 mm | **Fora** do contorno (a língua pára em x = +10) |
| 2716 + soquete (IC3) | x −31…−12,5 / y −15…+17 | ~13 mm | **Fora** (corpo começa em y = +17,5) |
| Z80 | x +36,7… | — | Fora (borda leste em +35,6, folga ~1 mm) |
| Capacitor "gota" | x +17…+20,5 / y +46…+56 | ~9 mm | Fora (recorte SE) |
| ICs 30–37 **soquetados** | y ≈ +42…+61 | ~8–9,5 mm | **Sob a placa** — ver empilhamento |
| Fileira de TTL (IC17…) | y ≈ +17,5…+35 | ~4,5 mm | Sob a placa, baixa |

## Empilhamento do interposer e teto do gabinete

**Medição no gabinete real (2026-07-18): sobram ~12 mm acima da ROM atual.** Com a
ROM soldada (~5 mm de altura), o teto útil acima da placa-mãe na região do IC2 é de
**~17 mm**.

Vãos por nível de empilhamento (fundo da placa do mod → superfície do TK-85):

| Montagem | Vão sob a placa | Altura total¹ |
|----------|----------------|----------------|
| 1 nível (pente torneado + soquete no IC2) | ~6–7 mm | ~12,5–13 mm |
| 1 nível + 1 soquete extra | ~10–11 mm | ~16,5–17 mm |
| 2 níveis (2 soquetes extras) | ~13–15 mm | ~20–26 mm |

¹ Com os CIs do mod **soldados** (sem soquetes); placa 1,6 mm + CI ~4,5 mm.
Soquetes na placa do mod acrescentam ~4 mm.

Implicações com o teto de ~17 mm:

- **2 níveis não cabem.** Descartado.
- **1 nível + 1 soquete extra** (vão ~10–11 mm): passa sobre os ICs 30–37
  soquetados (~8–9,5 mm) com folga pequena, e fica no limite do teto (~17 mm)
  **desde que os CIs do mod sejam soldados** (a ROM pode usar soquete de lâmina,
  baixo). É a opção compatível com o contorno atual da placa.
- **1 nível simples** (vão ~6–7 mm): altura confortável (~13 mm), mas o corpo da
  placa **não pode pairar sobre os ICs 30–37** — exigiria redesenhar o contorno
  para ficar só sobre as áreas baixas (fileira de TTL, faixa norte, campo de
  resistores a leste).

**Pendente:** o teto de ~17 mm foi medido sobre o IC2. A folga sobre a região sul
(fileiras IC17… e IC30–37, em direção ao teclado) pode ser diferente — precisa ser
medida antes de bater o martelo, pois é onde fica o corpo da placa.

Outras notas:

- Estabilidade: a placa fica em balanço apoiada só no soquete do IC2. Recomenda-se
  um **pé de apoio** (espaçador de nylon com base adesiva, ou bloco de espuma) sob
  o canto sudoeste do corpo.
- Na foto de referência o IC2 está **soldado direto** na placa (sem soquete).
  Nesse caso é preciso dessoldar a ROM e instalar um soquete torneado antes de
  plugar o mod.

## J1 (sinais do TK-85)

Barra de 8 pinos em ângulo reto na borda sul, com os pinos apontando para a frente
do micro (lado do teclado). Os fios voadores para A14/A15/MREQ/RFSH/WR (CPU),
RAM CS, clock de vídeo (IC-11) e clock de 60 Hz (IC-24) saem por ali.
