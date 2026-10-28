# calendar-api

O retorno do login do Google para a agenda do ProjeTeam.

Publicado pelo GitHub Pages em **https://workdevnture.github.io/calendar-api/**. Este é o
endereço que se cadastra como **URI de redirecionamento autorizado** no cliente OAuth do Google
de cada empresa. Ele é o mesmo para todas as organizações.

## Por que existe

O Google devolve o login com `iss=https://accounts.google.com` (RFC 9207). O firewall
(ModSecurity) da hospedagem do ProjeTeam barra, com erro 406, um parâmetro que começa com
`https://` de outro domínio. O bloqueio acontece antes do sistema, e a hospedagem não abre exceção.

Esta página fica fora daquele firewall. Ela lê o retorno, fica só com `state` e `code`, mais o
emissor sem o `https://`, e repassa para a organização certa. O endereço da organização vai
codificado dentro do `state`.

Nada é guardado, enviado a terceiros ou carregado de fora. A página aceita apenas destinos
`https://*.projeteam.com.br/api/agenda_oauth.php` (e `*.localhost` para desenvolvimento).

## Origem

A fonte fica em `infra/oauth-retorno/` no repositório do PMO (`workdevnture/projeteam_crm`).
O lado do sistema está em `includes/agenda_oauth_lib.php`, nas funções `agendaRedirectUri` e
`agendaStateNovo`.
