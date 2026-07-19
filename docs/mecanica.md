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

- **Língua** (sobre o IC2): x −11,1…+10 / y −20…+17,5 — carrega os dois pentes do
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

**Medições no gabinete real (2026-07-18):** sobram **~12 mm** livres tanto acima da
ROM (IC2) quanto acima dos ICs 30–37. Todos esses CIs estão **soquetados** de
fábrica (soquetes de lâmina marrons, ~3,5–4 mm — inclusive o IC2 e o 2716), com o
topo a ~8–9,5 mm da placa-mãe. O teto útil é portanto de **~20–21 mm** em toda a
região ocupada pela placa do mod.

**Pilha adotada** (contorno atual mantido):

| Camada | Altura acumulada sobre a placa-mãe |
|--------|-----------------------------------|
| Soquete existente do IC2 (lâmina, de fábrica) | ~3,5–4 mm |
| Soquete torneado extra (espaçador) | ~7,5–8 mm |
| Pentes do interposer sob a placa do mod | vão total **~10–11 mm** |
| Placa do mod (1,6 mm) | ~12–13 mm |
| CIs do mod **soldados** (~4,5 mm) | topo a **~16,5–17 mm** ✓ |
| ROM em soquete de lâmina baixo na placa do mod | topo a **~20 mm** — no limite, cabe |

- O vão de ~10–11 mm passa sobre os ICs 30–37 soquetados (~8–9,5 mm) com folga de
  1–2,5 mm.
- Os CIs do mod (TTLs e 6116) devem ser **soldados sem soquete** para preservar o
  teto. Só a ROM — o chip original transplantado — usa soquete, de lâmina e perfil
  baixo.
- Pino torneado encaixa em soquete de lâmina, mas confira o aperto do contato; se
  ficar frouxo, vale trocar o soquete do IC2 por um torneado.
- Estabilidade: a placa fica em balanço apoiada só no soquete do IC2. Recomenda-se
  um **pé de apoio** (espaçador de nylon com base adesiva, ou bloco de espuma) sob
  o canto sudoeste do corpo.

## Montagem da ROM sobre a língua (interposer)

Os dois pentes do interposer estão em geometria DIP-24 (600 mil) e carregam
**exatamente as mesmas nets, pino a pino, do soquete `U4`** — consequência do
interposer pass-through. Ou seja: a ROM 2364 pode ser encaixada **diretamente
sobre a língua**, em cima de `U9` e `U6`, dispensando o soquete `U4` (que fica
despopulado; **nunca** popular os dois ao mesmo tempo com duas ROMs).

A peça que realiza isso é um **soquete DIP-24 torneado com terminais wire-wrap**
(postes de 2 ou 3 níveis, 10–17 mm): o corpo fica no topo da placa como soquete da
ROM, e os postes atravessam a placa e viram os pinos macho do interposer por baixo.
Exemplos: Mill-Max série 123 (123-93-624-41-001000), Preci-Dip 110-93-624-41-001000;
como alternativa mais fácil de encontrar, **duas barras SIP 1×12 torneadas de
terminal longo** ("round pin female header, wire-wrap/long pin") cortadas na medida.

Alturas nessa configuração (**medidas sobre a placa-mãe do TK-85**; topo da placa
do mod fica a ~11,6–12,6 mm):

- Assento do soquete wire-wrap: ~4,5–5 mm sobre a placa do mod — os CIs
  `U9`/`U6` soldados (~4,3 mm) passam por baixo **no limite** (folga 0,2–0,7 mm).
- Topo da ROM: 11,6–12,6 + 4,5–5 + 4,5 ≈ **20,6–22 mm** — **igual ou acima do
  teto de ~20–21 mm**. Com um soquete extra empilhado (para dar folga sobre
  U9/U6), vai a ~24 mm — **não cabe de jeito nenhum**.
- **Conclusão: com o teto medido, esta opção é desaconselhada.** Fica registrada
  pela equivalência elétrica (vale para gabinetes modificados ou uso sem tampa);
  a montagem recomendada é a ROM no soquete `U4` (topo a ~19–20,6 mm).
- Por baixo, os postes atravessam o soquete-espaçador e entram no soquete original
  do IC2. Postes wire-wrap são quadrados (~0,64 mm): encaixam bem em soquetes de
  **lâmina** (o espaçador e o soquete original do TK-85); evite usar soquete
  torneado como espaçador nesse caso, pois o furo torneado é para pino redondo.

## RV1 — ajuste por cima

`RV1` usa o Bourns **3266W** (ajuste pelo topo). A variante lateral (3266P) foi
descartada porque o parafuso ficava voltado para o interior da placa, inacessível
com os soquetes vizinhos populados.

**Altura:** o 3266W tem **8,26 mm** de corpo — topo a ~19,9–20,9 mm sobre a
placa-mãe, no **mesmo grupo crítico da ROM** frente ao teto de ~20–21 mm. Antes de
soldar, faça um ensaio a seco: com a pilha do interposer montada e a tampa
fechando, é preciso haver **≥ 8,3 mm** livres acima do topo da placa do mod na
região do RV1. Se faltar, os planos B são: (a) trocar por um trimpot de volta
única com ajuste por cima e ~5 mm de altura (ex.: Piher PT-6-V — exige retrabalho
dos pads naquele canto), ou (b) montar o trimpot fora da placa, por 3 fios curtos
nos pads (estilo "teia de aranha", fiel ao artigo).

## Roteamento

Duas camadas, roteado por autorouter (Freerouting 2.2.4) com acabamento manual,
DRC do KiCad **zerado** (0 violações elétricas, 0 pads desconectados):

- Trilhas de sinal **0,25 mm**; `+5V` e `GND` **0,6 mm**; isolamento mínimo
  **0,13 mm**; vias 0,6/0,3 mm — tudo dentro da capacidade de qualquer fabricante
  comum de PCB (limite típico 0,127 mm).
- `GND` é roteado como trilha **e** reforçado por pours nas duas faces.
- A checagem `starved_thermal` foi rebaixada para aviso: quatro pads de GND ficam
  com um único raio térmico da zona, mas todos têm também conexão plena por
  trilha — eletricamente sólidos; o alívio térmico ainda ajuda na soldagem.

## J1 (sinais do TK-85)

Barra de 8 pinos em ângulo reto na borda sul, com os pinos apontando para a frente
do micro (lado do teclado). Os fios voadores para A14/A15/MREQ/RFSH/WR (CPU),
RAM CS, clock de vídeo (IC-11) e clock de 60 Hz (IC-24) saem por ali.
