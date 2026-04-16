Comandos do dia 4:

Comando de Volume

```bash
# Criar volume
docker volume create nome_do_volume

# Listar volumes
docker volume ls

# Inspecionar volume
docker volume inspect nome_do_volume

# Remover volume
docker volume rm nome_do_volume
```

Comando de Bind Mount

```bash
# Criar bind mount
docker run -v /caminho/do/host:/caminho/do/container nome_da_imagem
```

Comando de tmpfs Mount

```bash
# Criar tmpfs mount
docker run -v /caminho/do/container nome_da_imagem
```

Comando de Volume Nomeado

```bash
# Criar volume nomeado
docker volume create nome_do_volume

# Iniciar container com volume nomeado
docker run -v nome_do_volume:/caminho/do/container nome_da_imagem
```

Comando de Volume Compartilhado

```bash
# Criar volume compartilhado
docker volume create nome_do_volume

# Iniciar containers com volume compartilhado
docker run -v nome_do_volume:/caminho/do/container1 nome_da_imagem1
docker run -v nome_do_volume:/caminho/do/container2 nome_da_imagem2
```

Comando de Volume Anônimo

```bash
# Criar volume anônimo
docker run -v /caminho/do/container nome_da_imagem
```

Comando de Volume Read-Only

```bash
# Criar volume read-only
docker run -v nome_do_volume:/caminho/do/container:ro nome_da_imagem
```

Comando de Volume Read-Write

```bash
# Criar volume read-write
docker run -v nome_do_volume:/caminho/do/container:rw nome_da_imagem
```
