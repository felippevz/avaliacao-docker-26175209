# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Felippe Vaz Pereira
Matrícula: 26175209
Usuário do GitHub: felippevz
Usuário do Docker Hub: felippevz

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?

   R: Imagem base usada: nginx:1.27-alpine,
      Tamanho final:
         Disk usage: 73.6MB
         Content size: 21MB

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.

   R: O Nginx procura as pastas em '/usr/share/nginx/html/'
      Comandos usados:
            docker run -d --name teste-portal -p 8009:80 nginx:1.27-alpine (iniciei a imagem do nginx padrão para ver de onde ele criaria o index.html) 
            docker exec teste-portal ls /usr/share/nginx (executei o comando para ver se existia as pastas padrões do nginx)
            docker exec teste-portal ls /usr/share/nginx/html (entrei na pasta que apareceu, que foi a html, pra confirmar que era a que tinha o index.html)

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.

   R: Nome da imagem: viaserra-portal
      Link: https://hub.docker.com/r/felippevz/viaserra-portal

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?

   R: Preciso reconstruir a imagem com 'docker build -t felippevz/viaserra-portal:1.0-26175209 ./portal' e depois enviar com 'docker push felippevz/viaserra-portal:1.0-26175209'. Como o Dockerfile copia o HTML para dentro da imagem, só rebuildando a imagem a mudança entra.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | COPY | a pasta de origem no COPY não existe no projeto | o erro de build, com a mensagem de que o caminho não foi encontrado (=> ERROR [3/3] COPY pagina/ . ) | troquei o nome da pasta de origem pelo nome correto |
| 2 | CMD | o Nginx era iniciado em segundo plano, então o processo principal terminava | Exited (0) em docker ps -a, e logs sem erro, só a inicialização normal | Troquei o comando simples (ngix) para o padrão da imagem (nginx -g daemon off;) |
| 3 | COPY / WORKDIR | Com WORKDIR /usr/share/nginx, o COPY site/ . colocava a página em /usr/share/nginx, e não em /usr/share/nginx/html, que é a pasta de onde o Nginx serve o site. | O container ficou Up, mas em http://localhost:7009 apareceu a página "Welcome to nginx!" em vez de "Voltamos em breve". | Troquei o destino do COPY para /usr/share/nginx/html/ |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

R: No -p, o formato é porta-do-host:porta-do-container. O número da esquerda é a porta do meu computador (host) e o da direita é a porta do container. A porta do container é, portanto, o número depois dos dois pontos.
   -p 7009:80: o que chega na porta 7009 do meu computador é encaminhado para a porta 80 do container, onde o Nginx escuta. Funciona: acesso http://localhost:7009.
   -p 80:7009: o que chega na porta 80 do meu computador é encaminhado para a porta 7009 do container. Como o Nginx escuta na 80 dentro do container, nada responderia na 7009, então a página não abriria.

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.

8. Qual comando derruba os dois containers de uma vez?

## Verificador

9. Código de conclusão impresso pelo verificador:

```
(cole aqui)
```
