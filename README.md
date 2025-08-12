# Gerador Automático de Vídeos de Santos Católicos

Workflow automatizado no n8n que gera vídeos sobre Santos Católicos usando conteúdo do Google Drive, text-to-speech e renderização de vídeo.

## Visão Geral

Este workflow processa automaticamente conteúdo de Santos Católicos armazenado no Google Drive para criar vídeos educacionais. Executa diariamente às 6:00, processando pastas de santos contendo ensinamentos e imagens para gerar vídeos narrados.

<img width="1336" height="169" alt="{EDE4432E-AC9A-4DF1-BB50-9F57DA92B716}" src="https://github.com/user-attachments/assets/6eb5b4c5-153d-4b3e-97c7-901238d45b57" />

## Funcionalidades

- **Agendamento Automático**: Execução diária às 6:00
- **Integração Google Drive**: Lê pastas e conteúdo dos santos
- **Text-to-Speech**: Converte ensinamentos para áudio em português brasileiro usando Google TTS
- **Armazenamento em Nuvem**: Armazena assets em buckets do Google Cloud Storage
- **Geração de Vídeo**: Cria vídeos usando a API do Creatomate
- **Processamento em Lote**: Processa múltiplos santos em uma única execução

## Estrutura do Workflow

### Fontes de Dados
- **Google Drive**: Pasta principal "Santos Católicos" contendo subpastas de santos
- **Formato do Conteúdo**: Cada pasta de santo contém:
  - `ensinamentos.txt`: Texto dos ensinamentos do santo
  - `imagens/`: Pasta com imagens do santo

### Pipeline de Processamento
1. **Descoberta de Conteúdo**: Busca pastas de santos no Google Drive
2. **Processamento de Texto**: Download e limpeza dos textos de ensinamentos
3. **Geração de Áudio**: Converte texto em fala (pt-BR, voz masculina)
4. **Processamento de Imagens**: Download das imagens dos santos
5. **Upload de Assets**: Armazena áudio e imagens no Google Cloud Storage
6. **Criação de Vídeo**: Renderiza vídeo final usando template do Creatomate

## Requisitos

**Plataformas e Serviços:**
- n8n (plataforma de automação de workflow)
- Conta Google (para acesso às APIs)
- Google Cloud Platform (TTS e Cloud Storage)
- Creatomate (renderização de vídeos)

**APIs Necessárias:**
- Google Drive API
- Google Cloud Storage API  
- Google Text-to-Speech API
- Creatomate API

**Credenciais Requeridas:**
- Google Drive OAuth2
- Google Cloud Storage OAuth2
- Google OAuth2 para TTS
- Creatomate Bearer Token

## Configuração

### 1. Estrutura do Google Drive
```
Santos Católicos/
├── São Francisco/
│   ├── ensinamentos.txt
│   └── imagens/
│       └── imagem1.jpg
├── Santa Teresa/
│   ├── ensinamentos.txt
│   └── imagens/
│       └── imagem1.jpg
└── ...
```

### 2. Google Cloud Storage
- Bucket: `santos_catolicos`
- Arquivos de áudio: `audio_{nome_santo}.mp3`
- Arquivos de imagem: `imagem_{nome_santo}.jpg`

### 3. Template Creatomate
- Template ID: `644e643c-cc26-43db-8513-42546ad558a7`
- Elementos modificáveis:
  - `Text-1`: Nome do santo
  - `Image-1`: URL da imagem
  - `Audio-1`: URL do áudio

## Como Usar

1. **Preparação**: Organize o conteúdo dos santos no Google Drive conforme a estrutura especificada
2. **Configuração**: Configure todas as credenciais necessárias no n8n
3. **Ativação**: Ative o workflow para execução automática diária
4. **Monitoramento**: Acompanhe os logs de execução para verificar o processamento

## Estrutura dos Nós

- **Agendador**: Trigger diário às 6:00
- **Busca de Conteúdo**: Localiza pastas e arquivos dos santos
- **Processamento de Texto**: Limpa e prepara texto para TTS
- **Geração de Áudio**: Cria narração usando Google TTS
- **Upload de Assets**: Armazena arquivos no Google Cloud Storage
- **Renderização**: Gera vídeo final via Creatomate

## Saída

- **Vídeos**: Renderizados via Creatomate com narração e imagens dos santos
- **Duração**: 30 segundos por vídeo
- **Formato**: Definido pelo template do Creatomate
