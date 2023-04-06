---
title: Errata 3-Ajuste nas configurações das VMs e comandos de alguns contêineres – Nov/2020
subtitle:
image:
alt:

caption:
  title: Errata 3-Ajuste nas configurações das VMs e comandos de alguns contêineres – Nov/2020
  subtitle:
  thumbnail: assets/img/portfolio/blog2.png
  alt: Icons made by [Cornelia Springer](https://www.pngitem.com/userpic/13649/) from [Pngitem](https://www.pngitem.com/middle/iwhTmbo_blogging-png-transparent-png/)
---
Olá, pessoal!

Torço para que vocês e seus familiares fiquem bem em meio à pandemia e desejo saúde aos que precisam!

Durante a leitura do livro, Thiago Augusto Negrão Orbite encontrou alguns problemas e após vários testes e um detalhado troubleshooting encontramos a solução que será detalhada a seguir e que pode ajudar outras pessoas que vierem a ter esses problemas. Thiago, muito obrigado pela confiança e tempo investido!

**Problema 1**: O contêiner do Nexus não estava iniciando por falta de memória.

Lendo o log de cima para baixo, o primeiro erro que ocorre é esse:

```bash
ERROR [FelixStartLevel] *SYSTEM com.orientechnologies.orient.core.storage.impl.local.paginated.OLocalPaginatedStorage - Exception <code>0CDBA14C</code> in storage <code>plocal:/nexus-data/db/OSystem</code>: 2.2.36 (build d3beb772c02098ceaea89779a7afd4b7305d3788, branch 2.2.x)
<strong>java.lang.OutOfMemoryError</strong>: null
at sun.misc.Unsafe.allocateMemory(Native Method)
at java.nio.DirectByteBuffer.(DirectByteBuffer.java:127)
Onde java.lang.OutOfMemoryError é uma mensagem padrão do Java que indica falta de memória.
```

**Solução**: Aumentar a quantidade de memória RAM da VM ci-server de 3 GB para 4GB. Essa VM é utilizada para executar vários outros serviços, tais como: Jenkins, Sonarqube, Gogs e PostgreSQL. Com o passar do tempo novas funcionalidades são adicionadas a estes serviços e para todos funcionarem corretamente na mesma VM é necessário aumentar a quantidade de memória.

**Problema 2**: O contêiner do Nexus não estava iniciando por falta de espaço em disco.

Após corrigir o problema anterior, apareceu no log o seguinte erro:

```bash
<strong>OLowDiskSpaceException</strong>: Error occurred while executing a write operation to database 'OSystem' due to limited free space on the disk (4012 MB). The database is now working in read-only mode. Please close the database (or stop OrientDB), make room on your hard drive and then reopen the database. The minimal required space is 4096 MB. Required space is now set to 4096MB (you can change it by setting parameter storage.diskCache.diskFreeSpaceLimit) .
DB name="OSystem
```

O trecho do log indica que o Nexus precisa de pelo menos 4 GB de espaço em disco para criar o banco de dados.

Solução: Aumentar o tamanho do disco (HD) das VMs de 10 GB para 30 GB.

As soluções dos problemas 1 e 2 foram aplicadas ao arquivo ``capitulo_02/vagrant/Vagrantfile`` conforme mostrado neste commit:

[https://gitlab.com/livro/jenkins/-/commit/eec27883006a2a5854c93e0552e8e975a141538a](https://gitlab.com/livro/jenkins/-/commit/eec27883006a2a5854c93e0552e8e975a141538a)

**Problema 3**: Erro de sintaxe na opção ``--restart`` no comando que inicia os contêineres do Nexus, Jenkins e Sonarqube.

**Solução**: A alteração foi aplicada conforme mostrado nestes commits:

* [https://gitlab.com/livro/jenkins/-/commit/75519b7dfc130667e79712563e72a90d6bac0469](https://gitlab.com/livro/jenkins/-/commit/75519b7dfc130667e79712563e72a90d6bac0469)
* [https://gitlab.com/livro/jenkins/-/commit/0ecfaaa6c8d61332ffd1a38089a320f086d9fe6b](https://gitlab.com/livro/jenkins/-/commit/0ecfaaa6c8d61332ffd1a38089a320f086d9fe6b)
* [https://gitlab.com/livro/jenkins/-/commit/0e6c733d362507fda3bf0289dd7d5e50d9b06a7d](https://gitlab.com/livro/jenkins/-/commit/0e6c733d362507fda3bf0289dd7d5e50d9b06a7d)

Antes:

```bash
--restart always
```

Depois:

```bash
--restart=always
```

**Problema 4**: Obter a senha inicial do Nexus.

**Solução**: A solução já tinha sido publicado em Nov/2019 na Errata 2. Também foi reforçado o comando no arquivo capitulo_02/nexus.txt, conforme mostrado em: [https://gitlab.com/livro/jenkins/-/blob/master/capitulo_02/nexus.txt](https://gitlab.com/livro/jenkins/-/blob/master/capitulo_02/nexus.txt).

**Problema 5**: Erro ao configurar o LDAP utilizando o plugin Jenkins Configuration as a Code (JCasC).

**Solução**: A solução já tinha sido publicado no arquivo capitulo_03/jenkins.yaml, conforme mostrado em: [https://gitlab.com/livro/jenkins/-/blob/master/capitulo_03/jenkins.yaml](https://gitlab.com/livro/jenkins/-/blob/master/capitulo_03/jenkins.yaml).

Este é um plugin que muda muito rápido com o tempo e a configuração também depende do trabalho dos desenvolvedores responsáveis por cada um dos outros 1.500 plugins do Jenkins para manter a compatibilidade a cada nova versão do JCasC.

JCasC releases: [https://github.com/jenkinsci/configuration-as-code-plugin/releases](https://github.com/jenkinsci/configuration-as-code-plugin/releases)

Plugins Jenkins: [https://plugins.jenkins.io](https://plugins.jenkins.io)

Na execução do livro você não precisará ter o Jenkins integrado ao LDAP, mas entendo a importância disso para uso no trabalho e por isso fiz uma rápida pesquisa e separei estes links para quem tiver interesse em saber mais sobre este plugin:

* [https://www.ecanarys.com/Blogs/ArticleID/394/LDAP-Integration-with-Jenkins](https://www.ecanarys.com/Blogs/ArticleID/394/LDAP-Integration-with-Jenkins)
* [https://subscription.packtpub.com/book/application_development/9781784390082/2/ch02lvl1sec35/configuring-the-ldap-plugin](https://subscription.packtpub.com/book/application_development/9781784390082/2/ch02lvl1sec35/configuring-the-ldap-plugin)
* [https://www.praqma.com/stories/start-jenkins-config-as-code/](https://www.praqma.com/stories/start-jenkins-config-as-code/)
* [https://github.com/jenkinsci/configuration-as-code-plugin](https://github.com/jenkinsci/configuration-as-code-plugin)
* [https://docs.google.com/presentation/d/1VsvDuffinmxOjg0a7irhgJSRWpCzLg_Yskf7Fw7FpBg](https://docs.google.com/presentation/d/1VsvDuffinmxOjg0a7irhgJSRWpCzLg_Yskf7Fw7FpBg)
* [https://www.jenkins.io/projects/jcasc](https://www.jenkins.io/projects/jcasc)
* [https://medium.com/preply-engineering/jenkins-omg-275e2df5d647](https://medium.com/preply-engineering/jenkins-omg-275e2df5d647)
* [https://medium.com/@ragin/jenkins-jenkins-configuration-as-code-jcasc-together-with-jobdsl-on-kubernetes-2f5a173491ab](https://medium.com/@ragin/jenkins-jenkins-configuration-as-code-jcasc-together-with-jobdsl-on-kubernetes-2f5a173491ab)
* [https://www.digitalocean.com/community/tutorials/how-to-automate-jenkins-setup-with-docker-and-jenkins-configuration-as-code](https://www.digitalocean.com/community/tutorials/how-to-automate-jenkins-setup-with-docker-and-jenkins-configuration-as-code)
* [https://docs.cloudbees.com/docs/cloudbees-jenkins-distribution/latest/distro-admin-guide/configuration-as-code](https://docs.cloudbees.com/docs/cloudbees-jenkins-distribution/latest/distro-admin-guide/configuration-as-code)
* [https://github.com/jenkinsci/configuration-as-code-plugin/blob/master/README.md](https://github.com/jenkinsci/configuration-as-code-plugin/blob/master/README.md)
* [https://github.com/jenkinsci/configuration-as-code-plugin/blob/master/demos/ldap/README.md](https://github.com/jenkinsci/configuration-as-code-plugin/blob/master/demos/ldap/README.md)
* [https://www.jenkins.io/blog/2018/08/23/speaker-blog-casc-part-1/](https://www.jenkins.io/blog/2018/08/23/speaker-blog-casc-part-1/)
* [https://docs.google.com/presentation/u/1/d/1-irLGTAvMe8Fz1md1zkJtpIpYo9NItZKnsgTYnIqbZU/htmlpresent](https://docs.google.com/presentation/u/1/d/1-irLGTAvMe8Fz1md1zkJtpIpYo9NItZKnsgTYnIqbZU/htmlpresent)
* [https://dzone.com/articles/jenkins-configuration-as-code-documentation](https://dzone.com/articles/jenkins-configuration-as-code-documentation)
* [https://www.admin-magazine.com/Archive/2019/51/Jenkins-Configuration-as-Code](https://www.admin-magazine.com/Archive/2019/51/Jenkins-Configuration-as-Code)

Bons estudos!
