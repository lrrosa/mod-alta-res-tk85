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
