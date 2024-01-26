# Manual do Utilizador

## Objetivos do Programa

Objetivos do Programa

O programa desenvolvido tem como principais objetivos:
1. Resolver problemas do passeio do Cavalo - O cavalo apenas se pode movimentar em L, um tabuleiro tem o tamanho de 10x10, onde cada casa possui uma pontuação associada.
2. Maximizar a Pontuação - Encontrar uma sequência de movimentos que permita ao cavalo acumular o maior número de pontos possível, seguindo as regras específicas do jogo.
3. Regras específicas do jogo:
  - Número simétricos, em cada jogada é removido do tabuleiro a opção de jogada a posição do tabuleiro que contem o número simétrico.
  - Número Duplo, se a pontuação da casa em que o cavalo calhou fôr um número duplo (por exemplo 55) é removido do tabuleiro o número duplo de maior valor existente.
4. Algoritmos de Procura: Utilizar e avaliar diferentes algoritmos de procura, tais como Busca em Largura (BFS), Busca em Profundidade (DFS) e A*, para determinar qual oferece o melhor desempenho em termos de eficiência e eficácia na resolução de problemas.

## Funcionamento Geral

O programa é uma implementação computacional avançada destinada a resolver uma variante desafiadora do problema do Passeio do Cavalo, um clássico da teoria dos jogos e da inteligência artificial. Neste problema, o cavalo do xadrez deve visitar cada casa de um tabuleiro de xadrez 10x10 exatamente uma vez, seguindo as regras tradicionais de movimento dos cavalos no xadrez.
Esta implementação específica difere do problema clássico de várias maneiras significativas:
- Tabuleiro Pontuado: Cada casa no tabuleiro não é apenas uma casa para ser visitada, mas contém uma pontuação que o cavalo acumula ao visitar. O objetivo é maximizar a pontuação total, não apenas visitar todas as casas.
- Regras Especiais: O jogo inclui regras especiais para movimentos que são simétricos ou formam um 'duplo', o que afeta a estratégia de maximização da pontuação.
- Competição: Existe a dimensão de competir contra um oponente, o que adiciona uma camada estratégica à sequência de movimentos escolhida.
- O programa utiliza algoritmos de procura como BFS, DFS e A*, cada um com suas particularidades e heurísticas específicas para navegar no espaço de soluções do problema. As heurísticas são regras ou métodos que ajudam a otimizar a busca pela solução mais eficiente e eficaz.

O programa foi desenhado com uma ênfase na clareza e na facilidade de uso, garantindo que mesmo utilizadores sem experiência prévia em inteligência artificial possam operá-lo e compreender os conceitos subjacentes através de uma interface interativa e instrutiva.

## Como Usar

Para iniciar o jogo é necessário apenas inserir o comando "(iniciar)"

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/85de5621-11f8-4994-a1ec-3d2fc1ef3dec)

Ao inserir o comando é perguntado a localização do ficheiro problems.bat (ficheiro esse que contem todos os problemas a resolver).

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/17c522ae-6809-4e4b-acdc-76fef453f56b)

Para inserir a localização do ficheiro problemas.bat é com o seguinte comando "C:/Users/[Utilizador]/Localização/ficheiro/problemas.bat"

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/526891c3-a3b2-483b-9849-437c349ed58c)

Ao inserir a localização e supondo que correu tudo bem, é questionado qual o problema que deseja solucionar.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/ff368a66-70f7-4acf-b9d6-8a6c5b2fb497)

Ao inserir a letra correspondente, é apresentado uma mensagem a identificar qual o problema escolhido e logo de seguida é apresentado o tabuleiro e uma mensagem para escolher qual casa da 1ª linha pretende posicionar o cavalo.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/63226967-7eaa-480f-b827-8d62f2f8a99e)

Depois de selecionar qual casa pretende posicionar o cavalo, é apresentado o tabuleiro atualizado com o cavalo e pergunta qual algoritmo pretende utilizar para solucionar o problema.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/758dfba9-6abe-4459-9070-c05507277c91)
