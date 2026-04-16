### Descomplicando o Docker Compose

O Docker Compose é uma ferramenta que permite definir e executar múltiplos containers Docker em um único arquivo YAML.

Comandos úteis:
- docker-compose up: Cria e inicia os containers.
- docker-compose down: Para e remove os containers.
- docker-compose ps: Lista os containers.
- docker-compose logs: Mostra os logs dos containers.
- docker-compose build: Constrói as imagens dos containers.
- docker-compose pull: Baixa as imagens dos containers.
- docker-compose push: Envia as imagens dos containers.
- docker-compose restart: Reinicia os containers.
- docker-compose stop: Para os containers.
- docker-compose start: Inicia os containers.
- docker-compose down --volumes: Para e remove os containers e os volumes.
- docker-compose down --rmi all: Para e remove os containers e as imagens.
- docker-compose down --volumes --rmi all: Para e remove os containers, os volumes e as imagens.

### Docker compose reservations

O `reservations` serve para definir limites de recursos (como CPU e memória) que devem ser garantidos para o funcionamento do container. Em versões mais recentes do spec do compose, ficam dentro de `deploy.resources`.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    deploy:
      resources:
        reservations:
          cpus: '0.5'
          memory: 512M 
```

### Docker compose depends_on

O `depends_on` serve para definir a ordem de inicialização dos containers. Ele garante que um container específico só será iniciado depois que outro serviço do qual ele dependa já tiver iniciado (ou estiver saudável, dependendo da configuração utilizada).

Exemplo de como usar no `docker-compose.yml`:
```yaml
    depends_on:
      - redis  
```

### Docker compose healthcheck

O `healthcheck` serve para configurar um script ou comando interno do container que verifica se a aplicação dentro dele está saudável e funcionando perfeitamente, garantindo seu monitoramento durante a execução.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    healthcheck:
      test: ["CMD", "redis-cli", "ping"] ## comando para verificar se o container está rodando
      interval: 10s ## tempo entre as verificações periódicas
      timeout: 5s ## tempo máximo para a execução da verificação
      retries: 5 ## número de falhas permitidas antes de marcar como "unhealthy"
      start_period: 10s ## tempo inicial esperado para o container iniciar antes de começar a checar
```

### Docker Contexts

O `context` (utilizado muitas vezes em `build`) serve para definir o diretório raiz das informações que serão enviadas para o Docker construir a imagem. Geralmente você passa um caminho relativo de pasta onde o seu `Dockerfile` também fica armazenado.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    build:
      context: ./pasta-do-microsservico 
```

### Docker env_file

O `env_file` serve para carregar múltiplas variáveis de ambiente a partir de um arquivo externo (ex.: `.env`), evitando manter senhas e tokens diretamente no arquivo do `docker-compose.yml`.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    env_file:
      - .env
```

### Docker compose networks

O `networks` serve para conectar os containers através de redes virtuais virtuais isoladas internamente gerenciadas pelo Docker. Assim eles podem se comunicar entre si de maneira segura ou serem separados para isolamento.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    networks:
      - nome_da_rede
```

### Docker compose volumes

O `volumes` serve para persistir os dados armazenados nos containers em pastas na sua máquina (host), ou simplesmente compartilhar arquivos de configurações. Mesmo que o container seja apagado, os dados persistirão pelo volume.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    volumes:
      - nome_do_volume_ou_caminho:/caminho/no/container
```

### Docker compose restart

O `restart` serve para configurar a política de reinício automático do container. Caso seja sinalizada uma quebra ou falha inesperada, pode ser configurado para ser subir de volta (ex.: `always`, `on-failure` ou `unless-stopped`).

Exemplo de como usar no `docker-compose.yml`:
```yaml
    restart: always
```

### Docker compose deploy

O `deploy` serve para configurar opções avançadas de orquestração do container, definindo estratégias para publicação em massa (como com o Docker Swarm), controlando a quantidade de instâncias (*replicas*), limites de processamento, e atrasos nas atualizações.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    deploy:
      replicas: 3
      update_config:
        parallelism: 2
        delay: 10s
      rollback_config:
        parallelism: 2
        delay: 10s
      placement:
        constraints:
          - node.role == manager
      resources:
        limits:
          cpus: '0.5'
          memory: 512M 
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
```

### Docker compose build

O `build` serve para explicitar que a imagem deste container precisa ser montada (construída) usando definições locais e um Dockerfile, definindo o `context` e/ou o nome do arquivo que será buildado para o ambiente correspondente. 

Exemplo de como usar no `docker-compose.yml`:
```yaml
    build:
      context: .
      dockerfile: Dockerfile
```

### Docker compose command

O `command` serve para sobrescrever dinamicamente o comando padrão de execução que vem programado no sistema da imagem (o comando `CMD` interno do seu Dockerfile ou imagem pai).

Exemplo de como usar no `docker-compose.yml`:
```yaml
    command: ["echo", "Hello World"]
```

### Docker compose entrypoint

O `entrypoint` serve para forçar ou sobreescrever inteiramente o ponto de entrada principal rígido da imagem (o `ENTRYPOINT`). Diferente do command, o entrypoint define a fundação irredutível do script a ser inciado.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    entrypoint: ["echo", "Hello World"]
```

### Docker compose ports

O `ports` serve para abrir acessos à portas locais da sua máquina fazendo redirecionamento/mapeamento para as portas rodando internamente no serviço do container. Ele permite você se comunicar de fora com o sistema dentro.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    ports:
      - "5000:5000" ## "porta-no-host:porta-no-container"
```

### Docker compose expose

O `expose` serve puramente para documentar e abrir portas internas. Diferente do `ports`, as portas listadas em `expose` só ficam disponíveis para os outros containers da *mesma rede*, e isoladas da máquina host. 

Exemplo de como usar no `docker-compose.yml`:
```yaml
    expose:
      - "5000"
```

### Docker compose labels

O `labels` serve para pendurar metadados textuais na configuração e identificação de serviços nos seus containers. Muitas ferramentas modernas de infraestrutura como o Traefik leem essas labels para configurarem rotas e relatórios.

Exemplo de como usar no `docker-compose.yml`:
```yaml
    labels:
      - "com.docker.compose.project=nome_do_projeto"
      - "com.docker.compose.service=nome_do_serviço"
      - "com.docker.compose.version=versão"