# Revisão do simulador

## Alterações

- Neonatal (prática 13) identificado como **Brinde para os alunos**, bloqueado inicialmente e liberado no painel do professor.
- Catálogo antigo do Firestore é compatibilizado em memória. A liberação automática antiga do Neonatal é descartada até a primeira decisão do professor no novo fluxo; liberações posteriores são preservadas.
- Liberações salvas em transação, preservando alterações simultâneas.
- Sincronização do Firestore iniciada; função de persistência dos cadastros restaurada e erros dos listeners tratados.
- Quando as credenciais são exemplos, a aplicação tenta obter a configuração em `/__/firebase/init.json`, disponível no Firebase Hosting. Se não conseguir, informa o modo local.
- Referência do calcâneo atualizada para o arquivo JPEG existente.
- Progresso recalculado sem duplicar práticas; envio evita duplicar laudos que já chegaram pelo listener.
- Avaliação só informa sucesso depois da confirmação de gravação.
- Troca de incidência limpa as marcações da imagem anterior.
- Imagem indisponível é sinalizada e impede envio de exame sem imagem carregada.
- Alteração de código da turma acompanha os alunos; exclusão de turma com alunos é bloqueada.
- Protocolo, anamnese e feedback são escapados antes da inserção em HTML.

## Validação

Execute `node tests/regression.cjs`. Verifica catálogo, compatibilização do Neonatal, bloqueio de acesso, permissões de liberação na aplicação, progresso, falha de gravação e arquivos das imagens. Não acessa nem altera o banco de produção.

## Pendências para produção

1. Adicionar as radiografias autorizadas `imagens/neonatal_torax_abdomen.jpg` e `imagens/neonatal_perfil.jpg`. Estes arquivos não estão no repositório local. O módulo aparece no catálogo, mas o exame não pode ser enviado sem a imagem.
2. As coleções `sga_config` e `sga_submissions` foram confirmadas no console. A configuração pública real do aplicativo SGA Web foi incorporada. Os arquivos `firebase.json` e `.firebaserc` foram preparados para o projeto correto. O Hosting ainda apresenta o assistente inicial, sem site publicado. As regras do Firestore ainda precisam ser verificadas.
3. Implementar Firebase Authentication com identidade e perfil de professor validados por regras do Firestore. Atualmente o usuário escolhe o perfil e informa uma matrícula; as verificações JavaScript não garantem autorização no servidor. A senha do administrador fica no cliente e no documento acadêmico. Esse modelo exige substituição para uso com dados reais.
4. Verificar os demais campos de cadastro renderizados em HTML e a exportação CSV para entradas não confiáveis. A proteção dos campos de laudo corrigida aqui não constitui uma auditoria completa de segurança.
5. Publicar e validar o fluxo completo em produção. A exclusão/recriação do banco não foi executada. Há dois laudos visíveis na coleção antiga; o código agora converte os caminhos locais dessas imagens para caminhos relativos ao site.

As alterações de imagens e de `index.html` que já estavam no diretório foram preservadas.
