# Instruções de Implantação - Portal Retro Web

Este documento contém instruções detalhadas para implantar o Portal Retro Web em diferentes plataformas de hospedagem.

## Conteúdo do Pacote

O arquivo `retro-web-deploy.zip` contém todos os arquivos necessários para implantar a aplicação, exceto as dependências do Node.js (node_modules), que serão instaladas durante o processo de implantação.

## Requisitos Gerais

- Node.js 18.x ou superior
- NPM 8.x ou superior (ou PNPM 8.x)
- Git (opcional, mas recomendado para algumas plataformas)

## Opção 1: Implantação no Vercel (Recomendado)

O Vercel é a plataforma desenvolvida pelos criadores do Next.js e oferece a melhor experiência para hospedar aplicações Next.js.

### Passos para Implantação no Vercel:

1. Crie uma conta gratuita em [vercel.com](https://vercel.com)
2. Instale a CLI do Vercel:
   ```bash
   npm install -g vercel
   ```
3. Extraia o conteúdo do arquivo `retro-web-deploy.zip`
4. Navegue até a pasta extraída:
   ```bash
   cd retro-web-deploy
   ```
5. Instale as dependências:
   ```bash
   npm install
   ```
   ou
   ```bash
   pnpm install
   ```
6. Faça login na sua conta Vercel:
   ```bash
   vercel login
   ```
7. Implante a aplicação:
   ```bash
   vercel
   ```
8. Siga as instruções na tela para concluir a implantação.

Após a implantação, você receberá um URL no formato `retro-web.vercel.app` onde sua aplicação estará disponível.

## Opção 2: Implantação no Netlify

O Netlify é outra excelente plataforma para hospedar aplicações Next.js gratuitamente.

### Passos para Implantação no Netlify:

1. Crie uma conta gratuita em [netlify.com](https://netlify.com)
2. Instale a CLI do Netlify:
   ```bash
   npm install -g netlify-cli
   ```
3. Extraia o conteúdo do arquivo `retro-web-deploy.zip`
4. Navegue até a pasta extraída:
   ```bash
   cd retro-web-deploy
   ```
5. Instale as dependências:
   ```bash
   npm install
   ```
   ou
   ```bash
   pnpm install
   ```
6. Faça login na sua conta Netlify:
   ```bash
   netlify login
   ```
7. Crie um arquivo `netlify.toml` na raiz do projeto com o seguinte conteúdo:
   ```toml
   [build]
     command = "npm run build"
     publish = ".next"
   
   [[plugins]]
     package = "@netlify/plugin-nextjs"
   ```
8. Instale o plugin do Next.js para Netlify:
   ```bash
   npm install -D @netlify/plugin-nextjs
   ```
9. Implante a aplicação:
   ```bash
   netlify deploy --prod
   ```

Após a implantação, você receberá um URL no formato `retro-web.netlify.app` onde sua aplicação estará disponível.

## Opção 3: Implantação no RunHosting (VPS)

Para hospedar no RunHosting.com, você precisará de um plano VPS que suporte Node.js.

### Passos para Implantação em um VPS:

1. Contrate um plano VPS no RunHosting.com
2. Acesse seu servidor via SSH
3. Instale o Node.js e o NPM:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```
4. Instale o PM2 (gerenciador de processos para Node.js):
   ```bash
   npm install -g pm2
   ```
5. Faça upload do arquivo `retro-web-deploy.zip` para o servidor
6. Extraia o conteúdo:
   ```bash
   unzip retro-web-deploy.zip
   ```
7. Navegue até a pasta extraída:
   ```bash
   cd retro-web-deploy
   ```
8. Instale as dependências:
   ```bash
   npm install
   ```
9. Construa a aplicação:
   ```bash
   npm run build
   ```
10. Inicie a aplicação com PM2:
    ```bash
    pm2 start npm --name "retro-web" -- start
    ```
11. Configure o PM2 para iniciar automaticamente após reinicializações:
    ```bash
    pm2 startup
    pm2 save
    ```
12. Configure o Nginx como proxy reverso (assumindo que você já tenha o Nginx instalado):
    ```bash
    sudo nano /etc/nginx/sites-available/retro-web
    ```
    Adicione a seguinte configuração:
    ```nginx
    server {
        listen 80;
        server_name seu-dominio.com;
        
        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```
13. Ative a configuração e reinicie o Nginx:
    ```bash
    sudo ln -s /etc/nginx/sites-available/retro-web /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx
    ```

## Configuração do Banco de Dados

A aplicação utiliza o Cloudflare D1 como banco de dados. Para configurar:

1. Crie uma conta no Cloudflare (gratuita)
2. Instale o Wrangler CLI:
   ```bash
   npm install -g wrangler
   ```
3. Faça login no Cloudflare:
   ```bash
   wrangler login
   ```
4. Crie um banco de dados D1:
   ```bash
   wrangler d1 create retro-web-db
   ```
5. Atualize o arquivo `wrangler.toml` com o ID do banco de dados criado
6. Aplique as migrações:
   ```bash
   wrangler d1 migrations apply retro-web-db
   ```

## Suporte e Manutenção

Para qualquer dúvida ou problema durante a implantação, entre em contato. O código está bem documentado e organizado para facilitar futuras manutenções e atualizações.

---

Desenvolvido com ❤️ para você. Aproveite seu novo Portal Retro Web!
