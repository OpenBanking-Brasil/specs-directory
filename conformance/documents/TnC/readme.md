#Documentação referente a certificação Funcional

Arquivos contidos na pasta

1 - Termos e condições referentes a certificação funcional do Open Banking
2 - Certificado de conformidade funcional
3 - Exemplo de preenchimento do certificado de conformidade funcional

A documentação contida nesta pasta é referente ao processo de requisição de conformidade funcional para as APIs do Open Banking Brasil.

A instituição que requisitar a certificação deverá enviar os dois documentos contidos nesta pasta junto com os resultados dos seus testes para o service desk do Open Banking, disponível em: https://servicedesk.openbankingbrasil.org.br.

O Mesmo documento de certificado de conformidade funcional pode ser submetido para múltiplas APIs, desde que sejam devidamentes explicitadas as API sendo certificada em "Nome e versão da API certificada (“Test Plan”):"  

As submissões dos arquivos .zip deverá ser realizadas de forma separada para cada APIs a ser certificada. Cada .zip deve conter o log referente a apenas um plano de testes. 

O Certificado de conformidade funcional deve ser assinado por um dos representates da instiuição ou por representante cujos poderes foram devidamente outorgados.

Caso a instiuição opte pela assinatura por um representante societários a consulta do signatário será realizada utilizando a seguinte API do Banco Central:
https://olinda.bcb.gov.br/olinda/service/Informes_ComposicaoGruposEstatutarios/version/v1/odata/cargosEstatutariosPorCNPJ

Um exemplo de chamado utilizando essa API para o Banco do Brasil, cujo codigo ISPB é 00000000, seria:
https://olinda.bcb.gov.br/olinda/service/Informes_ComposicaoGruposEstatutarios/version/v1/odata/cargosEstatutariosPorCNPJ(cnpj=@cnpj)?%40cnpj=%2700000000%27&%24format=text%2Fplain

Reforçamos que não é necessário o reconhecimento de firma para assinatura do certificado de conformidade funcional, podendo o mesmo ser assinado através de assinatura digital ou através da eventual digitalização do documento impresso assinado.
