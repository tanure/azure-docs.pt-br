---
title: Rotação de certificado para o banco de dados do Azure para PostgreSQL servidor único
description: Saiba mais sobre as alterações futuras de alterações de certificado raiz que afetarão o banco de dados do Azure para PostgreSQL servidor único
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 96720e156963a5fb542e72823a602aa2cc6a0ead
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348994"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>Noções básicas sobre as alterações na AC raiz alterar para o banco de dados do Azure para PostgreSQL servidor único

O banco de dados do Azure para PostgreSQL alterará o certificado raiz para o aplicativo/driver cliente habilitado com SSL, usado para [se conectar ao servidor de banco de dados](concepts-connectivity-architecture.md). O certificado raiz atualmente disponível está definido para expirar em 15 de fevereiro de 2021 (02/15/2021) como parte das práticas recomendadas de manutenção e segurança padrão. Este artigo fornece mais detalhes sobre as alterações futuras, os recursos que serão afetados e as etapas necessárias para garantir que seu aplicativo mantenha a conectividade com o servidor de banco de dados.

>[!NOTE]
> Com base nos comentários dos clientes, estendemos a substituição do certificado raiz para nossa CA raiz Baltimore existente de outubro de 26, 2020 até 15 de fevereiro de 2021. Esperamos que essa extensão forneça tempo de avanço suficiente para que nossos usuários implementem as alterações do cliente se elas forem afetadas.

## <a name="what-update-is-going-to-happen"></a>Qual atualização vai acontecer?

Em alguns casos, os aplicativos usam um arquivo de certificado local gerado por meio de um arquivo de certificado de AC (autoridade de certificação) confiável para se conectar com segurança. Atualmente, os clientes podem usar apenas o certificado predefinido para se conectar a um servidor do banco de dados do Azure para PostgreSQL, que está localizado [aqui](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem). No entanto, o [Fórum do navegador da AC (autoridade](https://cabforum.org/) de certificação) publicou recentemente relatórios de vários certificados emitidos por fornecedores de autoridade de certificação para não estar em conformidade.

De acordo com os requisitos de conformidade do setor, os fornecedores de CA começaram a revogar certificados de CA para CAs não compatíveis, exigindo que os servidores usem certificados emitidos por CAs compatíveis e assinados por certificados de autoridade de certificação dessas CAs em conformidade. Como o banco de dados do Azure para PostgreSQL atualmente usa um desses certificados não compatíveis, que aplicativos cliente usam para validar suas conexões SSL, precisamos garantir que as ações apropriadas sejam tomadas (descritas abaixo) para minimizar o impacto potencial nos servidores PostgreSQL.

O novo certificado será usado a partir de 15 de fevereiro de 2021 (02/15/2021). Se você usar a validação de autoridade de certificação ou a validação completa do certificado do servidor ao conectar-se de um cliente PostgreSQL (sslmode = Verify-CA ou sslmode = Verify-Full), será necessário atualizar a configuração do aplicativo antes de 15 de fevereiro de 2021 (02/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Como fazer saber se meu banco de dados será afetado?

Todos os aplicativos que usam SSL/TLS e verificam se o certificado raiz precisa atualizar o certificado raiz. Você pode identificar se suas conexões verificam o certificado raiz examinando a cadeia de conexão.
-    Se a cadeia de conexão incluir `sslmode=verify-ca` ou `sslmode=verify-full` , você precisará atualizar o certificado.
-    Se a cadeia de conexão incluir `sslmode=disable` ,, `sslmode=allow` `sslmode=prefer` ou `sslmode=require` , você não precisará atualizar os certificados. 
-    Se a cadeia de conexão não especificar sslmode, você não precisará atualizar os certificados.

Se você estiver usando um cliente que abstrai a cadeia de conexão, examine a documentação do cliente para entender se ele verifica os certificados.

Para entender o PostgreSQL sslmode, consulte as [descrições do modo SSL](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) na documentação do PostgreSQL.

Para evitar que a disponibilidade do aplicativo seja interrompida devido aos certificados serem revogados inesperadamente ou para atualizar um certificado, que foi revogado, consulte a seção [**"o que preciso fazer para manter a conectividade"**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) .

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>O que preciso fazer para manter a conectividade

Para evitar que a disponibilidade do aplicativo seja interrompida devido aos certificados serem revogados inesperadamente ou para atualizar um certificado, que foi revogado, siga as etapas abaixo. A ideia é criar um novo arquivo *. pem* , que combina o certificado atual e o novo e durante a validação do certificado SSL uma vez que os valores permitidos serão usados. Consulte as etapas abaixo:

*   Baixe o BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 root CA dos links abaixo:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Gere um repositório de certificados de autoridade de certificação combinado com os certificados **BaltimoreCyberTrustRoot** e **DigiCertGlobalRootG2** incluídos.
    *   Para usuários do Java (PostgreSQL JDBC) usando DefaultJavaSSLFactory, execute:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Em seguida, substitua o arquivo de keystore original pelo novo gerado:
        *   System. SetProperty ("javax. net. SSL. trustStore", "path_to_truststore_file"); 
        *   System. SetProperty ("javax. net. SSL. trustStorePassword", "senha");
        
    *   Para usuários do .NET (Npgsql) no Windows, certifique-se de que **Baltimore Cybertrust raiz** e **DigiCert raiz global G2** existam no repositório de certificados do Windows, autoridades de certificação raiz confiáveis. Se não houver nenhum certificado, importe o certificado ausente.

        ![Banco de dados do Azure para PostgreSQL certificado .net](media/overview/netconnecter-cert.png)

    *   Para usuários do .NET (Npgsql) no Linux usando SSL_CERT_DIR, verifique se **BaltimoreCyberTrustRoot** e **DigiCertGlobalRootG2** existem no diretório indicado por SSL_CERT_DIR. Se não houver nenhum certificado, crie o arquivo de certificado ausente.

    *   Para outros usuários cliente do PostgreSQL, você pode mesclar dois arquivos de certificado de autoridade de certificação como este formato abaixo

        </br>-----INICIAR-----DE CERTIFICADO
 </br>(CA1 raiz: BaltimoreCyberTrustRoot. CRT. pem)
 </br>----------DE CERTIFICADO FINAL
 </br>-----INICIAR-----DE CERTIFICADO
 </br>(CA2 raiz: DigiCertGlobalRootG2. CRT. pem)
 </br>----------DE CERTIFICADO FINAL

*   Substitua o arquivo PEM da autoridade de certificação raiz original pelo arquivo de autoridade de certificação raiz combinado e reinicie o aplicativo/cliente.
*    No futuro, após o novo certificado implantado no lado do servidor, você poderá alterar o arquivo PEM da autoridade de certificação para DigiCertGlobalRootG2. CRT. PEM.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Qual pode ser o impacto de não atualizar o certificado?
Se você estiver usando o certificado raiz Baltimore CyberTrust para verificar a conexão SSL com o banco de dados do Azure para PostgreSQL, conforme documentado aqui, a disponibilidade do aplicativo poderá ser interrompida, pois o banco de dados não estará acessível. Dependendo do seu aplicativo, você pode receber uma variedade de mensagens de erro, incluindo, mas não se limitando a:
*    Certificado inválido/certificado revogado
*    A conexão atingiu o tempo limite

> [!NOTE]
> Não remova nem altere o **certificado Baltimore** até que a alteração de certificado seja feita. Enviaremos uma comunicação depois que a alteração for feita, após a qual é seguro descartar o certificado Baltimore. 

## <a name="frequently-asked-questions"></a>Perguntas frequentes

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. se eu não estiver usando SSL/TLS, ainda precisarei atualizar a autoridade de certificação raiz?
Nenhuma ação será necessária se você não estiver usando SSL/TLS. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. se eu estiver usando SSL/TLS, preciso reiniciar meu servidor de banco de dados para atualizar a AC raiz?
Não, você não precisa reiniciar o servidor de banco de dados para começar a usar o novo certificado. Essa é uma alteração no lado do cliente e as conexões de entrada do cliente precisam usar o novo certificado para garantir que eles possam se conectar ao servidor de banco de dados.

### <a name="3-what-will-happen-if-i-do-not-update-the-root-certificate-before-february-15-2021-02152021"></a>3. o que acontecerá se eu não atualizar o certificado raiz antes de 15 de fevereiro de 2021 (02/15/2021)?
Se você não atualizar o certificado raiz antes de 15 de fevereiro de 2021 (02/15/2021), seus aplicativos que se conectam via SSL/TLS e a verificação para o certificado raiz não poderão se comunicar com o servidor de banco de dados PostgreSQL e o aplicativo terá problemas de conectividade com o servidor de banco de dados PostgreSQL.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. qual é o impacto se estiver usando o serviço de aplicativo com o banco de dados do Azure para PostgreSQL?
Para os serviços de aplicativos do Azure, conectando-se ao banco de dados do Azure para PostgreSQL, podemos ter dois cenários possíveis e depende de como você está usando SSL com seu aplicativo.
*   Este novo certificado foi adicionado ao serviço de aplicativo no nível da plataforma. Se você estiver usando os certificados SSL incluídos na plataforma do serviço de aplicativo em seu aplicativo, nenhuma ação será necessária.
*   Se você estiver incluindo explicitamente o caminho para o arquivo de certificado SSL em seu código, precisará baixar o novo certificado e atualizar o código para usar o novo certificado. Um bom exemplo desse cenário é quando você usa contêineres personalizados no serviço de aplicativo como compartilhado na [documentação do serviço de aplicativo](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress)

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. qual é o impacto se estiver usando o AKS (serviços Kubernetess do Azure) com o banco de dados do Azure para PostgreSQL?
Se você estiver tentando se conectar ao banco de dados do Azure para PostgreSQL usando os AKS (serviços Kubernetess do Azure), ele será semelhante ao acesso de um ambiente de host de clientes dedicados. Consulte as etapas [aqui](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. qual é o impacto se você estiver usando Azure Data Factory para se conectar ao banco de dados do Azure para PostgreSQL?
Para o conector usando Azure Integration Runtime, o conector aproveita os certificados no repositório de certificados do Windows no ambiente hospedado do Azure. Esses certificados já são compatíveis com os certificados recém aplicados e, portanto, nenhuma ação é necessária.

Para o conector que usa o autohospedado Integration Runtime em que você inclui explicitamente o caminho para o arquivo de certificado SSL na cadeia de conexão, será necessário baixar o [novo certificado](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) e atualizar a cadeia de conexão para usá-lo.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. preciso planejar um tempo de inatividade de manutenção do servidor de banco de dados para essa alteração?
Não. Como a alteração aqui é apenas no lado do cliente para se conectar ao servidor de banco de dados, não há nenhum tempo de inatividade de manutenção necessário para o servidor de banco de dados para essa alteração.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. e se eu não conseguir um tempo de inatividade agendado para essa alteração antes de 15 de fevereiro de 2021 (02/15/2021)?
Como os clientes usados para se conectar ao servidor precisam atualizar as informações do certificado, conforme descrito na seção corrigir [aqui](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity), não precisamos de um tempo de inatividade para o servidor nesse caso.

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9. se eu criar um novo servidor após 15 de fevereiro de 2021 (02/15/2021), serei afetado?
Para servidores criados após 15 de fevereiro de 2021 (02/15/2021), você pode usar o certificado emitido recentemente para seus aplicativos se conectarem usando SSL.

###    <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. com que frequência o Microsoft atualiza seus certificados ou qual é a política de expiração?
Esses certificados usados pelo banco de dados do Azure para PostgreSQL são fornecidos por autoridades de certificação (CA) confiáveis. Portanto, o suporte desses certificados no banco de dados do Azure para PostgreSQL está vinculado ao suporte desses certificados pela CA. No entanto, como nesse caso, pode haver bugs imprevistos nesses certificados predefinidos, que precisam ser corrigidos no início.

###    <a name="11-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>11. se eu estiver usando réplicas de leitura, preciso executar essa atualização somente no servidor primário ou nas réplicas de leitura?
Como essa atualização é uma alteração no lado do cliente, se o cliente usado para ler dados do servidor de réplica, você também precisará aplicar as alterações para esses clientes. 

### <a name="12-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>12. temos uma consulta do lado do servidor para verificar se o SSL está sendo usado?
Para verificar se você está usando a conexão SSL para se conectar ao servidor, consulte [verificação de SSL](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity).

### <a name="13-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>13. há uma ação necessária se eu já tiver o DigiCertGlobalRootG2 no meu arquivo de certificado?
Não. Não há nenhuma ação necessária se o arquivo de certificado já tiver o **DigiCertGlobalRootG2**.

### <a name="14-what-is-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>14. o que você está usando a imagem do Docker da PgBouncer sidecar fornecida pela Microsoft?
Uma nova imagem do Docker que dá suporte a [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) e [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) é publicada abaixo [aqui](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) (marca mais recente). Você pode extrair essa nova imagem para evitar qualquer interrupção na conectividade a partir de 15 de fevereiro de 2021. 

###    <a name="15-what-if-i-have-further-questions"></a>15. e se eu tiver outras dúvidas?
Se você tiver dúvidas, obtenha respostas de especialistas da Comunidade no [Microsoft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com). Se você tiver um plano de suporte e precisar de ajuda técnica,  [entre em contato conosco](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)
