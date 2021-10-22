---
title: Máquinas Virtuais
subtitle:
image:
alt:

caption:
  title: Máquinas Virtuais
  subtitle:
  thumbnail:
---
Para automatizar e simplificar a criação de máquinas virtuais para executar os exercícios do livro, será utilizado o [Vagrant](https://www.vagrantup.com), um software que permite a criação máquinas virtuais no Virtualbox, VmWare e Hiper-V, de forma simples e automatizada agilizando a criação de ambientes de desenvolvimento e testes. Neste livro, o Vagrant será usado em conjunto com o VirtualBox.

As configurações das máquinas virtuais são definidas num arquivo chamado de [Vagrantfile](https://gitlab.com/livro/jenkins/blob/master/capitulo_02/vagrant/Vagrantfile). Ele faz uso da box [Ubuntu_18.04-64-Puppet](https://app.vagrantup.com/aeciopires/boxes/ubuntu-18.04-64-puppet).

Observações:

* Para executar simultaneamente todas as máquinas virtuais é recomendado usar um computador com pelo menos 16 GB de memória RAM. Pode ser usado um computador com 8 GB de memória RAM, mas há grandes chances das operações serem realizadas com baixa performance.

* Como alternativa você pode criar uma conta gratuita no [Docker Hub](https://hub.docker.com) e, em seguida, acessar o site [Play with Docker](https://play-with-docker.com). Neste site, você poderá criar gratuitamente até 5 instâncias de máquinas virtuais, cada uma com 4 GB de memória RAM e com o Docker instalado, para usar por até 4 horas. A única desvantagem é que depois desse tempo, a sessão expirará e todas as instâncias e os dados serão descartados.

* Outra alternativa é criar uma conta gratuita num serviço de computação na nuvem e receber alguns créditos para criar máquinas virtuais e usá-las durante um período de demonstração. Alguns desses serviços são: [Azure](https://azure.microsoft.com/en-us/free), [Digital Ocean](https://goo.gl/dwtvzE), [Google Cloud](https://cloud.google.com/free) e [Amazon AWS](https://aws.amazon.com/free). Ao criar a conta em um desses serviços será solicitado os dados de um cartão de crédito internacional, mas você não será cobrado até usar completamente os créditos oferecidos por cada serviço ou até expirar o período de demonstração.
