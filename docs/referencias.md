# Referências

O material de referência abaixo foi usado na reconstrução do esquema. Ele **não é
redistribuído neste repositório** por ser protegido por direitos autorais — está
listado apenas para atribuição e para quem quiser buscá-lo nas fontes originais.

## Artigo original

- **"Alta-Resolução no TK-85"** (Partes I, II e III) — Milton Maldonado Jr., revista
  *Microhobby*. É a descrição do circuito e do procedimento de montagem/ajuste que este
  projeto implementa.

## Datasheets dos componentes

- ROM **2364** (ROM de máscara de 8 KB, usada como IC2 do TK-85).
- SRAM **6116** (2 KB × 8) — as três memórias do framebuffer.
- EPROM **2716** (mencionada no material do projeto; equivalente pino a pino da 2364 em
  alguns contextos).
- Famílias TTL **74LS00, 74LS93, 74LS138, 74LS157** — datasheets dos respectivos
  fabricantes.

## Placa hospedeira

- **TK-85 revisão K** — esquema/imagem da placa do TK-85 onde o mod é instalado
  (referência para localizar os sinais tomados por `J1`: A14, A15, MREQ, RFSH, WR,
  clock de vídeo e clock de 60 Hz).

## Nota sobre direitos autorais

Este repositório contém apenas trabalho derivado próprio (o esquema em KiCad e a
documentação). Os documentos originais acima pertencem aos seus respectivos autores e
detentores de direitos.
