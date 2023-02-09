# Teste Backend Java

Seu desafio será desenvolver um serviço de denúncias e, como parte dele, veremos como você estrutura as camadas de aplicação, chamadas externas, variáveis de ambiente, cache, testes unitários, logs e documentação.

### Proposta

Implementar uma API REST `[POST] /v1/denuncias` para inserir novas denúncias.

- O serviço de denúncias deverá persistir no banco de dados todos os atributos recebidos no request, juntamente com os dados de endereço originários de uma api externa;
- O serviço de denúncia receberá a geolocalizacão a partir do request e deverá buscar os dados de endereço no seguinte serviço [https://developer.mapquest.com/documentation/geocoding-api](https://developer.mapquest.com/documentation/geocoding-api);
- Para saber como utilizar o referido serviço de geolocalização verifique [https://developer.mapquest.com/documentation](https://developer.mapquest.com/documentation) e [https://developer.mapquest.com/documentation/geocoding-api](https://developer.mapquest.com/documentation/geocoding-api);
- A API REST deverá retornar um erro caso o endereço não seja encontrado no serviço de geolocalização;

Considere as informações abaixo para desenvolver o teste:

**Request**

```bash
curl -X POST \
  http://localhost:8080/v1/denuncias \
  -H 'Content-Type: application/json' \
  -d '{
    "latitude": -15.789925709252136,
    "longitude": -47.887251273393815,
    "denunciante": {
      "nome": "José da Silva",
      "cpf": "45616532145"
    },
    "denuncia": {
      "titulo": "Esgoto a céu aberto",
      "descricao": "Existe um esgoto a céu aberto nesta localidade."
    }
  }'
```

**Response**

```json
{
  "data": {
    "id": 1,
    "latitude": -15.789925709252136,
    "longitude": -47.887251273393815,
    "denunciante": {
      "nome": "José da Silva",
      "cpf": "45616532145"
    },
    "denuncia": {
      "titulo": "Esgoto a céu aberto",
      "descricao": "Existe um esgoto a céu aberto nesta localidade."
    },
    "endereco": {
      "logradouro": "SHN Quadra 2 Bloco F",
      "bairro": "Asa Norte",
      "cidade": "Brasília",
      "estado": "Distrito Federal",
      "pais": "BR",
      "cep": "70702-060"
    }
  }
}
```

**Error**

```json
{
  "error": {
    "message": "Endereço não encontrado para essa localidade.",
    "code": "02"
  }
}
```

### Tipos de Erros sugeridos

Estas são as sugestões de erros mapeados, porém fique livre para adicionar outros, caso identifique erros não mapeados.

| Code | Message                                       |
| ---- | --------------------------------------------- |
| 01   | Request inválido.                             |
| 02   | Endereço não encontrado para essa localidade. |

### Serviços dependentes

Este é um exemplo do retorno do request à API de geolocação. Estado, cidade e país são dados obrigatórios na composição do endereço. Os demais campos deverão ficar em branco caso não seja retornado dados para os mesmos.

**Response.body**

```json
{
  "info": {
    "statuscode": 0,
    "copyright": {
      "text": "© 2022 MapQuest, Inc.",
      "imageUrl": "http://api.mqcdn.com/res/mqlogo.gif",
      "imageAltText": "© 2022 MapQuest, Inc."
    },
    "messages": []
  },
  "options": {
    "maxResults": -1,
    "ignoreLatLngInput": false
  },
  "results": [
    {
      "providedLocation": {
        "location": "Brasilia"
      },
      "locations": [
        {
          "street": "",
          "adminArea6": "",
          "adminArea6Type": "Neighborhood",
          "adminArea5": "",
          "adminArea5Type": "City",
          "adminArea4": "",
          "adminArea4Type": "County",
          "adminArea3": "",
          "adminArea3Type": "State",
          "adminArea1": "BR",
          "adminArea1Type": "Country",
          "postalCode": "",
          "geocodeQualityCode": "A1XAX",
          "geocodeQuality": "COUNTRY",
          "dragPoint": false,
          "sideOfStreet": "N",
          "linkId": "0",
          "unknownInput": "",
          "type": "s",
          "latLng": {
            "lat": -15.7783,
            "lng": -47.9291
          },
          "displayLatLng": {
            "lat": -15.7783,
            "lng": -47.9291
          },
          "mapUrl": ""
        }
      ]
    }
  ]
}
```

### O que esperamos

- Que o desafio seja feito em Java;
- Que considere utilizar cache para consultas ao serviço de geolocalização;
- TDD;
- Princípios SOLID;
- [12Factor](https://12factor.net/);
- Passo-a-passo de como rodar sua aplicação;
- Conteinerização da aplicação utilizando docker e docker compose.

### Envio do teste

Ao finalizar, deverá ser enviado o link do repositório e do sistema em produção (se possível) [por este formulário](https://forms.office.com/Pages/ResponsePage.aspx?id=XDRlnVMnXE6Qf-NTDavkihJbY4jGBJBPlqTqU0MgZKJUODVWV0g3QURKUldBMlFPVzRSWVQ4UDk3TC4u&wdLOR=c73395CF2-2677-41A7-89DE-ABA221DC6366).

### Avaliação

Irá ser avaliado:

- Seu conhecimento técnico e criatividade;
- Sua habilidade para resolver problemas de forma simples e eficiente;
- Boas práticas de código.

---

Boa sorte!
