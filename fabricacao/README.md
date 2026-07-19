# Arquivos de fabricação

`mod_tk85_hires_gerbers.zip` contém os Gerbers (X2) e a furação Excellon prontos
para envio a qualquer fabricante comum de PCB (JLCPCB, PCBWay, etc.). Os mesmos
arquivos estão soltos em [`gerbers/`](gerbers/).

## Especificação da placa

| Parâmetro | Valor |
|-----------|-------|
| Camadas | 2 (F.Cu / B.Cu) |
| Espessura | 1,6 mm |
| Dimensões | ~105 × 79 mm (contorno em T, ver `Edge_Cuts`) |
| Trilha mínima | 0,25 mm (sinais); 0,6 mm (+5V/GND) |
| Isolamento mínimo | 0,13 mm |
| Furo mínimo | 0,3 mm (vias 0,6/0,3) |
| Furos não metalizados | nenhum |
| Acabamento | qualquer (HASL serve; a placa é toda THT) |

Nada fora do padrão barato dos fabricantes (capacidade típica: trilha/isolamento
0,127 mm, furo 0,3 mm). DRC do KiCad zerado na revisão que gerou estes arquivos.

## Observações de montagem

- Os CIs do mod são **soldados sem soquete** (exceto a ROM 2364, em soquete de
  lâmina baixo) — restrição de altura do gabinete; ver
  [`../docs/mecanica.md`](../docs/mecanica.md).
- Os dois pentes do interposer (`IC2_PINS_*`) usam barras de pino torneado longo,
  inseridas por cima, com um soquete torneado extra como espaçador no TK-85.
- A pinagem do conector `J1` está serigrafada no **verso** da placa.
