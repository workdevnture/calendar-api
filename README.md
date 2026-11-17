# calendar-api

O retorno do login do Google para a agenda da Devnture Platform.

Publicado pelo GitHub Pages em **https://workdevnture.github.io/calendar-api/**. Este é o
endereço que se cadastra como **URI de redirecionamento autorizado** no cliente OAuth do Google
de cada empresa. Ele é o mesmo para todas as organizações.

## Por que existe

O Google devolve o login com `iss=https://accounts.google.com` (RFC 9207). O firewall
(ModSecurity) da hospedagem da plataforma barra, com erro 406, um parâmetro que começa com
`https://` de outro domínio. O bloqueio acontece antes do sistema, e a hospedagem não abre exceção.

Esta página fica fora daquele firewall. Ela lê o retorno, fica só com `state` e `code`, mais o
emissor sem o `https://`, e repassa para a organização certa. O endereço da organização vai
codificado dentro do `state`.

Nada é guardado, enviado a terceiros ou carregado de fora. A página aceita apenas destinos
`https://*.devnture.com.br/api/agenda_oauth.php`, os antigos `https://*.projeteam.com.br/…`
(durante a transição de domínio da versão 6.0) e `*.localhost` para desenvolvimento.

**Publicar antes da virada de domínio:** a versão que aceita `*.devnture.com.br` precisa estar
no ar antes de qualquer organização mudar de endereço; sem ela, conectar a agenda Google na org
migrada para aqui com "Este retorno não é de uma organização". Ao fim da transição, o
`projeteam` sai da expressão.

## Origem

A fonte fica em `infra/oauth-retorno/` no repositório do PMO (`workdevnture/projeteam_crm`).
O lado do sistema está em `includes/agenda_oauth_lib.php`, nas funções `agendaRedirectUri` e
`agendaStateNovo`.
