# GitLab CE Self-Hosted com Traefik, Let's Encrypt e Cloudflare

Projeto para implantação de uma plataforma **GitLab CE Self-Hosted** utilizando **Docker**, **Traefik** como Reverse Proxy e **Let's Encrypt** para emissão automática de certificados SSL através do **DNS Challenge da Cloudflare**.

Toda a solução foi desenvolvida utilizando ferramentas gratuitas e de código aberto, podendo ser reproduzida em provedores como Oracle Cloud, AWS, Azure, Google Cloud ou qualquer VPS Linux.

---

# Arquitetura

```text
                    Internet
                         │
                         │
                  gitlab.seudominio.com
                         │
                         ▼
                +-------------------+
                |    Cloudflare     |
                |    (DNS Only)     |
                +-------------------+
                         │
                         ▼
                +-------------------+
                |      Traefik      |
                | Reverse Proxy     |
                | HTTPS / SSL       |
                +-------------------+
                         │
                         ▼
                +-------------------+
                |    GitLab CE      |
                |      Docker       |
                +-------------------+
                         │
                         ▼
                 Docker Volumes
              (Config / Logs / Data)
```

---

# Tecnologias utilizadas

- Ubuntu Server 24.04 LTS
- Oracle Cloud Free Tier
- Docker Engine
- Docker Compose
- GitLab CE
- Traefik
- Let's Encrypt
- Cloudflare DNS Challenge
- Docker Networks
- Docker Volumes

---

# Objetivo

Disponibilizar uma instalação moderna e segura do GitLab CE utilizando:

- SSL automático
- Renovação automática dos certificados
- Reverse Proxy dedicado
- Containers desacoplados
- Persistência de dados
- Arquitetura modular
- Fácil manutenção

---

# Estrutura do projeto

```
gitlab-self-hosted-platform/
│
├── gitlab/
│   └── docker-compose.yml
│
├── infra/
│   ├── docker-compose.yml
│   ├── traefik.yml
│   ├── .env.example
│   └── letsencrypt/
│
├── docs/
│
└── README.md
```

---

# Pré-requisitos

Antes da instalação é necessário possuir:

## 1. Oracle Cloud (ou outro provedor)

Criar uma VM Linux.

Recomendado:

- Ubuntu Server 24.04 LTS
- ARM64 ou AMD64
- 4 vCPU
- 24 GB RAM (ideal para GitLab)
- IP Público

---

## 2. Domínio

Possuir um domínio registrado.

Exemplo:

```
seudominio.com
```

---

## 3. Cloudflare

Criar uma conta gratuita.

Adicionar seu domínio.

Criar um registro DNS:

```
gitlab.seudominio.com
```

Modo:

```
DNS Only
```

(Nuvem cinza)

---

## 4. Criar API Token da Cloudflare

Criar um API Token com permissões:

- Zone → DNS → Edit
- Zone → Zone → Read

Salvar o token para utilização pelo Traefik.

---

# Instalar Docker

Instale:

- Docker Engine
- Docker Compose Plugin

Valide:

```bash
docker version
docker compose version
```

---

# Clonar o projeto

```bash
git clone https://github.com/SEU_USUARIO/gitlab-self-hosted-platform.git

cd gitlab-self-hosted-platform
```

---

# Configurar variáveis

Criar:

```
infra/.env
```

Baseando-se no arquivo:

```
infra/.env.example
```

Exemplo:

```env
CLOUDFLARE_DNS_API_TOKEN=SEU_TOKEN
LETSENCRYPT_EMAIL=voce@email.com
```

---

# Criar rede Docker

```bash
docker network create traefik-public
```

---

# Subir infraestrutura

Entrar na pasta:

```
infra/
```

Executar:

```bash
docker compose up -d
```

Isso irá subir:

- Traefik
- HTTPS
- Integração com Let's Encrypt
- Renovação automática SSL

---

# Subir GitLab

Entrar em:

```
gitlab/
```

Executar:

```bash
docker compose up -d
```

---

# HTTPS

O certificado é emitido automaticamente utilizando:

- Let's Encrypt
- DNS Challenge
- Cloudflare API

Não é necessário emitir certificados manualmente.

A renovação também ocorre automaticamente.

---

# Persistência

O projeto utiliza Docker Volumes para persistência:

- Configurações
- Banco interno
- Repositórios
- Logs

Mesmo recriando os containers, os dados permanecem preservados.

---

# Arquitetura modular

Este projeto separa responsabilidades:

## Infraestrutura

- Traefik
- SSL
- Reverse Proxy

## Aplicação

- GitLab CE

Essa abordagem facilita upgrades, manutenção e escalabilidade.

---

# Próximos passos

Após a instalação é possível explorar recursos como:

- GitLab CI/CD
- GitLab Runner
- Pipelines automatizados
- Deploy contínuo
- Container Registry
- Integração com Kubernetes
- Infrastructure as Code

---

# Resultado esperado

Acesso pelo navegador:

```
https://gitlab.seudominio.com
```

Com:

- Cadeado HTTPS válido ✅
- Certificado Let's Encrypt ✅
- Renovação automática ✅

---

# Segurança

Nunca publique:

- `.env`
- `acme.json`
- Tokens da Cloudflare
- Chaves privadas SSH
- Senhas

Utilize sempre arquivos de exemplo (`.env.example`) para compartilhar configurações.

---

# Contribuições

Contribuições são bem-vindas!

Caso encontre melhorias ou queira evoluir esta arquitetura, fique à vontade para abrir uma Issue ou Pull Request.

---

# Licença

Este projeto é distribuído sob a licença MIT.

---

# ‍Autor

**Rosivaldo Nunes da Cunha**

Projeto desenvolvido com foco em estudos práticos de DevOps, Containers, Automação e CI/CD utilizando tecnologias amplamente adotadas no mercado.
