# Route53

- Serviço de alta disponibilidade e escalavel para serviços de DNS
- Usado para **registrar dominios, roteamento DNS e health check**

## Routing Policies

### Simple Routing

- Pode mapear um domain name para um IP ou muitos
- Se especificar multiplos IPs o Route53 irá escolher um aleatoriamente

### Weighted Routing

- Faz o roteamendo por peso em multiplas regiões
- Ex 70% para us-east e 30% para sa-east
- Pode ser usado em teste-ab e blue-green deployment
- Precisa especificar um record para cada IP

### Latency-Based Routing

- Roteia para um região baseado na latência
- Precisa criar um latency resource record set na mesma região (ELB ou EC2) para receber o trafego
- Precisa especificar um record para cada IP

### Failover Routing

- Usado para rotear para outras instância quando a primária falha
- Usa um health check automatico
- Pode configurar um health check manual mais detalhado

### Geolocation Routing

- Permite escolher para onde o tráfego será enviado com base na localização geográfica dos usuários

### Geo-proximity Routing

- Permite escolher para onde o tráfego será enviado com base na localização geográfica de seus usuários e recursos
- Pode optar por rotear mais ou menos tráfego com base em um peso especificado, que é conhecido como **bias**
- Bias expande ou reduz a disponibilidade de uma região geográfica, o que facilita a transferência do tráfego de recursos em um local para recursos em outro
- É necessário habilitar o Route53 traffic flow
- Se deseja controlar o tráfego global, use o Geo-proximity Routing
- Se quiser que o tráfego permaneça em uma região local, use Geolocation Routing

### Multivalue Routing

- Praticamente o mesmo que o Simple Routing
- Mas permite que coloque health check em cada conjunto de registros
- Isso garante que apenas um IP íntegro seja retornado aleatoriamente, em vez de qualquer IP
