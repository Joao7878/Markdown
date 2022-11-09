# Sistemas Operacionais

## Definição

É um programa de computador que atua como intermediário entre o usuário e o hardware.  
O kernel é o primeiro programa que é iniciado no momento em que o sistema operacional se inicia, ou seja, o kernel é um programa do sistema operacional. O kernel realiza a comunicação direta do sistema operacional com o hardware da máquina.

## Características

Um sistema operacional tem como base 3 características predominantes:

- Transparência
- -Simplifica as atividades para o usuário
- Gerência
- - Compartilha e otimiza os recursos computacionais
- Encapsulamento
- - Esconde os detalhes da implementação;
- - "Padroniza" a interface, muitas vezes mesmo entre dispositivos diferentes.

## Tipos de SO

Existem 3 tipos de SO:

- Monoprogramáveis/Monotarefa
- - Uma tarefa por vez;
- - Implementação Simples;
- - Ex:Mainframes
- Multiprogramáveis/Multitarefa
- - Compartilha recursos do sistemas entre diversas tarefas;
- - Aumenta produtividade;
- - Redução de custos;
- - Implementação complexa;
- - Ex: Computadores pessoais
- Multiprocessados
- - São sistemas multitarefas, porém possuem mais de uma CPU;
- - Ideal para sistemas que necessitam bastante do uso da CPU, ex: Computadores científicos, multiprogramação, supercomputadores;
- - Tolerância à falha em algum processador;
- - Distribuição de carga de processamento;
- - Execução simultânea de programas
- - Os sistemas multiprocessados se dividem em dois, os fortemente acoplados (Processadores com vários núcleos[Compartilham uma única memória]) e os fracamente acoplados(Server Cluster(não estão tão acoplados, ou seja, possuem memória e dispositivos de I/O distintos) [Sistemas Operacionais de redes por exemplo]);

## Sistemas de Tempo Real

São sistemas que agem em tempos limite de resposta extremamente rígidos.  
Cada tarefa tem determinado tempo de resposta.  
Trabalha com níveis de prioridade de tarefas para serem executadas.  
Sem fatia de tempo.  
Ex: Sistema operacional de um avião(cada tarefa do sistema precisa ser executada de maneira efetiva e rápida)

## Tipos de sistemas multitarefas

- Colaborativo
- - Windows 95,98
- - Não existe fatia de tempo
- Preemptivo
- - Windows 10, Linux, Mac OS,...
- - Existe fatia de tempo
- - Preempção
