# Manual do Utilizador

## Objetivos do Programa

Objetivos do Programa

O programa desenvolvido tem como principais objetivos:
1. Implementar o algoritmo alfaBeta, é uma estratégia de decisão em jogos de dois jogadores. Este algoritmo é crucial para criar um adversário automatizado desafiador, capaz de tomar decisões estratégicas inteligentes durante o jogo.
2. Permitir jogos em dois modos diferentes, Humano vs Computador e Computador vs Computador. No primeiro modo, um jogador humano compete contra o computador, e no segundo, o programa permite que dois computadores joguem entre si. Isso proporciona não só um desafio para os jogadores humanos mas também uma plataforma para testar e avaliar a eficácia das estratégias de IA implementadas.
3. Proporcionar uma experiência de jogo desafiadora e educativa, mas também uma oportunidade de aprendizado sobre algoritmos de inteligência artificial e estratégias de jogo.
   
## Funcionamento Geral

O programa é uma implementação computacional avançada destinada a resolver uma variante desafiadora do problema do Passeio do Cavalo, um clássico da teoria dos jogos e da inteligência artificial. Neste problema, o cavalo do xadrez deve visitar cada casa de um tabuleiro de xadrez 10x10 exatamente uma vez, seguindo as regras tradicionais de movimento dos cavalos no xadrez.
Esta implementação específica difere do problema clássico de várias maneiras significativas:
- Tabuleiro Pontuado: Cada casa no tabuleiro não é apenas uma casa para ser visitada, mas contém uma pontuação que o cavalo acumula ao visitar. O objetivo é maximizar a pontuação total, não apenas visitar todas as casas.
- Regras Especiais: O jogo inclui regras especiais para movimentos que são simétricos ou formam um 'duplo', o que afeta a estratégia de maximização da pontuação.
- Competição: Existe a dimensão de competir contra um oponente, o que adiciona uma camada estratégica à sequência de movimentos escolhida.
- O programa utiliza algoritmos de procura como BFS, DFS e A*, cada um com suas particularidades e heurísticas específicas para navegar no espaço de soluções do problema. As heurísticas são regras ou métodos que ajudam a otimizar a busca pela solução mais eficiente e eficaz.

O programa foi desenhado com uma ênfase na clareza e na facilidade de uso, garantindo que mesmo utilizadores sem experiência prévia em inteligência artificial possam operá-lo e compreender os conceitos subjacentes através de uma interface interativa e instrutiva.

## Como Usar

Para iniciar o jogo é necessário apenas inserir o comando "(configurar-partida)"

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/cc3c0a0d-f551-47a3-a3cd-e31e58d815a7)

Ao inserir o comando é apresentado no ecrã as duas possibilidades de jogo.
Deverá escrever 1 ou 2 dependendo da escolha que pretender.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/9fc8270c-90e6-4111-8152-42b39cb65adc)

Ao inserir a escolha é apresentado uma mensagem para introduzir o tempo limite que o computador tem para jogar.
Inserir um numero entre 1000 a 5000, que corresponde ao tempo em milissegundos.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/a8a78af2-0920-4a73-a695-f999f20cee69)

De seguida é apresentado no ecrã a perguntar quem pretende que começe a jogar.
Deverá escrever 1 ou 2 dependendo da sua escolha.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/6622d953-9f48-43cf-b335-6cbb9bedea87)

Agora o jogo já começou!
É apresentado no ecrã o estádo atual do tabuleiro e as jogadas disponiveis que tem para jogar, essa lista de jogadas apenas aparece na vez do humano jogar.
Escreva o numero da jogada que pretender, por exemplo, das opções da imagem, para selecionar a jogada (2, 1), que move o cavalo duas linhas para baixo e uma coluna para o lado direiro, tem de escrever 1.
No tabuleiro o jogador 1 é representado como -1, e o jogador 2 como -2.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/2f8e3d61-37cc-4533-9c42-5f5f53c7eef3)

Depois de selecionar a jogada, o jogador computador, faz a jogada dele, como poderá observar na proxima imagem, que o jogador 2 já moveu o cavalo

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/919754c3-4d76-4819-8593-4e25e23b993d)

O processo se repete até que nenhum dos jogadores se consigam movimentar mais, e quando isso acontece e mostrado uma mensagem a dizer qual foi o jogador que ganhou e a respetiva pontuação.

![Sem título](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/b80f4be7-7739-4d41-a090-e8c513ec1a2d)

Para o modo de jogo Computador vs Computado, quando começar o jogo neste modo, terá que esperar um pouco e quando terminarem o jogo é apresentado a mensagem de quem ganhou.

![image](https://github.com/TiagoRodrigues201400314/Manual-de-Utilizador/assets/100838766/c29911fe-cb95-466d-94b6-b63598e69104)
