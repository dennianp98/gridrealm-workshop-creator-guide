# Guia de Criação de Edifícios para a Oficina

Bem-vindo, Criador! Obrigado pelo seu interesse em criar conteúdo personalizado! Sua contribuição ajudará a comunidade a prosperar! Este guia irá ajudá-lo a criar conteúdo que atenda às especificações e carregá-lo na Oficina Steam.

## Requisitos Técnicos
O carregamento de conteúdo depende de dois scripts: BuildingData e WorkshopUploadTool. BuildingData é usado para preencher os dados do edifício, e WorkshopUploadTool é usado para o carregamento. Você também precisa do Steamworks.NET. Se precisar atualizar o Steamworks.NET, pode baixá-lo da página do GitHub: https://github.com/rlabrecque/Steamworks.NET/releases, importá-lo para o Unity e anexar o script Steam Manager a um GameObject vazio.
Empacotei os scripts necessários e o Steamworks.NET em um arquivo de pacote Unity. Você pode usar este arquivo de pacote ou baixá-los separadamente: Link XX

### Especificações do Modelo de Edifício

> ⚠️ **Lembrete de Componentes Obrigatórios**: Os prefabs de edifício devem conter os três componentes principais a seguir. A falta de qualquer um deles causará mau funcionamento:
> - **Mesh Filter**: Exibe a malha do modelo
> - **Mesh Renderer**: Renderiza o modelo (requer atribuição de material)
> - **Collider** (Box Collider): Usado para destaque ao passar o mouse, detecção de clique e detecção de colisão de posicionamento

### Formatos de Recursos Suportados

- **Modelos**: FBX
- **Texturas**: PNG
- **Materiais**: Unity URP Lit Shader

### Limites de Tamanho de Arquivo

- AssetBundle de edifício único: ≤ 50 MB
- Imagem de visualização: ≤ 1 MB
- Tamanho total de download: ≤ 100 MB

## Diretrizes de Design de Edifícios

### 1. Especificações de Tamanho

Projete com base no tamanho da grade ocupado pelo edifício. O tamanho da base do edifício (fundação) deve ser de no mínimo 1 metro de comprimento e largura:
Para que a ocupação da grade do edifício seja calculada corretamente, recomenda-se usar quadrados ímpares para o tamanho da base do edifício, como: 1×1, 3×3, 5×5. Quadrados pares e formas não quadradas não são recomendados, como: 1×2, 2×2, 2×3, 4×4.
Altura: Sem limite estrito, mas recomenda-se não inferior a 0,1 metros

### 2. Estilo Visual

Para manter a consistência do jogo, recomenda-se:
- Usar estilo LowPoly
- Garantir as proporções gerais e relações de escala do edifício (referência de escala padrão do jogo: altura do personagem 0,1 metros)
- Usar designs amigáveis para vista isométrica
- Evitar detalhes excessivamente pequenos (difíceis de ver à distância)
- Usar contornos claros e contraste de cores
- Considerar os detalhes de diferentes ângulos

### 3. Recomendações de Otimização de Desempenho

- Controlar razoavelmente a contagem de polígonos
- Materiais transparentes (usar Alpha Test em vez de Alpha Blend)

## Processo de Criação

### Passo 1: Modelagem em Software 3D

1. Criar modelos usando Blender, Maya, 3ds Max, etc.
2. Garantir que a origem do modelo esteja no centro inferior
Usando Blender como exemplo:
✅ Correto: O ponto central do edifício está na origem mundial
((Imagem 1))
❌ Incorreto: O edifício está deslocado ou a rotação não está alinhada
((Imagem 2))
3. Aplicar todas as transformações (Scale, Rotation)
4. Exportar em formato FBX

### Passo 2: Importar para o Unity

1. Arrastar o FBX para o projeto Unity
2. Definir a Position do Transform do prefab do edifício como (0, 0, 0)
3. Garantir que a base do edifício esteja no plano Y=0
4. O edifício deve estar voltado para a direção positiva do eixo Z

### Passo 3: Configurar Materiais e Texturas

1. Criar materiais (usando URP Lit Shader)
2. Ajustar os parâmetros dos materiais

### Passo 4: Adicionar Componentes Obrigatórios

Garantir que o prefab do edifício contenha os três **componentes obrigatórios** a seguir:

#### 1. Mesh Filter e Mesh Renderer

1. Selecionar o objeto raiz do edifício
2. Confirmar que o componente **Mesh Filter** foi adicionado (se não, clicar em Add Component → Mesh Filter)
3. Confirmar que o componente **Mesh Renderer** foi adicionado
4. Atribuir materiais no Mesh Renderer (usar Standard ou URP Lit Shader)

> ⚠️ **Importante**: Edifícios sem Mesh Renderer não serão exibidos e não acionarão o efeito de destaque ao passar o mouse!

#### 2. Collider

1. Selecionar o objeto raiz do edifício
2. Adicionar um componente **Box Collider** (recomendado) ou **Mesh Collider**
3. Ajustar o tamanho do Collider para cobrir todo o edifício
4. Garantir que o Collider não se estenda além do alcance do edifício, pois isso causará colisão com outros edifícios e impedirá o posicionamento
((Imagem 3))

> ⚠️ **Importante**: Collider é uma condição necessária para o destaque ao passar o mouse e detecção de clique! Edifícios sem Collider não podem ser selecionados ou interagidos pelos jogadores.

### Passo 5: Criar Prefab

1. Arrastar o edifício para a janela Project para criar um Prefab
2. Convenção de nomenclatura: `{NomeEdificio}_Prefab`
3. Testar se o prefab pode ser instanciado normalmente

### Passo 6: Criar BuildingData

1. Clique direito → **Create → City Builder → Building Data**
2. Preencher os seguintes campos:

| **Building Name** | Nome de exibição do edifício, pode usar nome ou número | Ex.: Ferriswheel, SSmbl2 |
| **Building ID** | Identificador único, recomenda-se usar nome do estilo mais número, ou número puro | Ex.: SunsetCity_01, SSmbl2 |
| **Style** ⭐ | **Determina em qual aba de estilo o edifício aparece no menu de construção do jogo**, recomenda-se usar seu nome de desenvolvedor, nome do pacote ou nome do tema. Edifícios da mesma série devem ter o mesmo valor | Ex.: Lyon'sPack, SunsetCity |
| **Category** ⭐ | **Determina em qual coluna de categoria aparece sob esse estilo**, pode usar categorias integradas do jogo para compatibilidade (veja tabela abaixo), ou preencher de acordo com sua própria classificação de edifícios | Ex.: Large, Residential |
| **Grid Size** | Tamanho de ocupação da grade do edifício | Ex.: 3x3 |
| **Building Prefab** | Arraste seu prefab de edifício | — |

**Referência de Categorias Integradas do Jogo**:

| `Large` | Grande |
| `Medium` | Médio |
| `Small` | Pequeno |

> ⚠️ **Os campos Style e Category são muito importantes**: Eles serão empacotados em `metadata.json` (escrito automaticamente pela ferramenta de carregamento). Quando o jogo carrega seu mod, usa esses dois campos para determinar a posição do edifício no menu. Certifique-se de preenchê-los, caso contrário o edifício pode não ser categorizado e exibido corretamente.

3. Criar um ícone de edifício (PNG 128×128 recomendado), arrastar para o campo **Icon**

### Passo 7: Testar o Edifício

1. Testar o edifício no jogo:
   - Pode ser posicionado corretamente
   - A rotação funciona normalmente
   - A detecção de colisão está correta
   - Está satisfeito com o efeito visual
2. Ajustar até ficar satisfeito

### Passo 8: Preparar o Carregamento

- Criar uma imagem de visualização (600×600 recomendado)
- Confirmar que **Style** e **Category** no BuildingData estão corretamente preenchidos (a ferramenta de carregamento os lerá automaticamente)
Lista de verificação pré-carregamento:
   - [ ] A contagem de triângulos do modelo está dentro dos limites
   - [ ] A resolução das texturas é razoável
   - [ ] Contém o componente **Mesh Filter**
   - [ ] Contém o componente **Mesh Renderer** (material atribuído)
   - [ ] Contém o componente **Collider**
   - [ ] O prefab pode ser instanciado normalmente
   - [ ] O edifício está alinhado à grade
   - [ ] Não há materiais ou texturas ausentes
   - [ ] Todos os campos do BuildingData estão preenchidos
   - [ ] A imagem de visualização está nítida
   - [ ] Passou no teste no jogo

## Passo 9: Carregar para a Oficina

> ⚠️ **O carregamento é concluído em duas etapas**, pois o empacotamento do AssetBundle e o carregamento no Steam têm requisitos diferentes para o estado do editor, devendo ser feitos separadamente.

### Primeira Etapa: Empacotar Recursos (Modo Non-Play)

1. Garantir que está no **Modo Non-Play** (o editor não está em execução)
2. Abrir **City Builder → Workshop Upload Tool**
3. Selecionar tipo de mod: **Building**
4. Arrastar **BuildingData** (a ferramenta lerá automaticamente Style e Category, pode ser modificado manualmente)
5. Arrastar **prefab de edifício**
6. Preencher informações da oficina (principalmente o título, a descrição pode ser editada na Oficina)
7. Clicar em **"Primeira Etapa: Empacotar recursos como AssetBundle"**
8. Quando vir a mensagem **"Empacotamento concluído! Por favor entre no modo Play e clique na segunda etapa para carregar"**, foi bem-sucedido

> 💡 O caminho de empacotamento será salvo automaticamente e não será perdido após entrar no modo Play, não precisa reinserir as informações.

### Segunda Etapa: Carregar para o Steam (Modo Play)

1. Clicar no botão **Play** do editor Unity, aguardar a inicialização do SteamManager
2. Confirmar na janela da ferramenta que o Steam está inicializado (sem mensagens de aviso)
3. Clicar em **"Segunda Etapa: Carregar para a Oficina Steam"**
4. Aguardar a conclusão do carregamento, anotar o **ID do item da Oficina** exibido (para atualizações futuras)

> 💡 A ferramenta de carregamento gerará automaticamente `metadata.json` e o empacotará no diretório de conteúdo do AssetBundle, que contém informações de Style e Category. Após os jogadores se inscreverem, o jogo exibirá automaticamente na categoria correta.

### Passo 10: Publicar e Manter

1. Completar as informações na página da Oficina Steam
2. Adicionar mais capturas de tela

## Notas sobre Materiais de Céu e Chão
- Os shaders de céu e chão devem usar o pipeline URP
- Os shaders de chão preferencialmente devem usar normais procedurais para lidar com ajustes dinâmicos do tamanho da área da grade

## Erros Comuns e Soluções

### Erro 0: Erro de empacotamento "Building AssetBundles while in play mode is not allowed"

**Causa**: O botão de empacotamento foi clicado no modo Play, mas o empacotamento do AssetBundle só pode ser executado no modo Non-Play

**Solução**:
1. Sair do modo Play (clicar no botão de parar)
2. Clicar novamente em "Primeira Etapa: Empacotar recursos como AssetBundle" na ferramenta
3. Entrar no modo Play após a conclusão do empacotamento para carregar

### Erro 1: O Edifício É Exibido em Rosa

**Causa**: Material ou shader ausente

**Solução**:
1. Garantir que todas as texturas estão corretamente atribuídas
2. Verificar se os materiais estão incluídos no AssetBundle

### Erro 2: O Edifício Não Pode Ser Posicionado

**Causa**: Componentes obrigatórios ausentes (Mesh Filter/Mesh Renderer/Collider) ou erro na configuração do tamanho da grade

**Solução**:
1. Confirmar que o prefab do edifício contém os componentes **Mesh Filter**, **Mesh Renderer** e **Collider**
2. Verificar se o Mesh Renderer tem materiais atribuídos
3. Adicionar Box Collider ao objeto raiz do edifício
4. Verificar a configuração do Grid Size no BuildingData
5. Confirmar as dimensões do edifício

### Erro 3: Deslocamento da Posição do Edifício

**Causa**: As configurações do Transform do prefab estão incorretas

**Solução**:
1. Definir a Position do prefab como (0, 0, 0)
2. Garantir que a base do edifício está no plano Y=0
3. Verificar a posição do Pivot Point do modelo

### Erro 4: Deslocamento do Edifício Após Rotação

**Causa**: O ponto central do edifício não está no centro da grade

**Solução**:
1. Ajustar a origem do modelo no software 3D
2. Ou ajustar a posição dos objetos filhos no Unity
3. Garantir que o edifício gira em torno do ponto central

### Erro 5: Arquivo Muito Grande para Carregar

**Causa**: Resolução de textura muito alta ou modelo muito complexo

**Solução**:
1. Reduzir a resolução das texturas (ex.: de 4K para 2K)
2. Otimizar o modelo para reduzir a contagem de polígonos
3. Comprimir texturas (usar formato DXT5 ou BC7)
4. Remover detalhes desnecessários

## Técnicas Avançadas

### Adicionar Animações

Se seu edifício contém animações (como um moinho de vento giratório):
1. Criar um Animator Controller, arrastar a animação para o controlador
2. Adicionar um componente Animator ao prefab
3. Arrastar o Animator Controller para o componente Animator

### Usar o Sistema LOD

Anexar scripts de culling de detalhes para edifícios grandes:
1. Adicionar o script BuildingDetailCulling
2. Atribuir diferentes níveis LOD ao modelo
3. Definir distâncias de culling para diferentes níveis

### Efeitos de Material Personalizados

Usar Shader Graph para criar efeitos especiais:
1. Criar um asset Shader Graph
2. Projetar efeitos personalizados (como janelas brilhantes)
3. Criar um material que use esse shader
4. Garantir que o shader seja compatível com a plataforma alvo

## Conclusão

Obrigado por suas contribuições para a criação de conteúdo da comunidade. Espero que o jogo tenha uma vitalidade duradoura através do seu conteúdo!

Espero ver suas criações adicionar mais possibilidades coloridas ao jogo!

---

**Versão**: 1.0  
**Última Atualização**: 2026  
**Aplicável a**: Unity 2022.3 LTS
