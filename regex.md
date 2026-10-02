# Padrões utilizados

## Query do Gmail (módulo Watch emails)

Filtra apenas e-mails relevantes: a automação só roda quando o assunto contém uma dessas expressões.

```
subject:boleto OR subject:vencimento OR subject:"2ª via" OR subject:"segunda via"
```

## Regex do valor (Text parser nº 1)

Captura valores monetários no padrão brasileiro, como `R$ 150,00` ou `R$ 1.250,90`.

```
R\$\s*(\d{1,3}(?:\.\d{3})*(?:,\d{2})?)
```

- `R\$\s*`: o símbolo `R$` seguido de espaços opcionais
- `\d{1,3}(?:\.\d{3})*`: parte inteira, com separador de milhar por ponto
- `(?:,\d{2})?`: centavos opcionais, com vírgula

## Regex do vencimento (Text parser nº 2)

Identifica a data que vem depois da palavra "vencimento", aceitando `/`, `-` ou `.` como separador.

```
[Vv]encimento[:\s]+(\d{2}[\/\-\.]\d{2}[\/\-\.]\d{2,4})
```

- `[Vv]encimento`: aceita "Vencimento" ou "vencimento"
- `[:\s]+`: dois-pontos e/ou espaços depois da palavra
- `\d{2}[\/\-\.]\d{2}[\/\-\.]\d{2,4}`: dia, mês e ano (2 ou 4 dígitos)

> Exemplos reconhecidos: `Vencimento: 15/10/2025`, `vencimento 15-10-25`, `Vencimento: 15.10.2025`
