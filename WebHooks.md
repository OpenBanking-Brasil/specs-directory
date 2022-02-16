# Webhooks do diretório de participantes

Para o cadastro e objetivos do webhooks no diretório de participantes pedimos que consultem o [Guia de Operação do Diretório do Open Banking](https://openbanking-brasil.github.io/areadesenvolvedor/#guia-operacional-do-diretorio-central)

## Relação de eventos que acionam um webhook no diretório:

**Criação, update ou delete:**
- Organisations (organizações)
- API Discovery Endpoint (Endpoints de descoberta de uma API)
- API Resource (Recursos de API)
- Contacts (contatos de uma organização)
- Auth Server Certification (Certificação de um servidor de uma organização)
- Org Auth Server (Servidor de autorização de uma organização)
- Org Auth Domain Claim (Pedido de autorização de domínio de uma organização)
- Org Auth Domain Role Claim (Pedido de Autorização de Organização do Papel de Domínio)

**Criação ou update:**
- Software Authority Claim (Pedido de autoridade de software)

**Criação ou delete:**
- Org Auth Domain Role Authorisation (Autorização de Organização Autorização de Papel)

**Criação ou revogação:**
- Org or Software Certificate (Certificado de organização ou de software)

**Criação, update, delete, suspensão, retirada de suspensão, bloqueamento e desbloqueamento:**
- Software Statement

**Outros:**
- Termos e condições de uma organização: Iniciação do processo de assinatura. Durante o processo, caso haja uma mudança no status

## Payloads das notificações

## Estrutura Básica do Payload

Os payloads dos webhooks são construídos seguindo uma estrutura de _geral até o evento_. Ou seja, para cada evento acionado, será detalhado dentro do payload response a estrutura inteira que o compõe.

Assim, em cada payload você consegue ver e ler toda a cadeia da qual aquele evento pertence, facilitando a consulta.

Como exemplo, temos um webhook notificando abaixo a criação de um servidor de autorização:

![image](https://user-images.githubusercontent.com/94750959/154345856-f0de0db0-a225-4900-895d-4ddfd52d6f23.png)

Note que a linha de Revision fica no mesmo nível do evento.

Um outro exemplo de criação de um novo software authority claim, vemos que o payload mostra a organização, o software statement e depois o objeto que foi atualizado.

![image (1)](https://user-images.githubusercontent.com/94750959/154346538-ffc4389c-a63a-4d38-993c-c72c4f4ba850.png)


Em relação aos campos, o payload segue a mesma estrutura das [APIs do Diretório](https://github.com/OpenBanking-Brasil/specs-directory/blob/main/openapi.yaml)

## Exemplos de payloads

### Exemplos de payloads - Inscrição

#### Payload de inscrição no webhook de um servidor de autorização (subscription)

```
  {
  "Type": "SubscriptionConfirmation",
  "MessageId": "139ff0c2-2538-4d84-8ecb-a86197bd716d",
  "Token": "2336412f37fb687f5d51e6e2425dacbbad1d5daadd816283d7ed719b1a6a8400ab53cdc4cbc03db137fe33fb4c223c3fbc6e3aab09cbf7a04942589aedd95b7f282e638231e780f3c5897424fb581c38796d209f0ea79708cc79b87de83ed24b3e3de35a58129cc239b1fe9df66875d4ac1169828e810a6b1c129ceffe3176bce3f59dd2e7a30d40c734a320437bfb65",
  "TopicArn": "arn:aws:sns:us-west-2:574283098123:obbrasil-sandbox-webhook_notifications",
  "Message": "You have chosen to subscribe to the topic arn:aws:sns:us-west-2:574283098123:obbrasil-sandbox-webhook_notifications.\nTo confirm the subscription, visit the SubscribeURL included in this message.",
  "SubscribeURL": "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:574283098123:obbrasil-sandbox-webhook_notifications&Token=2336412f37fb687f5d51e6e2425dacbbad1d5daadd816283d7ed719b1a6a8400ab53cdc4cbc03db137fe33fb4c223c3fbc6e3aab09cbf7a04942589aedd95b7f282e638231e780f3c5897424fb581c38796d209f0ea79708cc79b87de83ed24b3e3de35a58129cc239b1fe9df66875d4ac1169828e810a6b1c129ceffe3176bce3f59dd2e7a30d40c734a320437bfb65",
  "Timestamp": "2022-02-16T00:55:32.720Z",
  "SignatureVersion": "1",
  "Signature": "sdogl2e89rqjDdbxM3Sj842hfsezNPy4uMcecQbjYwpgQgzNCXBGrUhLlLrvS+xR+yXxyYJJGpbAMCRtl51naQNaecEhI1Xb84c2oejmdw3G+Z8UkamCsmJ18DqRs05RqeVgKhAmMMElyi4vT9Qwahs0ABVhpLvA40Ao73qfDNULmICJAUJxLrBLgKM17SU4/M6XhJnoYc47JlVeJGCpU4tW961oZrLf+Ku1Fbs7DkLQIpM/ucX5REZk32fNCB8au5asJkE3a6wSWVJ2yQfj1d+OIlrktJFJf1ucQWf1JWEQ4WRrBkPpZ5ruLT4j45RkqxMDKvA4vMzB6Od+p1cyyA==",
  "SigningCertURL": "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-7ff5318490ec183fbaddaa2a969abfda.pem"
  }
```
### Exemplo Payload Alteração Authorisation Server

#### Payload de um novo Recurso de API (Api Resource)
```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"AuthorisationServers": [
{
"AuthorisationServerId": "856e62aa-6891-48fa-b8de-4f365d4b22e5",
"AutoRegistrationSupported": false,
"SupportsCiba": false,
"SupportsDCR": false,
"ApiResources": [
{
"ApiResourceId": "96249106-e6b5-4041-8ac1-0b66d7e1bad7",
"ApiVersion": 1,
"FamilyComplete": false,
"CertificationStatus": "Awaiting Certification",
"ApiFamilyType": "admin",
"Revision": 38339
}
]
}
]
}

```

#### Payload da adição de um novo Api Endpoint (admin)
```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"AuthorisationServers": [
{
"AuthorisationServerId": "856e62aa-6891-48fa-b8de-4f365d4b22e5",
"AutoRegistrationSupported": false,
"SupportsCiba": false,
"SupportsDCR": false,
"ApiResources": [
{
"ApiResourceId": "96249106-e6b5-4041-8ac1-0b66d7e1bad7",
"ApiDiscoveryEndpoints": [
{
"ApiDiscoveryId": "c1ad4290-9b15-4ce6-a5f0-fc186985710b",
"ApiEndpoint": "https://teste.com.br/open-banking/admin/v1/metrics",
"Revision": 38341
}
],
"CertificationStatus": "Awaiting Certification"
}
]
}
]
}
```
#### Payload Criação Authorization Server

```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "AuthorisationServers": [
    {
      "AuthorisationServerId": "e2f917ad-a058-42a9-82eb-844b0dbf879d",
      "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
      "AutoRegistrationSupported": true,
      "SupportsCiba": false,
      "SupportsDCR": false,
      "CustomerFriendlyDescription": "https://www.naoseaplica.com",
      "CustomerFriendlyLogoUri": "https://www.naoseaplica.com/file.svg",
      "CustomerFriendlyName": "Webhook Test Server 2",
      "OpenIDDiscoveryDocument": "https://www.naoseaplica.com",
      "PayloadSigningCertLocationUri": "https://www.naoseaplica.com",
      "Revision": 38440
    }
  ]
}

```

#### Payload Remoção Authorization Server

```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "AuthorisationServers": [
    {
      "AuthorisationServerId": "e2f917ad-a058-42a9-82eb-844b0dbf879d",
      "AutoRegistrationSupported": false,
      "SupportsCiba": false,
      "SupportsDCR": false,
      "Revision": 38441
    }
  ]
}

```
#### Payload Alteração Authorization Server
```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "AuthorisationServers": [
    {
      "AuthorisationServerId": "856e62aa-6891-48fa-b8de-4f365d4b22e5",
      "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
      "AutoRegistrationSupported": true,
      "SupportsCiba": false,
      "SupportsDCR": false,
      "ApiResources": [
        {
          "ApiResourceId": "96249106-e6b5-4041-8ac1-0b66d7e1bad7",
          "ApiVersion": 1,
          "ApiDiscoveryEndpoints": [
            {
              "ApiDiscoveryId": "c1ad4290-9b15-4ce6-a5f0-fc186985710b",
              "ApiEndpoint": "https://teste.com.br/open-banking/admin/v1/metrics"
            }
          ],
          "FamilyComplete": true,
          "CertificationStatus": "Awaiting Certification",
          "ApiFamilyType": "admin"
        }
      ],
      "CustomerFriendlyDescription": "teste para webhook - edição",
      "CustomerFriendlyLogoUri": "https://www.naoseaplica.com/file.svg",
      "CustomerFriendlyName": "Webhook Test Server",
      "TermsOfServiceUri": "https://www.naoseaplica.com",
      "NotificationWebhook": "https://webhook.site/42b537c6-9e21-4387-868f-2d4e54e9bde5",
      "NotificationWebhookStatus": "Confirmed",
      "NotificationWebhookAddedDate": "2022-02-16",
      "OpenIDDiscoveryDocument": "https://www.naoseaplica.com",
      "PayloadSigningCertLocationUri": "https://www.naoseaplica.com",
      "Revision": 38439
    }
  ]
}
```



### Exemplo Payload Alteração Software Statement

#### Payload da adição de um novo Software Statement

```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"SoftwareStatements": {
"39c82433-0f57-4653-b513-01b03997ff0f": {
"SoftwareDetails": {
"Status": "Active",
"ClientId": "39c82433-0f57-4653-b513-01b03997ff0f",
"ClientName": "Webhook test SS",
"Description": "Teste de web hook",
"Environment": "Sandbox",
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
"SoftwareStatementId": "39c82433-0f57-4653-b513-01b03997ff0f",
"Mode": "Live",
"RtsClientCreated": true,
"ClientUri": "https://testss.com.br/asdfx",
"LogoUri": "https://www.teste.com/openbanking/logo_ccb.svg",
"RedirectUri": [
"https://openid.net/wordpress-content/uploads/2021/08/Cloud"
],
"Version": 1,
"Locked": false,
"AdditionalSoftwareMetadata": "{}",
"Revision": 38343
}
}
}
}
```

#### Payload de update de um Software Statement

```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"SoftwareStatements": {
"e86671e8-ee21-4b09-93b2-293e4324ac70": {
"SoftwareDetails": {
"Status": "Active",
"ClientId": "OgG1zd-dXjoujvZSzlrn8",
"ClientName": "Teste LOGO",
"Description": "teste de logo url www2 ww3",
"Environment": "Sandbox",
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
"SoftwareStatementId": "e86671e8-ee21-4b09-93b2-293e4324ac70",
"Mode": "Live",
"RtsClientCreated": true,
"PolicyUri": "https://testss.com.br/asdfx",
"ClientUri": "https://testss.com.br/asdfx",
"LogoUri": "https://www2.br.ccb.com/openbanking/logo_ccb.svg",
"RedirectUri": [
"https://openid.net/wordpress-content/uploads/2021/08/Cloud"
],
"Version": 1,
"Locked": false,
"AdditionalSoftwareMetadata": "{}",
"Revision": 38342
}
}
}
}
```

#### Payload de delete de um Software Statement

```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"SoftwareStatements": {
"e86671e8-ee21-4b09-93b2-293e4324ac70": {
"SoftwareDetails": {
"Status": "Inactive",
"ClientName": "Teste LOGO",
"Description": "teste de logo url www2 ww3",
"Environment": "Sandbox",
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
"SoftwareStatementId": "e86671e8-ee21-4b09-93b2-293e4324ac70",
"Mode": "Live",
"RtsClientCreated": false,
"PolicyUri": "https://testss.com.br/asdfx",
"ClientUri": "https://testss.com.br/asdfx",
"LogoUri": "https://www2.br.ccb.com/openbanking/logo_ccb.svg",
"RedirectUri": [
"https://openid.net/wordpress-content/uploads/2021/08/Cloud"
],
"Version": 1,
"Locked": false,
"AdditionalSoftwareMetadata": "{}",
"Revision": 38349
}
}
}
}
```

#### Payload da criação de um novo Software Authority Claim
```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"SoftwareStatements": {
"39c82433-0f57-4653-b513-01b03997ff0f": {
"SoftwareAuthorityClaims": [
{
"SoftwareStatementId": "39c82433-0f57-4653-b513-01b03997ff0f",
"SoftwareAuthorityClaimId": "d297237e-8f5a-46f5-b6d2-e74ec9eaaea0",
"Status": "Active",
"AuthorisationDomain": "Open Banking Brasil ",
"Role": "DADOS",
"Revision": 38344
}
]
}
}
}

```
### Exemplo Payload Alteração Certificados

#### Payload da criação de um novo certificado BRCAC

```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "SoftwareStatements": {
    "7218e1af-195f-42b5-a44b-8c7828470f5a": {
      "SoftwareCertificates": [
        {
          "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
          "SoftwareStatementIds": [
            "7218e1af-195f-42b5-a44b-8c7828470f5a"
          ],
          "Status": "Active",
          "ValidFromDateTime": "Wed Feb 16 19:35:00 UTC 2022",
          "ExpiryDateTime": "Sat Mar 18 19:35:00 UTC 2023",
          "e": "AQAB",
          "keyType": "brcac",
          "kid": "SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM",
          "kty": "RSA",
          "n": "pj6eoKgxitjvmP8IOycMByNQkW5uvv4T9trRHONSNlxvVYE-s1evJfOdpiBlFa1HVWJzJReIjrUgwOR4R89OAxVnC55N-4Ztzd24Y2qLMOoExj9YeWrupZzq1lpWew0rAHdnrYia6JCT2OrUR2nGrpSk8OWA7KLuHg5Vrs_1RuYHE51ks7mS2AHpmr6PTmDWrvo-4bxI-RFifVaOJ5JlV4GZ7xIuj40ZMFD2Zsn1vH8CiqtOh25yaMp3pnuCUg0-YXIdlqDslG-oQ7YAVctIOzsXwraHsU70VKiyvwv3F8KtGHTigCVoEO8er4U-NdipP275rehfjXXaGnEPyTYe0w",
          "use": "enc",
          "x5c": [
            "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"
          ],
          "x5thashS256": "SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM",
          "x5u": "https://keystore.sandbox.directory.openbankingbrasil.org.br/74e929d9-33b6-4d85-8ba7-c146c867a817/7218e1af-195f-42b5-a44b-8c7828470f5a/SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM.pem",
          "Revision": 38443
        }
      ]
    }
  }
}
```

#### Payload da criação de um novo certificado BRSEAL

```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"OrganisationCertificates": [
{
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
"SoftwareStatementIds": [
"12e2b091-fab9-4c0f-b0e2-9d7c9b33a443",
"71670a98-143c-4fd6-877c-afce013a2cf4",
"e5de3c6c-9cd0-4f13-a836-0e8ff19aa3db",
"e88963b5-ab80-437c-9911-b99780d0f68f",
"1509a662-6b3a-4cb8-b7c0-ffb6e596eb0d",
"79adabac-9443-4910-9b2e-f8cc574d57fc",
"011148c8-72f6-4491-81a5-954cab757b2f",
"b454c0cb-3210-4e8b-bbc2-a22da1679e63",
"bec76078-afa6-4fee-88c3-44789ffb817c",
"9ae30866-215d-4218-9014-470319077329",
"df4be229-97c5-415a-b315-d66abb7baf14",
"8819bd01-a4a8-4847-83f0-35b38d9ef22a",
"20597fcb-a1f4-4dbb-90ed-9531052e4b93",
"278bbdde-eeb3-42e4-995c-b265bc0a3014",
"951c0aaf-3ad9-4913-bab7-5dd0116de8a4",
"15c17d2f-7633-46d7-b37b-eb8a79bfa454",
"d7499c82-8294-495b-9cc5-5d79b8b093b7",
"2a8f9513-6c0c-42ee-bf36-fc45a404432e",
"6a67ffd7-9627-490f-9312-9f3591196831",
"ac6ab37f-bfd6-422e-9382-fc617bd434d4",
"86888b9a-1d1c-40bf-92d1-96b9521d850f",
"185636b6-2713-4658-ba14-1f7e924007c8",
"7c66977b-c785-44ee-b0c6-b75c9c1c6126",
"9e357995-a191-4860-9db7-ceda3692a8c9",
"10120340-3318-4baf-99e2-0b56729c4ab2",
"41ee547e-d22a-452f-86e3-bdbecff57e69",
"92157cdc-8cea-4e29-b9dc-e370ccc027c9",
"368bceef-6096-4e1a-a702-64c07d12c98a",
"67ffa051-a6fa-4732-af95-eb32e70ae5bb",
"bf4fe585-a56f-4c68-82fe-eb1265261727",
"1a984f05-948e-48c1-b829-8147877059c7",
"7218e1af-195f-42b5-a44b-8c7828470f5a",
"39c82433-0f57-4653-b513-01b03997ff0f",
"782ab284-87ed-4080-bc3c-e22126cd83a5",
"bb18cc84-4c5a-403f-9b31-beef97f22aa4",
"e86671e8-ee21-4b09-93b2-293e4324ac70",
"b611a7a2-366e-4146-920a-360fcfbc5ab7"
],
"Status": "Active",
"ValidFromDateTime": "Wed Feb 16 01:26:00 UTC 2022",
"ExpiryDateTime": "Sat Mar 18 01:26:00 UTC 2023",
"e": "AQAB",
"keyType": "brseal",
"kid": "zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8",
"kty": "RSA",
"n": "10Bi8lZeXcStcgrVZIBX9FFw2oPJyEWwsgrYJcQXs4qiWFyYN-pjIYwzYRIkgmc9QNv3DZVuc8t-ThTxpu53xI43PwZQw2k4azwLQtFm_OSWwiJ_PGgFy65AWwwF0bbnseyOZ4zufDCkwcUF3SO7h7aUbcRSD0SQ7cvqi7yjF9bmXTy37kzhRkQYxz9KPEmCCkTUt__gZ-WhAIua_qEgTNlxK3k7qXDZXJaMjTpDMOqM0yviYzLX10npFeAzanJPBkZx3vmjItO-6SdNGRVBsjYZBv9Qg1W7_mW34rmy1viM029wc8kdbg6AjWb25k4tzWW68KEctS33RR9TPczcCQ",
"use": "sig",
"x5c": [
"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"
],
"x5thashS256": "zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8",
"x5u": "https://keystore.sandbox.directory.openbankingbrasil.org.br/74e929d9-33b6-4d85-8ba7-c146c867a817/zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8.pem",
"Revision": 38346
}
]
}

```


#### Payload da inativação de um certificado BRCAC 

```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "SoftwareStatements": {
    "7218e1af-195f-42b5-a44b-8c7828470f5a": {
      "SoftwareCertificates": [
        {
          "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
          "SoftwareStatementIds": [
            "7218e1af-195f-42b5-a44b-8c7828470f5a"
          ],
          "Status": "Inactive",
          "ValidFromDateTime": "2022-02-16 19:35:00.0",
          "ExpiryDateTime": "2023-03-18 19:35:00.0",
          "e": "AQAB",
          "keyType": "brcac",
          "kid": "SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM",
          "kty": "RSA",
          "n": "pj6eoKgxitjvmP8IOycMByNQkW5uvv4T9trRHONSNlxvVYE-s1evJfOdpiBlFa1HVWJzJReIjrUgwOR4R89OAxVnC55N-4Ztzd24Y2qLMOoExj9YeWrupZzq1lpWew0rAHdnrYia6JCT2OrUR2nGrpSk8OWA7KLuHg5Vrs_1RuYHE51ks7mS2AHpmr6PTmDWrvo-4bxI-RFifVaOJ5JlV4GZ7xIuj40ZMFD2Zsn1vH8CiqtOh25yaMp3pnuCUg0-YXIdlqDslG-oQ7YAVctIOzsXwraHsU70VKiyvwv3F8KtGHTigCVoEO8er4U-NdipP275rehfjXXaGnEPyTYe0w",
          "use": "enc",
          "x5c": [
            "MIIG5zCCBc+gAwIBAgIUCsl1Ht855SQatiV/U4N+sc/pVNkwDQYJKoZIhvcNAQELBQAwcTELMAkGA1UEBhMCQlIxHDAaBgNVBAoTE09wZW4gQmFua2luZyBCcmFzaWwxFTATBgNVBAsTDE9wZW4gQmFua2luZzEtMCsGA1UEAxMkT3BlbiBCYW5raW5nIFNBTkRCT1ggSXNzdWluZyBDQSAtIEcxMB4XDTIyMDIxNjE5MzUwMFoXDTIzMDMxODE5MzUwMFowggETMQswCQYDVQQGEwJCUjELMAkGA1UECBMCUlMxDzANBgNVBAcTBkxvbmRvbjEcMBoGA1UEChMTT3BlbiBCYW5raW5nIEJyYXNpbDEtMCsGA1UECxMkNzRlOTI5ZDktMzNiNi00ZDg1LThiYTctYzE0NmM4NjdhODE3MRYwFAYDVQQDEw10cHAubG9jYWxob3N0MRcwFQYDVQQFEw40MzE0MjY2NjAwMDE5NzEdMBsGA1UEDxMUUHJpdmF0ZSBPcmdhbml6YXRpb24xEzARBgsrBgEEAYI3PAIBAxMCVUsxNDAyBgoJkiaJk/IsZAEBEyQ3MjE4ZTFhZi0xOTVmLTQyYjUtYTQ0Yi04Yzc4Mjg0NzBmNWEwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCmPp6gqDGK2O+Y/wg7JwwHI1CRbm6+/hP22tEc41I2XG9VgT6zV68l852mIGUVrUdVYnMlF4iOtSDA5HhHz04DFWcLnk37hm3N3bhjaosw6gTGP1h5au6lnOrWWlZ7DSsAd2etiJrokJPY6tRHacaulKTw5YDsou4eDlWuz/VG5gcTnWSzuZLYAemavo9OYNau+j7hvEj5EWJ9Vo4nkmVXgZnvEi6PjRkwUPZmyfW8fwKKq06HbnJoyneme4JSDT5hch2WoOyUb6hDtgBVy0g7OxfCtoexTvRUqLK/C/cXwq0YdOKAJWgQ7x6vhT412Kk/bvmt6F+NddoacQ/JNh7TAgMBAAGjggLRMIICzTAMBgNVHRMBAf8EAjAAMB0GA1UdDgQWBBSCP1MPvu9Lz3s9L7hJkFXNy407lDAfBgNVHSMEGDAWgBSGf1itF/WCtk60BbP7sM4RQ99MvjBMBggrBgEFBQcBAQRAMD4wPAYIKwYBBQUHMAGGMGh0dHA6Ly9vY3NwLnNhbmRib3gucGtpLm9wZW5iYW5raW5nYnJhc2lsLm9yZy5icjBLBgNVHR8ERDBCMECgPqA8hjpodHRwOi8vY3JsLnNhbmRib3gucGtpLm9wZW5iYW5raW5nYnJhc2lsLm9yZy5ici9pc3N1ZXIuY3JsMBgGA1UdEQQRMA+CDXRwcC5sb2NhbGhvc3QwDgYDVR0PAQH/BAQDAgWgMBMGA1UdJQQMMAoGCCsGAQUFBwMCMIIBoQYDVR0gBIIBmDCCAZQwggGQBgorBgEEAYO6L2QBMIIBgDCCATYGCCsGAQUFBwICMIIBKAyCASRUaGlzIENlcnRpZmljYXRlIGlzIHNvbGVseSBmb3IgdXNlIHdpdGggUmFpZGlhbSBTZXJ2aWNlcyBMaW1pdGVkIGFuZCBvdGhlciBwYXJ0aWNpcGF0aW5nIG9yZ2FuaXNhdGlvbnMgdXNpbmcgUmFpZGlhbSBTZXJ2aWNlcyBMaW1pdGVkcyBUcnVzdCBGcmFtZXdvcmsgU2VydmljZXMuIEl0cyByZWNlaXB0LCBwb3NzZXNzaW9uIG9yIHVzZSBjb25zdGl0dXRlcyBhY2NlcHRhbmNlIG9mIHRoZSBSYWlkaWFtIFNlcnZpY2VzIEx0ZCBDZXJ0aWNpY2F0ZSBQb2xpY3kgYW5kIHJlbGF0ZWQgZG9jdW1lbnRzIHRoZXJlaW4uMEQGCCsGAQUFBwIBFjhodHRwOi8vY3BzLnNhbmRib3gucGtpLm9wZW5iYW5raW5nYnJhc2lsLm9yZy5ici9wb2xpY2llczANBgkqhkiG9w0BAQsFAAOCAQEAUo6cCxPhK63hKTk9F4PfG7T0xr0gxqAv729nRUEonVfQv6m7Dt8zMf0Nmfe2BB9Oozmwa3Tq8m5Zz05d75hVIt0SHx9IcU9xLCHSrZy7qfbz0f/hVGYAWAwGS6QnYnbnmnZOkjNQ6WpbMatHVDDD0IglbtR9xYSTxD6HInFOTAbBFyE40F7u828vh8aQO20KlRvdgB/uNPfg2+7CIC7mIzsF7JwQcEQtwN2MqmNnoZtHex02oVoasYwSqN6ZAUPBJ+TCT9OPAjrKc//x6i84VmEHLmqlFXOIPXlS6PVyfs877AbG+nqod2qWV2z826LputSHp4O2S5KOjZHTSG5iSQ=="
          ],
          "x5thashS256": "SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM",
          "x5u": "https://keystore.sandbox.directory.openbankingbrasil.org.br/74e929d9-33b6-4d85-8ba7-c146c867a817/7218e1af-195f-42b5-a44b-8c7828470f5a/SWSVZ7cNWFpyBx9bDpiExC3OUH2UsV6fXnaguDlo4JM.pem",
          "Revision": 38444
        }
      ]
    }
  }
}

```
#### Payload da inativação de um certificado BRSEAL

```
{
  "OrganisationDetails": {
    "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
  },
  "OrganisationCertificates": [
    {
      "OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
      "SoftwareStatementIds": [
        "12e2b091-fab9-4c0f-b0e2-9d7c9b33a443",
        "71670a98-143c-4fd6-877c-afce013a2cf4",
        "e5de3c6c-9cd0-4f13-a836-0e8ff19aa3db",
        "e88963b5-ab80-437c-9911-b99780d0f68f",
        "1509a662-6b3a-4cb8-b7c0-ffb6e596eb0d",
        "79adabac-9443-4910-9b2e-f8cc574d57fc",
        "011148c8-72f6-4491-81a5-954cab757b2f",
        "b454c0cb-3210-4e8b-bbc2-a22da1679e63",
        "bec76078-afa6-4fee-88c3-44789ffb817c",
        "9ae30866-215d-4218-9014-470319077329",
        "df4be229-97c5-415a-b315-d66abb7baf14",
        "8819bd01-a4a8-4847-83f0-35b38d9ef22a",
        "20597fcb-a1f4-4dbb-90ed-9531052e4b93",
        "278bbdde-eeb3-42e4-995c-b265bc0a3014",
        "951c0aaf-3ad9-4913-bab7-5dd0116de8a4",
        "15c17d2f-7633-46d7-b37b-eb8a79bfa454",
        "d7499c82-8294-495b-9cc5-5d79b8b093b7",
        "2a8f9513-6c0c-42ee-bf36-fc45a404432e",
        "6a67ffd7-9627-490f-9312-9f3591196831",
        "ac6ab37f-bfd6-422e-9382-fc617bd434d4",
        "86888b9a-1d1c-40bf-92d1-96b9521d850f",
        "185636b6-2713-4658-ba14-1f7e924007c8",
        "7c66977b-c785-44ee-b0c6-b75c9c1c6126",
        "9e357995-a191-4860-9db7-ceda3692a8c9",
        "10120340-3318-4baf-99e2-0b56729c4ab2",
        "41ee547e-d22a-452f-86e3-bdbecff57e69",
        "92157cdc-8cea-4e29-b9dc-e370ccc027c9",
        "368bceef-6096-4e1a-a702-64c07d12c98a",
        "67ffa051-a6fa-4732-af95-eb32e70ae5bb",
        "bf4fe585-a56f-4c68-82fe-eb1265261727",
        "1a984f05-948e-48c1-b829-8147877059c7",
        "7218e1af-195f-42b5-a44b-8c7828470f5a",
        "39c82433-0f57-4653-b513-01b03997ff0f",
        "782ab284-87ed-4080-bc3c-e22126cd83a5",
        "bb18cc84-4c5a-403f-9b31-beef97f22aa4",
        "b611a7a2-366e-4146-920a-360fcfbc5ab7"
      ],
      "Status": "Inactive",
      "ValidFromDateTime": "2022-02-16 01:26:00.0",
      "ExpiryDateTime": "2023-03-18 01:26:00.0",
      "e": "AQAB",
      "keyType": "brseal",
      "kid": "zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8",
      "kty": "RSA",
      "n": "10Bi8lZeXcStcgrVZIBX9FFw2oPJyEWwsgrYJcQXs4qiWFyYN-pjIYwzYRIkgmc9QNv3DZVuc8t-ThTxpu53xI43PwZQw2k4azwLQtFm_OSWwiJ_PGgFy65AWwwF0bbnseyOZ4zufDCkwcUF3SO7h7aUbcRSD0SQ7cvqi7yjF9bmXTy37kzhRkQYxz9KPEmCCkTUt__gZ-WhAIua_qEgTNlxK3k7qXDZXJaMjTpDMOqM0yviYzLX10npFeAzanJPBkZx3vmjItO-6SdNGRVBsjYZBv9Qg1W7_mW34rmy1viM029wc8kdbg6AjWb25k4tzWW68KEctS33RR9TPczcCQ",
      "use": "sig",
      "x5c": [
        "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"
      ],
      "x5thashS256": "zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8",
      "x5u": "https://keystore.sandbox.directory.openbankingbrasil.org.br/74e929d9-33b6-4d85-8ba7-c146c867a817/zWF54vNJxQLnG5AU12ZZ_1e0ICoB_Zrj2cq4XrJiQE8.pem",
      "Revision": 38442
    }
  ]
}
```


### Exemplo Payload Alteração Contatos

#### Payload da adição de um contato

```
{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"Contacts": [
{
"ContactId": "5e3c5aed-0aab-43fb-bae9-28209d5c5b53",
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817",
"ContactType": "Technical",
"EmailAddress": "bernardo.campos@raidiam.com",
"PhoneNumber": "21997208035",
"Revision": 38348
}
]
}

```
#### Payload de delete de um contato

```

{
"OrganisationDetails": {
"OrganisationId": "74e929d9-33b6-4d85-8ba7-c146c867a817"
},
"Contacts": [
{
"ContactId": "ece9ceb4-6a2d-44a9-b66c-215de37181af",
"ContactType": "Business",
"Revision": 38347
}
]
}


```






