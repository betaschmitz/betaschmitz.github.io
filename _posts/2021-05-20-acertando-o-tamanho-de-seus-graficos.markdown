---
layout: post
title:  "Acertando o tamanho de seus gráficos"
description: "Descrição opcional do post"
date:   2021-05-20 10:15:00 -0300
categories: jekyll update
image: "/assets/img/posts/fluxo_gf.png"
---
Muito prazer! Me chamo Roberta, fiquem à vontade para me chamar de Beta.

Sou física, doutora em Geofísica Espacial e atualmente trabalho como cientista de dados. Ao longo de minha carreira aprendi bastante sobre visualização de dados, um tópico geralmente subvalorizado entre os acadêmicos, que querem apenas mostrar os grandiosos resultados de suas pesquisas e acabam esquecendo que uma boa visualização faz toda a diferença na hora da apresentação.

Se você possui interesse nesse assunto já deve ter lido a respeito de diversas bibliotecas e softwares de visualização. Matplotlib, Seaborn, Plotly, D3, Tableau, Google Charts, etc. Qual o melhor? Aprendi com meu orientador do mestrado a lição mais valiosa: **a melhor ferramenta é aquela que a gente mais conhece**. Também pode-se acrescentar que a melhor é a que mais se encaixa no que é preciso fazer.

Inicio aqui uma série de dicas para potencializar seus gráficos utilizando a ferramenta mais básica de todo pythonista: [matplotlib](https://matplotlib.org/3.3.2/index.html). Existem, com certeza, ferramentas mais complexas e que permitem uma maior customização dos gráficos em poucos comandos. Meu preferido é o [plotly em javascript](https://plotly.com/javascript/), que permite gerar gráficos interativos em páginas web, mas esse assunto veremos em outra oportunidade. Por hora, escolhi o matplotlib simplesmente pelo desafio de mostrar que é possível criar boas visualizações sem recorrer a bibliotecas mais complicadas.

Estes posts não têm a pretensão de ser considerado um guia de boas práticas, mas sem dúvida pode lhe auxiliar a obter um gráfico mais atraente a partir de pequenas alterações. Veremos como customizar fontes, legendas, cores e tamanhos dos gráficos, coisas que podem nos dar uma imensa dor de cabeça ao mostrar nossos resultados em diferentes tipos de mídias, como textos acadêmicos, apresentações em pôster ou em slides.

Então passa um café, faz uma pipoca e vamos lá, começaremos gerando gráficos no tamanho correto!

O código a seguir mostra como gerar um gráfico de barras de maneira bem simples, como pode ser visualizado abaixo.

```python
import matplotlib.pyplot as plt

animal= ['Gato', 'Cachorro', 'Coelho', 'Tartaruga', 'Pássaro']
valor = [15, 10, 9, 8.6, 6.1]

fig, ax = plt.subplots()
ax.bar(animal, valor)
plt.show()
```
![Gráfico de barras que mostra valores para animais; gato: 15, cachorro: 10, coelho: 9, tartaruga: 8.6 e pássaro 6.1.](https://dev-to-uploads.s3.amazonaws.com/i/m5qqvvxond9d1op12zns.png)

Até que parece bom, né? Mas pode melhorar bastante. Como podemos ver, esse é um gráfico bem simples, mas os conceitos que veremos aqui se aplicam a qualquer tipo de gráfico, não somente de barras.

Supondo que precisamos mostrar esse gráfico em uma apresentação de slides. O [tamanho padrão](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure) de uma figura no matplotlib é de 6,4 x 4,8 polegadas, embora esse tamanho possa variar de acordo com os dados apresentados. No entanto, isso não nos diz muita coisa quando estamos visualizando gráficos em computadores e/ou celulares, ou seja, necessitamos das medidas em pixels. A imagem mostrada anteriormente possui resolução de 432 x 288 pixels. Uma medida que nos auxilia a calcular a resolução é o DPI, do inglês *dots per inch*, ou pontos por polegada. DPI é uma medida associada a imagens impressas, que está relacionada à distância entre pontos de uma impressora em um papel. Não deve ser confundida com PPI, *pixels por polegada*, embora as duas medidas sejam frequentemente usadas para descrever a mesma coisa, como é o caso do matplotlib. O padrão no matplotlib é `dpi=100` para figuras do tamanho padrão, `6.4" x 4.8"`, e `dpi=72` para outros tamanhos. Dessa forma, se temos 100 pixels em uma polegada, então a resolução total de uma figura que mede `6.4" x 4.8"` é dada por `6.4*100 x 4.8*100`, ou seja, **640 x 480 pixels**. A maioria das telas que usamos hoje em dia possui resolução muito maior do que essa. Sendo assim, colocar uma imagem de 640 x 480 pixels em uma apresentação ou até mesmo em um texto nos mostra dois resultados diferentes: ou a imagem fica minúscula ou precisamos dar um zoom e ela fica borrada:

![Mesmo gráfico de barras da imagem anterior, mas maior e borrado por causa da resolução baixa.](https://dev-to-uploads.s3.amazonaws.com/i/tg293fkmk551mxnzui6l.png)

Sendo assim, o que é preciso fazer para obter uma imagem com boa resolução e bom tamanho? Uma opção é aumentar o tamanho da figura em polegadas utilizando o parâmetro `figsize`, mas ao mudar apenas esse argumento podemos ter outro problema que, particularmente, eu acho o pior de todos: **o tamanho das fontes pode ficar minúsculo**!

```python
import matplotlib.pyplot as plt

animal= ['Gato', 'Cachorro', 'Coelho', 'Tartaruga', 'Pássaro']
valor = [15, 10, 9, 8.6, 6.1]

fig, ax = plt.subplots(figsize=(12,8))
ax.bar(animal, valor)
plt.show()
```
![Gráfico anterior com tamanho grande mas letras pequenas](https://dev-to-uploads.s3.amazonaws.com/i/re0ph05m6whnsbf7vlc5.png)

Uma boa e rápida solução para esse problema seria aumentar o DPI, o que automaticamente aumenta o tamanho das fontes. [Esse post em inglês](https://stackoverflow.com/questions/47633546/relationship-between-dpi-and-figure-size) no *stackoverflow* explica com mais detalhes a relação entre `dpi` e `figzise` e por que aumentando DPI as fontes ficam maiores. Em resumo, pode-se dizer que um maior DPI faz com que todas as linhas desenhadas ocupem um maior espaço na figura, isso inclui textos, números, bordas e as próprias linhas dos gráficos. O mais correto a se fazer então é calcular a combinação correta entre `figsize` e `dpi` para que se chegue na resolução desejada, seja em pixels, centímetros, polegadas ou qualquer unidade. A figura a seguir mostra o gráfico com `dpi=120`. O matplotlib automaticamente ajustou a resolução da imagem para `720 x 480` pixels, ou `6" x 4"`.

```python
animal= ['Gato', 'Cachorro', 'Coelho', 'Tartaruga', 'Pássaro']
valor = [15, 10, 9, 8.6, 6.1]

fig, ax = plt.subplots(dpi=120)
ax.bar(animal, valor)
plt.show()
```
![Mesmo gráfico com figura maior que a original e agora com letras maiores](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3lm1xuzsq5rbmbvehwxl.png)

Está ficando muito mais legível, não acha? Agora vamos supor que os slides sejam mostrados em uma tela ou projetor Full HD, que geralmente possui proporção 16:9, e que queremos mostrar um slide só com um gráfico importante. Podemos modificar o tamanho da figura para ser mostrada na mesma proporção, ocupando um maior espaço na página. O gráfico a seguir foi feito com `figsize=(8,4.5)` e `dpi=150`, o que nos deu uma resolução boa de 1200 x 675 pixels. Não houve necessidade de colocar `figsize=(16,9)` pois teria que aumentar bastante o DPI e a figura gerada seria muito grande.

```python
animal= ['Gato', 'Cachorro', 'Coelho', 'Tartaruga', 'Pássaro']
valor = [15, 10, 9, 8.6, 6.1]

fig, ax = plt.subplots(figsize=(8, 4.5), dpi=150)
ax.bar(animal, valor)
plt.show()
```
![Mesmo gráfico anterior, desta vez com largura maior, pois a proporção entre largura e altura mudou.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/shrhpdjr1r9rzn8e306r.png)

A melhor dica que posso dar é você conhecer exatamente o tipo de mídia que serão mostrados seus gráficos e assim saber calcular o tamanho final desejado. Por exemplo, um texto acadêmico, seja TCC, dissertação de mestrado ou tese de doutorado, em geral é escrito em página A4, que possui 8,3 x 11,7 polegadas. Neste caso, não é necessário colocar um gráfico muito grande, pois a página já é pequena. Porém, ao apresentar seu trabalho em um pôster é bom ajustar o tamanho para que as pessoas consigam enxergar bem suas figuras ao passar pelo corredor. Dessa forma seu trabalho pode chamar mais atenção e, quem sabe, até ser premiado!

No próximo post veremos como customizar legendas e fontes, inclusive mudar o tamanho das fontes sem alterar o tamanho da figura inteira, como foi feito aqui. Se você tiver alguma dúvida fique à vontade para comentar abaixo ou entrar em contato comigo. Muito obrigada e até a próxima!
