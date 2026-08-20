# Processo do Projeto

Este documento descreve **como** o projeto foi construído e as principais decisões tomadas ao longo do caminho.

---

## Ferramentas Utilizadas

- **Agente de Código (Claude Code):** responsável por gerar toda a lógica da aplicação, testes e Dockerfiles.
- **Git + GitHub:** cada mudança importante foi feita em uma branch e revisada antes de ir para a versão principal.
- **GitHub Actions:** automatizou checagens de qualidade e testes a cada alteração.
- **Docker / docker-compose:** garantiu que o ambiente local fosse igual ao de produção.
- **Vercel CLI:** usado para realizar o deploy em produção.

---

## Problemas Encontrados

1. **Porta errada no deploy:** o container foi configurado para rodar na porta 8000, mas a Vercel esperava a 80. Isso causou falhas até ser corrigido.
2. **PDFs corrompidos ou mal editados:** alguns arquivos não eram aceitos corretamente no Git e outros apresentavam problemas de contexto, o que pode gerar instabilidade na aplicação.
3. **Nome de variável confuso:** uma variável chamada `l` foi rejeitada pelo sistema de lint, mostrando a importância da checagem automática.

> Em todos os casos, os erros só apareceram quando o sistema foi realmente executado — não apenas revisado.

---

## Experiência com a Vercel

Foi minha primeira vez lidando com a Vercel e tive algumas dificuldades em entender o fluxo da ferramenta e como ela lida com este tipo de aplicação:

- **Fluxo de deploy:** precisei aprender a diferença entre deploy de preview e de produção, além de configurar login e vínculo do projeto.
- **Configuração de runtime:** inicialmente procurei opções no dashboard, mas descobri que o `vercel.json` já definia o uso de container. Isso gerou confusão até compreender melhor a lógica da plataforma.

---

## Decisões Importantes

1. **Persistência em disco:** os dados temporários são salvos em arquivos locais, em vez de banco de dados, por simplicidade.
2. **Uso de `?` em caracteres incertos:** preferiu-se marcar dúvida em vez de arriscar valores errados.
3. **Fila interna de processamento:** optou-se por threads dentro do próprio processo, evitando a complexidade de sistemas externos de fila interna, esta,foi uma decisão consciente para simplificar o desenvolvimento e o deploy,
4.  mas traz riscos de instabilidade em ambientes distribuídos

---

## Pontos de Fragilidade/Pouca confiança na entrega

- **Fila interna + estado em disco:** se houver mais de uma instância rodando, pode ocorrer perda de dados temporários e falhas intermitentes.
- **Avisos de datas fora de sequência:** podem aparecer em excesso e acabar sendo ignorados.
- **Calibração do OCR:** foi feita apenas com exemplos sintéticos, sem testes em documentos reais.
- **Detecção de colunas:** depende de cabeçalhos reconhecidos; layouts diferentes podem causar erros silenciosos.
- **Arquivos PDF:** alguns podem não ser aceitos corretamente pela aplicação, o que pode gerar instabilidade.

---


