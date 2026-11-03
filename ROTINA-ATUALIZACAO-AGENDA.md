# Rotina de atualização — Agenda do Painel de TV

**Desde novembro/2026 isso é automático.** A agenda que aparece no painel vem
direto da sub-aba **Agenda — Astrea** de Números do Escritório no Portal
(coleção `astreaDiario` no Firestore, alimentada pelo robô do Astrea), não mais
de uma planilha do Google Sheets.

Um script (`portal-fb/scripts/painel-tv-sync/sincronizar_agenda.py`), agendado
para rodar **todo dia às 08h e às 16h**, lê esse snapshot e regrava `agenda.csv`
aqui no repositório automaticamente (commit + push). O painel busca esse
arquivo a cada 10 minutos e também recarrega a página inteira a cada hora.

## Não precisa fazer nada manualmente

Itens com status "Concluído" continuam somando sozinhos do painel.

Se a agenda do painel parecer desatualizada ou incompleta:

1. Confira primeiro se a sub-aba **Agenda — Astrea** do Portal (em Números do
   Escritório) também está desatualizada — se estiver, o problema é no robô do
   Astrea (ou no computador do escritório), não neste painel. Veja a memória
   "Robô Astrea no PC do escritório".
2. Se a sub-aba do Portal estiver certa mas o painel não, confira o log em
   `portal-fb/scripts/painel-tv-sync/log-diario.txt` e se a tarefa agendada
   `PainelTV_SincronizarAgendaAstrea` está rodando (veja o `LEIA-ME.md` da
   pasta do script).
3. Dá para forçar uma sincronização na hora sem esperar 08h/16h, rodando
   `Start-ScheduledTask -TaskName "PainelTV_SincronizarAgendaAstrea"` no
   computador onde a tarefa está instalada.

## Histórico

Até novembro/2026, a atualização era manual: exportar o relatório do Astrea em
.xlsx e importar por cima da planilha "Painel TV - Agenda (fonte)" no Google
Sheets, que o painel buscava direto. Essa planilha não é mais usada.
