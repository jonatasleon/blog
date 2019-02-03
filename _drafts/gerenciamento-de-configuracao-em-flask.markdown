---
layout: "post"
title: "Gerenciamento de configuração em Flask"
date: "2019-02-03 02:26"
---

Como gerenciar a injeção de configuração dentro de aplicações Flask com python-decouple.

Em Flask, nunca foi trivial gerenciar configurações conforme um ambiente, por exemplo, enquanto em ambiente de desenvolvimento, suponha que você deseja utilizar uma instância do sqlite com uma amostra de dados, assim você define que `app.config['SQLALCHEMY_DATABASE_URI']` seja igual a `'sqlite:////tmp/dev.db'`, agora em produção você utiliza uma instância de postgresql, então a a URI será, por exemplo, igual a `'postgresql://scott:tiger@localhost:5432/mydatabase'`. Na própria [documentação do Flask][07581c34] há uma seção que detalha como definir tais configurações. Contudo, há algumas situações que precisam ser levadas em considerações para que você tenha uma forma eficiente de definir configuração em função do ambiente de execução, são algumas delas:
- Servidor HTTP (Apache2, Nginx);
- Processo de _deployment_ (manual, semi-automatizado, _CD_);
- Múltiplas instâncias (_celery_, instâncias distribuídas);
- Dados sensíveis que não devem ser adicionar ao sistema de controle de versão (_git_, _svn_).

Pensando nisso, levei um certo tempo para adequar um fluxo que se adequa-se as minhas necessidades. Em cima dessas necessidades criei um fluxo que atende de forma "elegante"

  [07581c34]: http://flask.pocoo.org/docs/1.0/config/#configuring-from-files "Configuring from Files"
  [28e4925a]: https://docs.sqlalchemy.org/en/latest/core/engines.html "Engine Configuration"
