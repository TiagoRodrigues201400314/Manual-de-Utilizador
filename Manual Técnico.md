# Manual Técnico

## Introdução
Este manual técnico documenta o design, a implementação e a análise do programa de inteligência artificial para resolver a variante do problema do Passeio do Cavalo. O objetivo é fornecer uma visão detalhada do funcionamento interno do programa, incluindo os algoritmos utilizados, a estrutura de dados, as heurísticas implementadas e as limitações conhecidas.

## Algoritmo Geral
### Descrição do Algoritmo
#### Ficheiro Procura.lisp

##### Esta função implementa a busca em largura. 
Começa com um nó inicial, expande os nós sucessores que não foram visitados anteriormente, e continua a busca até encontrar uma solução ou esgotar todos os nós possíveis.
```
(defun bfs (no-inicial solucaop sucessores operadores &optional abertos fechados)
  (cond
   ((null no-inicial) nil)
   ((funcall solucaop no-inicial) (list (cons no-inicial fechados) (length abertos) (length fechados) (/ (nth 1 no-inicial) (length abertos))))
   (T (let* (
         (fechados (cons no-inicial fechados))
         (abertos (abertos-bfs abertos (remove nil (mapcar (lambda(x) (cond ((or (no-existep x fechados) (no-existep x abertos)) NIL) (T x))) (funcall sucessores no-inicial operadores 'bfs)))))
         )
      (bfs (car abertos) solucaop sucessores operadores (cdr abertos) fechados)))))
```
##### Esta função implementa a busca em profundidade.
Inicia a busca a partir de um nó inicial, explorando até à profundidade passada em argumento ao longo de cada ramo antes de retroceder.
```
(defun dfs (no-inicial solucaop sucessores operadores profundidade &optional abertos fechados)
  (cond
   ((null no-inicial) nil)
   ((and (= (no-profundidade no-inicial) profundidade) (null abertos)) NIL)
   ((funcall solucaop no-inicial) (list (cons no-inicial fechados) (length abertos) (length fechados) (/ (nth 1 no-inicial) (length abertos))))
   (T (let* (
         (fechados (cons no-inicial fechados))
         (abertos (abertos-dfs abertos (remove nil (mapcar (lambda(x) (cond ((or (no-existep x fechados) (no-existep x abertos)) NIL) (T x))) (funcall sucessores no-inicial operadores 'dfs profundidade)))))
         )
      (dfs (car abertos) solucaop sucessores operadores profundidade (cdr abertos) fechados)))))
```
##### Esta função implementa o algoritmo A*.
É uma busca que utiliza uma função de custo que é a soma de um custo conhecido para alcançar o nó atual mais uma heurística que estima o custo de alcançar o objetivo a partir desse nó.
```
(defun astar (no-inicial solucaop sucessores operadores &optional abertos fechados)
  (cond 
   ((null no-inicial) nil)
   ((funcall solucaop no-inicial) (list fechados (length abertos) (length fechados) (/ (nth 1 no-inicial) (length abertos))))
   (t (let* (
         (fechados (cons no-inicial fechados))
         (abertos (abertos-astar abertos (remove nil (mapcar (lambda(x) (cond ((or (no-existep x fechados) (no-existep x abertos)) NIL) (T x))) (funcall sucessores no-inicial operadores 'a-star))))))
        (astar (car abertos) solucaop sucessores operadores (cdr abertos) fechados)))))
```
##### Esta função gera e retorna a lista de nós abertos para a busca em largura, anexando novos sucessores à lista de abertos.
```
(defun abertos-bfs (abertos sucessores)
  (append abertos sucessores))
```
##### Esta função gera e retorna a lista de nós abertos para a busca em profundidade, colocando novos sucessores no início da lista de abertos.
```
(defun abertos-dfs (abertos sucessores)
  (append sucessores abertos))
```
##### Esta função gera e retorna a lista de nós abertos para o algoritmo A*, ordenando os nós com base em uma função de custo composta pela soma do custo até agora e a heurística.
```
(defun abertos-astar (abertos sucessores)
  (sort (append sucessores abertos) 'comparar-nos))
```
##### Esta função verifica se um nó existe em uma lista de nós.
```
(defun no-existep (no lista)
   (not (null (find (no-estado no) (mapcar #'no-estado lista)))))
```
##### Esta função calcula o custo total de um nó, que é usado no algoritmo A* para ordenar os nós.
```
(defun no-custo (no)
  (+ (heuristica no) (no-profundidade no)))
```
##### Esta função faz a comparação que determina a ordem dos nós na lista de abertos para o A* com base em seus custos totais.
```
(defun comparar-nos (a b)
  (< (no-custo a) (no-custo b)))
```
##### Esta função gera um novo nó sucessor aplicando um operador ao estado do nó.
```
(defun novo-sucessor(no op)
  (list (funcall op (car no)) (+ (no-profundidade no) 1) no))
```
##### Esta função retorna o estado atual de um nó.
```
(defun no-estado(no)
  (car no))
```
##### Esta função cria um novo nó com um estado, um custo de caminho (g), uma heurística (h) e um nó pai.
```
(defun cria-no (estado &optional (g 0) (h 0) (pai nil))
  (list estado g h pai))
```
##### Esta função retorna a profundidade de um nó, ou seja, o número de passos do nó inicial até o nó atual.
```
(defun no-profundidade (no)
  (cadr no))
```
##### Esta função retorna o nó pai de um nó dado, que é usado para rastrear o caminho de volta à solução.
```
(defun no-pai (no)
  (caddr no))
```
##### Esta é função genérica que aplica um algoritmo de busca ao tabuleiro usando operadores especificados.
```
(defun usar-algoritmo (tabuleiro algoritmo operadores)
  (funcall algoritmo (list tabuleiro 0 0 nil) operadores)
)
```
#### Ficheiro Puzzle.lisp

##### Esta função retorna a linha l do tabuleiro tab.
```
(defun linha (l tab)
  (nth l tab)
)
```

##### Esta função retorna a célula na linha l e coluna c do tabuleiro tab.
```
(defun celula (l c tab)
  (nth c (linha l tab))  
)
```

##### Esta função calcula o número simétrico para um número dado numero.
```
(defun numero-simetrico (numero)
  "Calcula o número simétrico."
  (cond 
   ((and (numberp numero) (>= numero 10)) (let ((dezena (floor numero 10))
                                                (unidade (mod numero 10)))
                                            (+ (* unidade 10) dezena)))
   (t nil)
  )
)
```

##### Esta função encontra a posição de um número num no tabuleiro e substitui esse número por nil.
```
(defun encontrar-posicao-numero (tabuleiro num)
  "Encontra a posição de um número no tabuleiro e mete a NIL"
  (let ((posicao (position num (reduce #'append tabuleiro))))
    (cond
     (posicao (let* ((linha (floor (/ posicao (length (first tabuleiro)))))
                     (coluna (mod posicao (length (first tabuleiro)))))
                (substituir linha coluna tabuleiro 'nil)))
     (t tabuleiro)))
)
```

##### Esta função verifica se um número numero é um duplo, de acordo com regras específicas do jogo.
```
(defun numero-duplo (numero)
  "Verifica se um número é um duplo"
  (and (>= numero 11) (<= numero 99) (= (mod numero 10) (floor numero 10)))
)
```

##### Esta função aplica a regra dos duplos no tabuleiro, chamando encontrar-posicao-numero para o maior duplo encontrado.
```
(defun aplicar-regra-duplos (tabuleiro)
  "Aplica a regra dos duplos no tabuleiro."
   (encontrar-posicao-numero tabuleiro (encontrar-maior-duplo tabuleiro))
)
```

##### Esta função aplica a regra de número simétrico ou duplo e retorna o estado atualizado.
```
(defun aplicar-regra-simetrico-duplo (num estado)
  "Aplica a regra do número simétrico ou do duplo e retorna o estado completo."
  (let ((novo-tabuleiro (cond
                         ((numero-duplo num) (aplicar-regra-duplos (car estado)))
                         (t (encontrar-posicao-numero (car estado) (numero-simetrico num))))))
    (list novo-tabuleiro (nth 1 estado) (nth 2 estado))))
```

##### Esta função busca o maior número duplo disponível no tabuleiro.
```
(defun encontrar-maior-duplo (tabuleiro)
  "Busca o maior número duplo disponível no tabuleiro."
  (labels 
      ((busca (num)
         (cond
          ((< num 11) nil)
          ((find num (reduce #'append tabuleiro)) num)
          (t (busca (- num 11)))))
       )
    (busca 99)
  )
)
```

##### Esta função verifica se uma posição (i, j) está dentro dos limites do tabuleiro.
```
(defun valida-posicao (i j)
  "Verifica se a posição está dentro do tabuleiro"
  (and (>= i 0) (< i 10) (>= j 0) (< j 10))
)
```

##### Esta função verifica se a casa na posição (i, j) do tabuleiro está vazia.
```
(defun casa-vazia (i j tabuleiro)
  "Verifica se a casa na posição (i, j) do tabuleiro contém um número."
  (let ((conteudo (nth j (nth i tabuleiro))))
    (and conteudo (not (eql conteudo 'cavalo_branco)) (not (eql conteudo 'cavalo_preto)))
  )
)
```

##### Esta função verifica se uma jogada é válida, ou seja, se está dentro do tabuleiro e a casa está vazia.
```
(defun validacao-jogada (i j tabuleiro)
  "Metodo que verifica todas as possibilidades"
  (cond
   ((and (valida-posicao i j)(casa-vazia i j tabuleiro)) 'T)
   (t nil)
  )
)
```

##### Esta função move o cavalo para uma nova posição (i, j) no tabuleiro, aplicando as regras de movimento e atualizando o estado.
```
(defun move-cavalo (i j estado &optional (cavalo 'T))
  "Move o cavalo no tabuleiro, aplicando as regras de movimentação."
  (let ((posicao-atual (posicao-cavalo (car estado) cavalo)))
    (let* ((novo-i (+ (first posicao-atual) i))
           (novo-j (+ (second posicao-atual) j))
           (novo-numero (celula novo-i novo-j (car estado))))
      (cond 
       ((validacao-jogada novo-i novo-j (car estado)) 
        (let ((tabuleiro-sem-cavalo (substituir (first posicao-atual) (second posicao-atual) (car estado) nil)))
          (let ((novo-estado (list (substituir novo-i novo-j tabuleiro-sem-cavalo cavalo) (adicionar-pontuacao novo-i novo-j estado) (nth 2 estado))))
            (aplicar-regra-simetrico-duplo novo-numero novo-estado))))
       (t nil)
      ))))
```

##### Esta função insere o cavalo em uma posição (i, j) no tabuleiro se a jogada for válida.
```
(defun insert-cavalo (i j tabuleiro &optional (cavalo 'T))
  "Move o cavalo no tabuleiro, aplicando as regras de movimentação."
  (cond 
   ((validacao-jogada i j tabuleiro) (substituir i j tabuleiro cavalo))
   (t nil)
  )
)
```

##### Esta função adiciona a pontuação da célula (i, j) ao estado, se for um número.
```
(defun adicionar-pontuacao (i j estado)
  (let ((valor (celula i j (car estado))))
    (cond 
     ((numberp valor) (+ (nth 1 estado) valor))
      (t (nth 1 estado)))))
```

##### Esta função verifica se o estado atual é uma solução, comparando a pontuação acumulada com um valor de referência.
```
(defun solucaop (estado)
  (cond 
   ((>= (nth 1 estado) (nth 2 estado)) t)
   (t nil)))
```

##### Esta função substitui o valor de uma célula (i, j) no tabuleiro com um novo valor valor.
```
(defun substituir (i j tabuleiro &optional (valor nil))
  (let ((linha (nth i tabuleiro)))
    (let ((nova-linha (substituir-linha j linha valor)))
      (substituir-linha i tabuleiro nova-linha)))
)
```

##### Esta função é auxiliar de substituir, atualiza uma linha do tabuleiro com uma nova linha nova-linha.
```
(defun substituir-linha (indice tabuleiro nova-linha)
  (cond 
   ((null tabuleiro) nil)
   ((= indice 0) (cons nova-linha (cdr tabuleiro)))
   (t (cons (car tabuleiro) (substituir-linha (1- indice) (cdr tabuleiro) nova-linha)))
  )
)
```

##### Esta função encontra a posição atual do cavalo no tabuleiro.
```
(defun posicao-cavalo (tabuleiro &optional (cavalo 'T))
  "Encontra qual a posição do cavalo"
  (labels ((aux (tab i j)
             (cond
               ((null tab) nil)
               ((null (car tab)) (aux (cdr tab) (1+ i) 0)) 
               ((eql cavalo (caar tab)) (list i j)) 
               (t (aux (cons (cdr (car tab)) (cdr tab)) i (1+ j))))))
    (aux tabuleiro 0 0)
  )
)
```

##### Estas funções movem o cavalo em todas as possíveis combinações de movimentos de cavalo no xadrez.
```
(defun move-cavalo-2-1 (estado)
  (funcall 'move-cavalo -2 -1 estado)
)

(defun move-cavalo-2_1 (estado)
  (funcall 'move-cavalo -2 1 estado)
)

(defun move-cavalo-1-2 (estado)
  (funcall 'move-cavalo -1 -2 estado)
)

(defun move-cavalo-1_2 (estado)
  (funcall 'move-cavalo -1 2 estado)
)

(defun move-cavalo_1-2 (estado)
  (funcall 'move-cavalo 1 -2 estado)
)

(defun move-cavalo_1_2 (estado)
  (funcall 'move-cavalo 1 2 estado)
)

(defun move-cavalo_2-1 (estado)
  (funcall 'move-cavalo 2 -1 estado)
)

(defun move-cavalo_2_1 (estado)
  (funcall 'move-cavalo 2 1 estado)
)
```

##### Esta função retorna uma lista de movimentos possíveis que o cavalo pode fazer.
```
(defun lista-movimentos() 
  (list 'move-cavalo_1_2 'move-cavalo_1-2 'move-cavalo-1_2 'move-cavalo-1-2 'move-cavalo_2_1 'move-cavalo_2-1 'move-cavalo-2_1 'move-cavalo-2-1)
)
```

##### Esta função retorna uma lista de operadores de movimento do cavalo a serem utilizados pelos algoritmos de busca.
```
(defun operadores()
  (list 'move-cavalo_1_2 'move-cavalo_1-2 'move-cavalo-1_2 'move-cavalo-1-2 'move-cavalo_2-1 'move-cavalo-2_1 'move-cavalo-2-1))
```

#### Ficheiro Projeto.lisp

##### Esta função lê problemas de um arquivo. O arquivo parece conter vários problemas separados por linhas com "-----".
```
(defun ler-problemas (filename)
  "Chama a função para ler os dados do ficheiro"
  (with-open-file (stream filename) (ler-problemas-aux stream))
)
```

##### Esta é uma função auxiliar que lê as linhas do arquivo recursivamente e constrói uma lista de problemas.
```
(defun ler-problemas-aux (stream &optional problemas problema-atual)
  "Lê os dados do ficheiro"
  (let ((linha (read-line stream nil nil)))
    (cond
     ((null linha)
      (cond 
       (problema-atual (nreverse (push (nreverse problema-atual) problemas)))
       (t problemas)
      ))
     ((string= linha "-----") (ler-problemas-aux stream (push (nreverse problema-atual) problemas) nil))
     (t (ler-problemas-aux stream problemas (push linha problema-atual)))
    ))
)
```

##### Esta função atribui letras (A, B, C, etc.) a cada problema lido do arquivo para fácil referência.
```
(defun mapear-problemas (problemas &optional (letras '("A" "B" "C" "D" "E" "F" "G")) mapeamento)
  "Atribui letras aos dados recebidos"
  (cond 
   ((null problemas) (nreverse mapeamento)) ((null letras) (error "Não há letras suficientes para mapear os problemas."))
   (t (mapear-problemas (cdr problemas) (cdr letras) (push (cons (car letras) (car problemas)) mapeamento)))
  )
)
```

##### Esta função solicita ao utilizador a localização do arquivo de problemas e permite que ele escolha um problema a ser carregado.
```
(defun escolher-problema ()
  (format t "Insira a localização do ficheiro problems.bat: ")
  (let* ((localizacao (read-line))
         (problemas (ler-problemas localizacao))
         (mapeamento (mapear-problemas problemas)))
    (format t "Escolha um problema (A-G):~%")
    (listar-opcoes mapeamento)
    (let ((escolha (string-upcase (read-line))))
      (let ((resultado (obter-problema escolha mapeamento)))
        (cond
         (resultado (format t "Problema escolhido: ~D~%~%" escolha)
                    (processar-escolha resultado))
         (t (format t "Não foi possível encontrar o problema escolhido. Tente novamente.~%") (escolher-problema))
        ))))
)
```

##### Esta função inicia o jogo com o problema escolhido, pedindo ao usuário para posicionar o cavalo e imprimindo o tabuleiro inicial.
```
(defun iniciar-jogo (problema)
  (let ((tabuleiro (cdr problema)))
    (unless (listp (car tabuleiro)) (setf tabuleiro (read-from-string (princ-to-string tabuleiro))))
    (format t "~%Em que casa, na linha 1, deseja colocar o cavalo? ")
    (let ((posicao (parse-integer (read-line) :junk-allowed t)))
      (cond 
       ((and posicao (<= 1 posicao) (<= posicao 10)) (imprimir-tabuleiro (insert-cavalo 0 (1- posicao) tabuleiro)) tabuleiro)
       (t (format t "Posição Inválida!~%") (iniciar-jogo problema))
      )))
)
```

##### Esta função busca um problema específico com base na escolha do usuário no mapeamento de problemas.
```
(defun obter-problema (escolha mapeamento)
  "Isola o problema consoante a escolha do utilizador"
  (let ((problema (assoc escolha mapeamento :test 'string-equal)))
    (cond 
     (problema (let ((tabuleiro (read-from-string (princ-to-string (cadr problema)))))
                 (cons (car problema) tabuleiro)))
     (t (format t "Opção inválida!") (escolher-problema))
    ))
)
```

##### Esta função processa a escolha do problema pelo utilizador, imprimindo o tabuleiro atual e inicia o jogo.
```
(defun processar-escolha (resultado)
  (cond
   (resultado (format t "Tabuleiro atual: ~%")
              (imprimir-tabuleiro (cdr resultado))
              (iniciar-jogo resultado))
   (t (format t "Não foi possível encontrar o problema escolhido. Tente novamente.") nil)
  )
)
```

##### Esta função lista os problemas disponíveis para o utilizador escolher.
```
(defun listar-opcoes (mapeamento)
  "Apresenta as opções ao utilizador"
  (when mapeamento
    (format t "~A - Problema ~A~%" (caar mapeamento) (caar mapeamento))
    (listar-opcoes (cdr mapeamento)))
)
```

##### Esta é uma função auxiliar, que imprime uma linha do tabuleiro.
```
(defun imprimir-linha (linha)
  "Imprime uma linha do tabuleiro."
  (cond 
   ((null linha) (format t "~%"))
   (t (format t "~A " (if (null (car linha)) "NIL" (car linha))) (imprimir-linha (cdr linha)))
  )
)
```

##### Esta função imprime o tabuleiro inteiro, linha por linha.
```
(defun imprimir-tabuleiro (tabuleiro)
  "Imprime o tabuleiro de forma recursiva."
  (cond 
   ((null tabuleiro) nil)
   (t (imprimir-linha (car tabuleiro)) (imprimir-tabuleiro (cdr tabuleiro)))
  )
)
```

##### Esta é a função principal que inicia o jogo, chama a função escolher-problema para carregar um problema e começar a jogar.
```
(defun iniciar ()
  "Inicia o jogo, permitindo ao usuário escolher a localização inicial do cavalo."
  (let* ((tabuleiro (escolher-problema))
         (algoritmo (ler-algoritmo))
         (profundidade (cond 
                         ((eql algoritmo 'dfs) (ler-profundidade))
                         (T 9999)))
         (solucao (usar-algoritmo tabuleiro algoritmo (operadores))))
    (imprimir-tabuleiro (caar solucao)))
)
```

##### Esta função pede ao utilizador para escolher um algoritmo de busca.

```
(defun ler-algoritmo ()
  "Permite fazer a leitura do algoritmo a utilizar."
  (progn
    (format t "Que algoritmo quer usar para procurar? ~%")
    (format t "1- Procura na largura ~%")
    (format t "2- Procura na profundidade ~%")
    (format t "3- Algoritmo A* ~%")
    (let ((resposta (read)))
      (cond 
       ((= resposta 1) 'bfs)
       ((= resposta 2) 'dfs)
       (T 'astar)
      )))
)
```

##### Esta função pede ao utilizador a profundidade limite para o algoritmo de busca em profundidade.
```
(defun ler-profundidade()
  "Permite fazer a leitura da profundidade limite para o algoritmo dfs."
  (progn
    (format t "Qual a profundidade limite? ~%")
    (read))
)
```

## Análise de Complexidade

#### Busca em Largura (BFS):
- O BFS explora todos os vértices e arestas em um grafo. No pior caso, todos os possíveis estados do tabuleiro (nós) serão explorados.
- Complexidade de Tempo: O(B + E), onde B é o número de nós e E é o número de arestas no grafo do problema.

#### Busca em Profundidade (DFS):
- O DFS explora o grafo tão profundamente quanto possível, até ao limite pré-definido, antes de retroceder. No pior caso, também explora todos os vértices e arestas.
- Complexidade de Tempo: O(V + E), onde V é o número de vértices (nós) e E é o número de arestas.

#### A*:
- O A* é mais eficiente do que o BFS e DFS puros porque usa heurísticas para guiar a busca, mas no pior caso, pode explorar muitos nós, especialmente se a heurística não for muito informativa.
- Complexidade de Tempo: O(b^d), onde b é o fator de ramificação (número médio de sucessores por estado) e d é a profundidade da solução mais distante.

### Complexidade de Espaço

#### BFS:
- O BFS precisa armazenar todos os nós que precisam ser explorados e todos os nós que já foram explorados.
- Complexidade de Espaço: O(B), onde B é o número total de nós possíveis.

#### DFS:
- O DFS pode ser mais eficiente em termos de espaço do que o BFS, pois não precisa armazenar todos os nós de um nível antes de passar para o próximo.
- Complexidade de Espaço: O(bm), onde b é o fator de ramificação e m é a profundidade máxima do grafo.

#### A*:
- Semelhante ao BFS em termos de espaço, pois pode precisar armazenar um grande número de nós, especialmente se a heurística não reduzir significativamente o espaço de busca.
- Complexidade de Espaço: Pode ser O(b^d), mas depende da eficácia da heurística.

## Estrutura de Dados

### Listas
Listas são usadas para representar uma variedade de coleções de dados, como:
- Tabuleiros de Jogo: O tabuleiro do jogo pode ser representado como uma lista de listas, onde cada sublista representa uma linha do tabuleiro.
- Estados Abertos e Fechados: Nos algoritmos de procura, listas são usadas para armazenar os estados abertos e fechados.
- Caminhos ou Sequências de Movimentos: Uma lista pode representar a sequência de movimentos que o cavalo faz no tabuleiro.

A estrutura de dados utilizada foi as listas. Por exemplo, ler-problemas-aux cria uma lista de problemas lendo de um arquivo. A função move-cavalo e outras funções relacionadas manipulam estados para realizar movimentos no tabuleiro e calcular pontuações.

## Heurísticas Implementadas
Para este projeto foi apenas implementado a heurística A*, é uma parte crucial que determina a eficiência e eficácia do algoritmo na busca de soluções. 
A heurística é uma função que estima o custo mais baixo do nó atual até o nó objetivo. Tenta prever o caminho mais curto ou custo mínimo para alcançar o objetivo a partir de um determinado ponto. 

Caracteristicas:
- Estimativa de Custo para o Objetivo (h(n)): A função heurística h(n) estima o custo de ir do nó n até o nó objetivo. Esta estimativa deve ser tão precisa quanto possível para que o A* funcione de forma eficiente.
- Admissibilidade: Uma heurística é dita admissível se nunca superestima o custo real de alcançar o objetivo.
- Consistência: Uma heurística é consistente se, para cada nó n e cada sucessor n' de n, o custo estimado de chegar ao objetivo a partir de n não é maior que o custo de ir de n para n' mais o custo estimado de chegar ao objetivo a partir de n'.

## Limitações e Opções Técnicas
Identifique qualquer limitação encontrada durante o desenvolvimento e as escolhas técnicas feitas para contorná-las.

## Análise Crítica dos Resultados
Analise o desempenho do programa, discutindo casos de teste, resultados esperados versus resultados obtidos e quaisquer discrepâncias.

## Análise Comparativa
Compare o desempenho dos diferentes algoritmos e heurísticas. Utilize tabelas e gráficos para ilustrar os resultados.

| Algoritmo | Problema | Resultado | Tempo de Execução |
|-----------|----------|-----------|-------------------|
| BFS       | Problema 1 | X Pontos | YYs              |
| DFS       | Problema 1 | X Pontos | YYs              |
| A*        | Problema 1 | X Pontos | YYs              |

## Requisitos Não Implementados
Liste os requisitos do projeto original que não foram implementados e explique o porquê.

## Conclusão

Este projeto representou uma exploração profunda e prática dos algoritmos de busca em Inteligência Artificial, aplicados ao desafiador e clássico problema do Passeio do Cavalo em um tabuleiro de xadrez. Através da implementação dos algoritmos de Busca em Largura (BFS), Busca em Profundidade (DFS) e A*, foi possível investigar diferentes abordagens para resolver um problema de busca complexo e com múltiplas variantes.

Principais conhecimentos novos e realizações:
- Flexibilidade dos Algoritmos de Busca: O projeto demonstrou como diferentes algoritmos podem ser adaptados e aplicados a um problema específico. Cada algoritmo apresentou suas características únicas em termos de eficiência, profundidade de busca e capacidade de encontrar a solução ótima.
- Importância das Heurísticas no A*: A implementação do A* destacou a importância crucial de uma heurística bem projetada. Foi observado que a eficiência do A* depende fortemente da qualidade da heurística usada, ressaltando o equilíbrio entre admissibilidade e informatividade.
- Desafios de Espaço e Tempo: Ficou evidente que a complexidade tanto em espaço quanto em tempo é um fator significativo nos algoritmos de busca. Enquanto o DFS mostrou-se mais eficiente em termos de espaço, o BFS e o A* foram mais eficazes em encontrar soluções completas, embora com maior consumo de memória.
- Representação do Problema: A representação do tabuleiro e dos movimentos do cavalo em listas provou ser eficaz, permitindo a manipulação e avaliação dos estados do jogo de maneira intuitiva e flexível.
- Conhecimentos sobre IA e Resolução de Problemas: O projeto foi uma oportunidade valiosa para entender melhor como a Inteligência Artificial pode ser aplicada na resolução de problemas complexos, não apenas em jogos, mas em uma variedade de contextos onde a procura e a otimização são necessárias.
- Potencial para Futuras Explorações: O projeto abriu caminhos para investigações futuras, especialmente no aprimoramento de heurísticas para o A* e na exploração de outras variantes do problema do Passeio do Cavalo.

Este projeto não só reforçou o conhecimento adquirido nas aulas dos algoritmos de busca e suas aplicações práticas, mas também destacou a importância da escolha e implementação de estratégias de busca adequadas de acordo com as características específicas do problema em questão.
