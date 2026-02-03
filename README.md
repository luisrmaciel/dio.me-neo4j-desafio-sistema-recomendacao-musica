# Sistema de Recomendação de Músicas com Neo4j 🎵🚀

## 📝 Descrição do Desafio
O objetivo deste projeto é construir um sistema de recomendação de músicas utilizando grafos. O modelo conecta usuários, músicas, artistas e gêneros para gerar sugestões personalizadas baseadas em:
1.  **Filtragem Colaborativa:** O que usuários com gosto similar estão ouvindo.
2.  **Filtragem por Conteúdo:** Músicas do mesmo gênero que o usuário curtiu.

## 🖼️ Modelagem do Grafo
O banco de dados foi modelado com os seguintes nós e relacionamentos:
- **Nós:** `User`, `Song`, `Artist`, `Genre`
- **Relacionamentos:** 
    - `(:User)-[:LISTENED_TO {count: N}]->(:Song)`
    - `(:User)-[:LIKED]->(:Song)`
    - `(:Song)-[:PERFORMED_BY]->(:Artist)`
    - `(:Song)-[:BELONGS_TO]->(:Genre)`

## 🛠️ Scripts Cypher (Setup)

### 1. Inserção dos Dados
```cypher
// Criando a estrutura base
CREATE (rock:Genre {name: "Rock"})
CREATE (pop:Genre {name: "Pop"})
CREATE (queen:Artist {name: "Queen"})
CREATE (taylor:Artist {name: "Taylor Swift"})

CREATE (bohemian:Song {title: "Bohemian Rhapsody"})
CREATE (shake:Song {title: "Shake it Off"})

CREATE (bohemian)-[:PERFORMED_BY]->(queen), (bohemian)-[:BELONGS_TO]->(rock)
CREATE (shake)-[:PERFORMED_BY]->(taylor), (shake)-[:BELONGS_TO]->(pop)

CREATE (ana:User {name: "Ana"})
CREATE (bruno:User {name: "Bruno"})

CREATE (ana)-[:LIKED]->(bohemian)
CREATE (bruno)-[:LIKED]->(bohemian)
CREATE (bruno)-[:LIKED]->(shake)

🧠 Queries de Recomendação
1. Recomendação "Quem ouviu isso, também ouviu..."
Encontra músicas ouvidas por pessoas que têm gostos similares ao usuário alvo.
code
Cypher
MATCH (u:User {name: "Ana"})-[:LISTENED_TO]->(m:Song)<-[:LISTENED_TO]-(o:User)-[:LISTENED_TO]->(rec:Song)
WHERE NOT (u)-[:LISTENED_TO]->(rec)
RETURN rec.title AS Recomendacao, count(o) AS Forca
ORDER BY Forca DESC
2. Recomendação por Gênero Favorito
Sugere músicas do mesmo gênero das músicas que o usuário deu "Like".
code
Cypher
MATCH (u:User {name: "Ana"})-[:LIKED]->(m:Song)-[:BELONGS_TO]->(g:Genre)<-[:BELONGS_TO]-(rec:Song)
WHERE NOT (u)-[:LISTENED_TO]->(rec)
RETURN rec.title AS Recomendacao, g.name AS Genero
📊 Resultados Visuais
(Aqui você pode colocar um print do seu grafo completo rodando MATCH (n)-[r]->(m) RETURN n,r,m)
Projeto entregue para o Bootcamp de Data Analytics com Neo4j.
