---
title: Errata 2 - Ajuste na configuração do Nexus – Nov/2019
subtitle:
image:
alt:

caption:
  title: Errata 2 - Ajuste na configuração do Nexus – Nov/2019
  subtitle:
  thumbnail:
---
Olá, pessoal!

Com as novas versões do Sonatype Nexus lançadas ao longo de 2019, é necessário realizar o passo a passo abaixo para obter a senha inicial e permitir acesso anônimo aos artefatos hospedados no Nexus para que o pipeline dos capítulos 8 e 9 do livro funcionem corretamente.

A senha inicial do Nexus pode ser obtida com o comando a seguir.

```bash
cat /docker/nexus/data/admin.password
```

Pode ser que demore alguns minutos para o arquivo ser criado após o conteiner do Nexus ter sido executado pela primeira vez.

Acesse o nexus hospedado na porta 8081/TCP do host ci-server.domain.com.br. Faça login no Nexus, com o login admin e a senha inicial. Caso tenha alterado a senha durante o primeiro acesso do Nexus, informe a nova senha cadastrada. Será solicitado uma nova senha.

Quando for perguntado se deve permitir acesso anônimo ao Nexus, diga que sim.

Caso, não tenha habilitado o acesso anônimo durante o primeiro acesso ao Nexus, acesse o menu **Security > Anonymous**. Marque a opção **Allow anonymous users to access the server**, conforme mostra a figura a seguir. Depois clique em **Save**.

 <p align="center">
    <img src="assets/img/portfolio/anonymous-revised.png">
 </p>

Pronto! Pode continuar seguindo as instruções do livro.

Bons estudos!
