# Pipeline Melhorada

Evolução da pipeline de CI, acrescentando o uso de artefatos entre os jobs de build e teste no GitHub Actions.

## Workflow: `artefato.yml`

Esse workflow (`.github/workflows/artefato.yml`) foi criado para conectar o job de **build** ao job de **test** através de um artefato compartilhado, em vez de cada job repetir o checkout do código.

### Job de Build

- **Gerar artefato ZIP** — compacta todo o código do repositório em `pipeline-demo.zip` (`zip -r pipeline-demo.zip .`).
- **Publicar artefato** — envia o ZIP gerado para o GitHub Actions com `actions/upload-artifact@v4`, sob o nome `codigo-buildado`.

### Job de Test

- **Baixar artefato** — recupera o artefato `codigo-buildado` publicado pelo job de build, usando `actions/download-artifact@v4`.
- **Extrair artefato** — descompacta o `pipeline-demo.zip` (`unzip pipeline-demo.zip`) para restaurar o código antes de rodar os testes.

## Por que usar artefatos?

Em vez de o job de teste fazer um novo checkout do repositório, ele reaproveita exatamente o código que passou pela etapa de build — garantindo que o que foi testado é o mesmo artefato gerado, e não uma nova cópia do repositório.
