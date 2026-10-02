# IAviso — Automação de avisos de boletos no WhatsApp

Automação no-code que monitora o Gmail em busca de e-mails de cobrança, extrai **valor** e **data de vencimento** do boleto e envia um aviso formatado no **WhatsApp** via API do Twilio.

Projeto desenvolvido no 1º semestre do curso, em grupo, na disciplina [NOME DA DISCIPLINA].

## O problema

Boletos chegam por e-mail e passam despercebidos. A ideia foi criar um aviso automático, direto no WhatsApp, com as duas informações que importam: quanto e até quando.

## Como funciona

![Fluxograma da automação no Make](docs/fluxograma.png)

O fluxo foi montado no [Make](https://www.make.com/) e tem 5 módulos:

1. **Gmail – Watch emails:** verifica a caixa de entrada a cada 15 minutos, filtrando por assunto (`boleto`, `vencimento`, `2ª via`, `segunda via`).
2. **Gmail – Get an email:** busca o conteúdo completo do e-mail encontrado.
3. **Text parser – Match pattern (valor):** usa regex para extrair valores no formato brasileiro (ex.: `R$ 150,00`).
4. **Text parser – Match pattern (vencimento):** usa regex para extrair a data de vencimento em diferentes formatos.
5. **HTTP – POST:** chama a API do Twilio e envia a mensagem formatada no WhatsApp.

Os padrões regex e a query do Gmail estão documentados em [`regex.md`](regex.md).

## Tecnologias

- Make (automação no-code)
- Gmail (trigger e leitura de e-mails)
- Expressões regulares (regex)
- API REST do Twilio (WhatsApp)

## Minhas contribuições

Apesar de ser um projeto de grupo, fui o responsável pela construção da automação:

- pesquisa e escolha da API (Twilio/WhatsApp);
- montagem do fluxo no Make;
- criação dos padrões regex e da query de filtro do Gmail;
- testes com e-mails reais.

O restante do grupo contribuiu com a pesquisa do tema e a apresentação.
Desenvolvido com apoio de IA (Claude), conforme autorizado pelo professor.

## Limitações conhecidas

- O filtro olha apenas o **assunto** do e-mail, então cobranças com assunto diferente não são detectadas.
- A regex captura qualquer valor em `R$` no corpo do e-mail; se houver mais de um valor, pode pegar o errado (falso positivo).
- E-mails com data de vencimento fora dos formatos previstos não são reconhecidos (falso negativo).
- Não há validação do remetente.

## Ética, privacidade e riscos (LGPD)

Como a automação lê e-mails e envia dados financeiros por um serviço de terceiros, o grupo analisou:

- **Privacidade:** acesso à caixa de entrada e tratamento de dados pessoais conforme a LGPD;
- **Terceiros:** dados trafegam pelo Make e pelo Twilio;
- **Erros de leitura:** valores ou datas extraídos incorretamente podem gerar avisos errados;
- **Segurança:** credenciais da API devem ficar protegidas e fora do repositório.

## Aviso

Credenciais (Account SID, Auth Token, números de telefone) **não** estão neste repositório e devem ser configuradas por quem for reproduzir o fluxo.
