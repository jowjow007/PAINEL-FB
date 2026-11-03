# Rotina de atualização — Agenda do Painel de TV

**Desde novembro/2026 isso é automático.** A agenda que aparece no painel vem
direto da sub-aba **Agenda — Astrea** de Números do Escritório no Portal
(coleção `astreaDiario` no Firestore, alimentada pelo robô do Astrea), não mais
de uma planilha do Google Sheets.

Um script (`portal-fb/scripts/painel-tv-sync/sincronizar_agenda.py`), agendado
para rodar **todo dia às 08h e às 16h**, lê esse snapshot e publica `agenda.csv`
numa branch separada deste repositório, **`agenda-live`** (não em `main`) — o
painel busca o arquivo direto de lá. O painel busca esse arquivo a cada 10
minutos e também recarrega a página inteira a cada hora.

A branch `agenda-live` é tratada como um "estado atual", não um histórico:
como este repositório é público (exigência do GitHub Pages), ela nunca
acumula mais de 1 commit — cada sincronização reescreve esse commit, para não
deixar nomes de clientes/processos guardados para sempre no histórico do Git.
Isso não muda nada do que já era visível antes (a planilha do Google Sheets
publicada na web também era pública para quem tivesse o link), só evita que
o dado de hoje fique arquivado permanentemente.

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
