1.3
1.3.1

        Colocando no Username: admin ' -- 
    recebemos a resposta: Error you have an error in your SQL syntax; check the manual that correpsonds to your MySQL server version for the right syntax to use near '" --' AND password = 'd41d8(...)" at line 1
    o q não chega
        O problema com a query era que aquilo ficava sempre com username='bob' --' AND (...)
        Aquela ultima plica estava a dar problemas pois o sistema automaticamente colocava uma ' no final da query

        Se fizermos: voldemort' -- //
        O // vai fazer com que a query seja efetivamente temrinada e que não seja adicionada aquela '!

1.3.2

        Neste caso a validaçao que esta a fazer feita é do genero: "SELECT * FROM users where (username='".$user."')
AND password = '.md5($password)."')"
        Ou seja, fazermos voldemort' -- // não chega, pois a query ficaria com um erro sintatico [faltaria o ")" -> (username='voldemort' -- // , Nota a falta do parentisis a fechar]

        Se fizermos: voldemort') -- //
        Ja conseguimos fechar os parentisis, portanto não havera erro sintatico e bish bash bosh, we´re in

NOTA: Tanto no 1.3.1 como na 1.3.2 estamos a atacar a validação pelo User pois a password esta a ser encriptada por com md5 Hash, antes de ser usada. O user porem não esta protegido de qualquer forma, portanto o nosso Goal é atacar fazendo apenas a validação de um user existente, comentando o codigo que verifica a password


1.3.3

        b%' order by 5 -- //
        Order by N esta a ordenar a apresentacao da table pela coluna N. Como a table tem 5 colunas (nota que a 1a esta escondida), podemos ordenar por uma qualquer destas.
        O "b" esta ali para pesquisarmos por produtos que comecem por b, e o % porque a query termina com e LIKE '".$query."%', portanto sem o % a query não retornaria nenhum produto :( 
        
        'union select null,id,username,password,fname from users -- // 
        Substitui o que a tabela mostra. Vai passar a mostrar em cada uma das linhas os campos (especificados por nos) à tabela Users...sooo...yah ta fixe

        'union select null,null,null,null,table_name from information_schema.tables -- //
        O information_schema tem varias informacoes, tabelas, colunas, etc ,etc. Com este comando em especifico ficamos com todas as tabelas da BD :)


1.3.4
        
        O campo vulnerabel é o Username
        Ao injetarmos SQL nesse campo, este codigo é depois executado quando entramos na query de Change Password (Por isso é q isto é uma Injection de 2a Ordem)

        ' or 1 in (select password from users) -- //
        Vai nos dar erro, mas o erro imprime na mesma o campo que queremos hehehe (Warning<: 1292: Truncated incorrect DOUBLE value : (...), onde (...) são as passwords na coluna passwords da tabela users)


1.3.5
        
