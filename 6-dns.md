# DNS - Domain Name System

- Serviço que converte nome de domínios para endereços de IP roteáveis
- Possível com IPv4 (Internet Protocol Version 4) - 32 bits endereços de espaço
- Possível com IPv6 - 128 bits endereços de espaço

## Domain Registrars

- Third party company que registra os domínios

## Top-level Domains

- É a última palavra do dominio -> example.**com**
- São controlados pela *Internet Assigned Numbers Authority (IANA)*

## Second-level Domain

- Segunda palavra no dominio -> example.**co**.uk

## Start of Authority (SOA)

- É como se fosse metadados de um domínio
- Disponibiliza informações sobre o dominio
- Nome, zona, name, expire time, TTL etc

## Address Records - A Record

- O cara que converte o nome de um dominio para um endereço IP

```json
{
    "ResourceRecordSets": [
        "TTL": 300,
        "Type": "A",
        "Name": "testting-domain.com",
        "ResourceRecords": [
            "Value": "52.216.8.34"
        ]
    ]
}
```

## CNAME Records - Canonical Names

- Funciona como um alias de dominio
- Redireciona de um dominio para outro

```json
{
    "ResourceRecordSets": [
        "TTL": 300,
        "Type": "CNAME",
        "Name": "testting-domain.com",
        "ResourceRecords": [
            "Value": "www.testting-domain.com"
        ]
    ]
}
```

## Name Server (NS) Records

- São usados pelo top-level domain servers
- Para direcionar o tráfego para os servidores DNS contendo DNS records
- Normalmente são disponibilizados multiplos name servers para redundancia
- Se gerenciando DNS records com Route53 então é apontado para os servidores da Amazon

## TTL - Time to Live

- Tempo que o DNS record fica cacheado
- Sempre definido em segundos
- Quanto menor o TTL mais rápido se propaga pela internet
