---
title: Configuração de instalação de aplicativos Clip-to-full
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Os donos/anunciantes podem atribuir downloads do usuário do aplicativo completo a partir do App Clip. Para fazer isso, o desenvolvedor precisa configurar tanto o App Clip quanto o aplicativo completo.
[block:api-header]
{
"title": "Procedures"
}
[/block]
Para configurar a atribuição de instalação de aplicativo Clip-to-full no App Clip e no app completo, a seguinte lista de procedimentos tem de ser completada.

**Importante**! Os mesmos passos precisam ser seguidos tanto no App Clip quanto no aplicativo completo.
[block:parameters]
{
"data": {
"h-0": "Verificação de procedimento",
"0-0": "1. No Clipe de Aplicativo, ative o Título dos Grupos de Aplicativos.",
"1-0": "2. No Clipe de Aplicativo, crie um Grupo de Aplicativos para passar dados entre o App Clip e o app completo.",
"2-0": 3. No Clipe de Aplicativo, atualize a Lista de Propriedades de Informação (arquivo `info.plist`).",
"3-0": "4. No aplicativo completo, repita os passos 1-3, usando o mesmo nome do Grupo de Aplicativos que o App Clip."
},
"cols": 1,
"rows": 4
}
[/block]
### Ativar a inscrição de grupos de aplicativos

**Para ativar o Título dos Grupos de Aplicativos**:

1. No Xcode, vá para sua página do Projeto.
2. Na seção **Alvos**, selecione o seu App Clip ou o alvo completo do aplicativo (dependendo do que você está configurando).
3. Clique na guia **Assinatura & Recursos**.
4. Click **+Capability**.
   O menu Recursos abre.
5. No menu, selecione grupos de aplicativos.

### Criar um Grupo de Aplicativos

**Para criar um Grupo de Aplicativos para transmissão de dados entre o App Clip e o app completo**:

1. No Xcode, vá para sua página do Projeto.
2. Na seção **Alvos**, selecione o seu App Clip ou o alvo completo do aplicativo (dependendo do que você está configurando).
3. Clique na guia **Assinatura & Recursos**.
4. Na guia **Assinar e capacidades**, vá para a seção **Grupos de Apps**.
5. Click **+**.
   A janela **Adicionar um novo contêiner** abre.
6. Na janela aberta, digite um nome para o seu Grupo de Aplicativos, com a seguinte convenção de nomeação:
   [block:code]
   {
   "codes": [
   {
   "code": "group. Nome. ppClipToFullApp",
   "language": "text"
   }
   ]
   }
   [/block]
### Exemplo
[block:image]
{
"images": [
{
"image": [
"https://files. eadme.io/cddf2e3-App_Group_Name.png",
"Nome do Grupo de Aplicativos. ng",
524,
177,
"#394148"
]
}
]
}
[/block]
### Atualizar a lista de propriedades da informação

**Para atualizar a Lista de Propriedade de Informação (arquivo `info.plist`)**:

- No arquivo **Propriedade de Informação** (arquivo `info.plist`), adicione uma linha com a chave e o valor detalhado na tabela a seguir.
  [block:parameters]
  {
  "data": {
  "h-0": "Chave",
  "h-1": "Tipo",
  "h-2": "Valor",
  "0-0": "AppsFlyerAppGroupName",
  "0-1": "String",
  "0-2": "O nome que você deu ao App Group no passo anterior."
  },
  "cols": 3,
  "rows": 1
  }
  [/block]
### Configurar o aplicativo completo

**Para configurar seu aplicativo completo**:

- Repetir todas as etapas anteriores para o aplicativo completo.
