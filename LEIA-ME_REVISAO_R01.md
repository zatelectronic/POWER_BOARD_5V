# POWER BOARD 5V — Revisão R01 — 2026-11-12
Zat Electronic · KiCad 10.0 · padrão Zat (doc 11)

## CORREÇÃO DE CIRCUITO (autorizada) — buck LM2596
Era o mesmo erro da POWER BOARD SERIAL: U2.2 (OUT) direto no +5V, L1 entre +5V e FB, D2 do FB para o C6–, C6– sem GND.
Agora segue o datasheet:
- U1.2 OUT -> nó SW (L1 + catodo D2).
- Anodo D2 -> GND.
- L1 -> +5V (C2+, FB, TP3).
- C2– -> GND.
No PCB, sem mover componentes:
- U1.2 -> pad de cima do L1 (que já ia ao D2) = nó SW.
- Pad de baixo do L1 = +5V.
- FB (U1.4) -> via (156,30; 67,46) -> B.Cu -> via (169,00; 75,435) na trilha +5V.
- Trilha D2.A–C2– passou a GND.

## Outras alterações de cobre (autorizadas)
- 5 vias de costura GND 1,0/0,5 mm: (119,5; 62,0), (126,0; 61,0), (156,0; 61,0), (173,0; 61,0), (180,0; 61,0).
  A zona GND do B.Cu estava isolada (isolated_copper); agora liga ao GND da frente.
- Zona GND sem preenchimento salvo (o cobre mudou): preencher com B. Simulação: todo o GND fica num só grupo.

## Esquema
- Esquema plano -> capa A2 + 2 folhas: 02 BUCK, 03 CONEXÕES (DC IN / DC OUT + PWR_FLAG).
- Reanotação: U2 -> U1, C5 -> C1, C6 -> C2 (D1, D2, F1, L1 e TP1-TP4 iguais).
- D1/D2: Device:D_Schottky (SS54 não existe no KiCad), Value SS54. O README dizia SS36 no D1: corrigido para SS54.
- TP1-TP4: a biblioteca digikey (dk_Test-Points / digikey-footprints) não está instalada.
  Agora: símbolo Connector:TestPoint + footprint novo ZAT_TestPoint:TestPoint_Pad_10.0x4.0mm.
  O pad 10 x 4 mm é o mesmo, com courtyard e F.Fab.
- PWR_FLAG no nó VIN (fora da junção), em GND, +5V e +24V. Nenhuma severidade em "ignore".
- Valores normalizados (100uF/35V, 680uF/16V, 33uH), Descrição PT, Stock Code, Package Group.

## PCB (fora o cobre acima, nada mudou)
- F1: ZAT_Fuse (3D 1808-2410). L1: ZAT_Inductor_SMD (3D L_12x12mm_H8mm).
- U1 TO-263-5: atualizado para a biblioteca KiCad 10. C, D: 3D padrão do KiCad.
- Referências visíveis em F.SilkS 1,0 mm (antes ocultas).
- Textos de arte (knockout) mantidos. .kicad_dru só com "(version 1)".

## Biblioteca global (já gravada no PC)
- footprints/ZAT_TestPoint.pretty (nova): TestPoint_Pad_10.0x4.0mm. fp-lib-table: + ZAT_TestPoint.

## Pontos de projeto
- IMPORTANTE: C1 de entrada 35 V com 24 V nominais; considere 50 V se a fonte tiver picos.
- ATENÇÃO: placas montadas com o circuito antigo não regulavam (saída chaveada direto na carga).
- MELHORIA: laço SW (U1.2 -> L1/D2) longo; aproximar D2 do U1 numa revisão de layout.

## Após abrir no KiCad
1. Reiniciar o KiCad (tabela de bibliotecas atualizada).
2. ERC no esquema.
3. PCB: B (preencher zonas) -> F8 (deve vir sem alterações) -> DRC.
