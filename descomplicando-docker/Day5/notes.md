Descomplicando Docker - Dia 5

Descomplicando Network no Docker

Tipos de Network:
- Bridge: Rede padrão do Docker, isolada por padrão, mas pode ser acessada externamente através de mapeamento de portas.
- Host: O container compartilha a rede do host, sem isolamento.
- Overlay: Rede distribuída entre múltiplos hosts, usada para comunicação entre containers em diferentes hosts.
- Macvlan: Permite que os containers tenham seu próprio endereço MAC, fazendo com que pareçam dispositivos físicos na rede.
- None: O container não tem rede.

Comandos úteis:
- docker network ls: Lista todas as redes.
- docker network inspect <network_name>: Inspeciona uma rede.
- docker network create <network_name>: Cria uma rede.
- docker network rm <network_name>: Remove uma rede.
- docker network connect <network_name> <container_name>: Conecta um container a uma rede.
- docker network disconnect <network_name> <container_name>: Desconecta um container de uma rede.
- docker network prune: Remove todas as redes que não estão conectadas a pelo menos um container (limpeza).

### Detalhes Importantes:
- **Resolução de Nomes (DNS Interno)**: Na rede `bridge` **padrão**, os containers não conseguem se comunicar usando seus nomes (hostnames). Para que a resolução de nomes funcione automaticamente, é necessário criar uma **rede customizada** (*user-defined bridge*) e conectar os containers nela.
- **Múltiplas Redes**: Um único container pode estar conectado a várias redes simultaneamente, sendo muito útil para isolar tráfego (ex: um container web na rede frontend e na rede backend, mas o banco de dados apenas na backend).

### Opções Avançadas de Criação:
- `docker network create -d <driver> <network_name>`: Especifica o tipo de rede. O padrão é `bridge`. Exemplo: `-d macvlan`.
- `docker network create --subnet 192.168.1.0/24 --gateway 192.168.1.1 <network_name>`: Cria uma rede definindo explicitamente o bloco de IP e o gateway.

### Expondo Portas (Port Binding):
- Para permitir o acesso externo aos containers na rede bridge, usamos o *port binding* na criação do container (`docker run`).
- Flag `-p <porta_host>:<porta_container>` (ex: `-p 8080:80` mapeia a porta 8080 do host para a 80 do container).
- Flag `-P` (maiúsculo): Publica todas as portas expostas do container em portas aleatórias do host.

### Limitando recursos de CPU e memória:
- `--cpus <numero>`: Limita a quantidade de CPU que o container pode usar. Ex: `--cpus 0.5` limita a 50% de um núcleo.
- `--memory <tamanho>`: Limita a quantidade de memória que o container pode usar. Ex: `--memory 512m` limita a 512MB de memória.
- `--memory-swap <tamanho>`: Limita a quantidade de memória que o container pode usar, incluindo swap. Ex: `--memory-swap 1g` limita a 1GB de memória, incluindo swap.