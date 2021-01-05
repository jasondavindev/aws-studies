# Snowball

- Serviço físico de transferência de dados
- Maletinha que parece lançador de mísseis
- Transfere PetaByte de dados
- Transfere dados para AWS
- A transferencia de 100TB de dados custa 1/5
- A transferencia de 100TB leva menos 1 semana Vs 100 dias com outros players

### Features e limitações

- O dado é encriptado end-to-end (256-bit encryption)
- Usa Trusted Platform Module (TPM) um ship especializado armazena chaves RSA
- Importa e exporta dados para S3
- Para propositos de segurança, as transferencias devem ser completadas dentro de 90 dias

### Tipos
- 50 TB (42TB usável)
- 80 TB (72TB usável)

## Snowball Edge

- Similar ao Snowball mas com mais storage e processamento local

### Features e limitações

- Display LCD
- Pode realizar processamento local e
- Cargas de trabalho de computação de ponta
- Pode usar cluster de 5 a 10 dispositivos

### Opções

- Storage optimized (24 vCPUs)
- Compute optimized (54 vCPUs)
- GPU optimized (54 vCPUs)
- 100 TB (83TB usável)
- 100 TB clustered (45TB por node)

## Snowball Mobile

- Um caminhãozão bolado com um container
- Trasfere até 100PT de dados por Snowmobile
- Conecta a rede do cliente com o caminhão
- Depois da transferência o caminhão volta pra AWS e
- Importa os dados para S3 ou Glacier

### Observações

- Snowball e Edge são para Peta-scale migration
- Snowmobile é para Exabyte-scale migration