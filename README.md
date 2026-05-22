# 🧠 Simulação Computacional para Gestão Térmica
##
<p align="center">
  <img width="900" height="500" alt="image" src="https://media.licdn.com/dms/image/v2/D5612AQETHx1UCkx66A/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1691981770897?e=2147483647&v=beta&t=0O7TBpNSUgM07-CcPeXtZDrWnO-Hpi4leJ4VQl-A3k8"/>
</p>

## 2-📋 Descrição do Projeto
Este projeto foi desenvolvido para a disciplina **Cálculo Numérico Avançado - UNIVERSIDADE ESTADUAL DO MARANHÃO**.  
O objetivo é **demostrar** o resfriamento de um chip de CPU ao longo do tempo dado os devidos parâmetros como:
- **Tipo do Material (Difusividade Térmica);**
- **Potência dos Núcleos;**
- **Temperatura Ambiente;**
- **Fase de Operação;**
- **Potência do Cooler**


O projeto utiliza uma **lista de adjacência** como estrutura de dados principal, oferecendo operações básicas de manipulação de grafos e **visualização gráfica** com as bibliotecas `NetworkX` e `Matplotlib`.


## 3-➕✖️ Modelagem Matemática
A simulação modela um chip de 20 por 20 milímetros, que é uma escala realista para um processador moderno. Esse chip é dividido em uma grade de 100 por 100 pontos, totalizando 10 mil pontos de temperatura sendo calculados simultaneamente. <br>
Cada um desses pontos representa uma região física do silício. A temperatura em cada ponto não é um valor fixo — ela muda a cada passo de tempo, influenciada pelos quatro vizinhos ao redor e pela potência que os núcleos estão gerando naquele instante. <br>
Os 8 núcleos da CPU estão posicionados em dois grupos de quatro, como em um processador real. Cada núcleo tem uma carga configurável de 0 a 100%, e essa carga determina diretamente quanto calor ele injeta no silício ao redor, seguindo uma distribuição gaussiana — mais quente no centro do núcleo, mais frio nas bordas. <br>
A cada passo da simulação — e isso acontece dezenas de vezes por segundo — o computador percorre os 10 mil pontos da grade e, para cada um deles, executa exatamente a mesma conta que os meus colegas apresentaram: a fórmula da diferença finita. <br>
Ele pega a temperatura atual do ponto, soma a contribuição dos quatro vizinhos através do laplaciano discreto, adiciona o calor gerado pelo núcleo que está ali, e desconta a perda pelo cooler. O resultado é a temperatura daquele ponto no próximo instante. <br>
Isso se repete indefinidamente, frame a frame. O que você vê na tela é literalmente o resultado de milhões de operações aritméticas acontecendo em tempo real no seu navegador — estamos falando de algo em torno de 70 milhões de operações por segundo só para manter a simulação rodando a 60 quadros por segundo. <br>

## 3.1 - Condição de Estabilidade 
Antes de qualquer iteração acontecer, a simulação calcula automaticamente o passo de tempo seguro. Esse valor não é arbitrário — ele é determinado pelo critério de estabilidade do método explícito, que diz que o passo de tempo precisa ser menor que o quadrado do espaçamento da malha dividido por quatro vezes a difusividade térmica. <br>
Para o nosso silício, isso nos dá um passo de tempo máximo de cerca de 0,109 milissegundos. A simulação usa 80% desse valor como margem de segurança. Se você ultrapassar esse limite, os erros não somem — eles se amplificam. A temperatura começa a oscilar e divergir em poucos passos. A simulação, na prática, explode numericamente.<br>

## 4-🧩 O que Cada Elemento Visual Representa

O mapa de cores — o heatmap — é a tradução direta do campo de temperatura. O azul escuro representa regiões próximas à temperatura ambiente, em torno de 35 graus. Conforme a temperatura sobe, a cor vai passando por ciano, verde, amarelo, laranja, até o vermelho escuro nas regiões mais quentes.
<img href="imagens_readme/mapa_calor.png"> <br>

As caixas brancas pontilhadas marcam a posição de cada núcleo. Você consegue ver claramente como o calor se origina ali e vai se espalhando pelo silício ao redor — esse espalhamento é exatamente a difusão térmica modelada pela equação de Fourier.<br>

O gráfico de linha no canto inferior — a sparkline — mostra como a temperatura máxima e a temperatura média evoluíram desde o início da simulação. Quando a linha sobe rápido, os núcleos estão em carga alta. Quando ela estabiliza, o sistema chegou ao equilíbrio térmico, onde a geração de calor e a dissipação pelo cooler se igualam. <br>
<img href="imagens_readme/variacaotmax.png">


- **Arestas**: conexões entre eles.  
- O grafo é desenhado automaticamente em uma janela interativa.
