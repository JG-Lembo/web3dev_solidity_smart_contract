# Projeto Frase do Dia
Construído na rede Goerli

Site disponível em [https://solidity-smart-contract--jg-lembo.repl.co/](https://solidity-smart-contract--jg-lembo.repl.co/) através do Replit.

## A Plataforma

A plataforma do projeto tem como funcionalidade principal a entrega de uma frase do dia ao usuário, através de sua conexão com uma carteira Ethereum. Para receber a frase do dia, o usuário precisa deixar também uma sugestão de frase. Além da frase, o usuário também consegue ver o endereço da carteira que deixou aquela sugestão.

O usuário só pode deixar uma sugestão por dia, mas pode ver a frase quantas vezes quiser.
Além disso, a página exibe a quantidade de leituras da frase daquele dia e também a quantidade total de leituras feitas desde o início.

Por fim, a cada 100 sugestões feitas, o usuário que fez a sugestão mais recente recebe uma bonificação em Ethereum (de teste, dado que o projeto é construído na rede Goerli).

## O Contrato

O contrato contém o código necessário para execução das funcionalidades da plataforma.

Primeiramente, o construtor inicia o contrato com 3 mensagens do dia, com seu autor sendo registrado como o responsável pelo deploy do contrato.

A função principal, _receiveMessageOfTheDay_, é responsável por receber a sugestão enviada, salvá-la no contrato junto de seu autor, e fornecer a mensagem do dia. Ela garante que o usuário só possa mandar uma sugestão por dia. Isso acontece pois a cada 100 sugestões, a função dá uma pequena bonificação para o autor da mensagem. Por isso a trava de uma sugestão por dia existe, evitando spam de sugestões para obtenção da bonificação.

A função garante também que a mensagem troque a cada dia, mas apenas em dias que o contrato for acionado. Isso é, se no dia 4 a mensagem 2 foi mostrada, e não houver nenhum acesso nos dias 5 e 6, o dia 7 exibirá a mensagem 3, se acessado. Isso garante que o número de mensagens do dia nunca acabe, visto que para cada nova leitura é necessário deixar uma sugestão.

O contrato também armazena o número de leituras daquela mensagem e o número total de leituras, que podem ser recuperadas pela função _getReads_.

Por fim, as funções _checkUserSentMessage_ e _getMessageOfTheDay_ servem para garantir que um usuário que já recebeu a mensagem do dia possa vê-la novamente sem deixar uma sugestão, isso é, sem realizar uma transação. Com a primeira, o frontend pode confirmar que o usuário já enviou uma sugestão hoje, e então usar a segunda para apenas pegar a mensagem, sem exigir uma sugestão, sem alterar o número de leituras e sem interferências nas chances de bonificação.
