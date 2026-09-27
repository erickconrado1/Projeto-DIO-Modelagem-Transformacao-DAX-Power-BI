# Modelagem e transformação de dados com DAX com Power BI

Projeto do desafio da DIO **Modelagem e transformação de dados com DAX com Power BI**, utilizando a planilha real `Financial Sample`.

## Objetivo
Transformar a tabela única `Financial Sample` em uma estrutura baseada em **Star Schema**, mantendo a tabela original como backup e criando as dimensões e a tabela fato solicitadas no enunciado.

## Estrutura preparada
- `Financials_origem` — cópia/backup da fonte original.
- `D_Produtos` — agrupamento por produto com estatísticas de unidades e preços.
- `D_Produtos_Detalhes` — detalhes de produto, faixa de desconto, preço de venda, unidades e preço de fabricação.
- `D_Descontos` — desconto, faixa de desconto e ID do produto.
- `D_Detalhes` — informações adicionais de vendas não concentradas na tabela fato.
- `D_Calendário` — calendário diário baseado no intervalo real de datas da fonte.
- `F_Vendas` — tabela fato com chave substituta (`SK_ID`), produto, métricas e atributos de venda.

## Transformações realizadas
1. Preservação da estrutura original da `Financial Sample`.
2. Padronização do nome da coluna original ` Sales` para `Sales`.
3. Criação do `ID_Produto` por coluna condicional, seguindo a lógica apresentada na aula: Carretera=0, Montana=1, Paseo=2, Velo=3, VTT=4, Amarilla=5.
4. Criação do `SK_ID` sequencial para identificar cada registro da tabela fato.
5. Agrupamento por produto para a `D_Produtos`.
6. Seleção e reorganização das colunas das demais tabelas.
7. Criação do calendário com `CALENDAR()`.
8. Organização das tabelas para o modelo em estrela.

## Relacionamentos
- `D_Produtos[ID_Produto]` 1:* `F_Vendas[ID_Produto]`
- `D_Calendário[Date]` 1:* `F_Vendas[Date]`
- A aula também apresenta relações auxiliares com `D_Produtos_Detalhes` e `D_Descontos`; como essas tabelas possuem registros repetidos por produto, o Power BI pode exigir uma relação muitos-para-muitos ou que a tabela seja mantida como auxiliar.

## DAX
```DAX
D_Calendário =
CALENDAR(
    MIN(Financials_origem[Date]),
    MAX(Financials_origem[Date])
)

Ano = YEAR(D_Calendário[Date])
Mês Nº = MONTH(D_Calendário[Date])
Mês = FORMAT(D_Calendário[Date], "MMMM")
```

## Entrega
O repositório deve conter o `.pbix`, a imagem do esquema em estrela, este `README.md`, o arquivo de fórmulas DAX e a planilha auxiliar preparada.

## Observação sobre a fonte
A coluna existente na planilha é ` Sales`, com espaço inicial. No material da aula ela aparece como `Sales`; a estrutura preparada usa `Sales`. Não existe uma coluna `Salers` na fonte real.
